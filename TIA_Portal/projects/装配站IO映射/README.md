# 装配站：IO 映射、自动流程与复位（S7-1200，TIA Portal V16）

Factory I/O 装配站仿真工程。16 点输入、12 点输出经 `FC_IO_Map` 折算进 `DB_IO`；`FB_Auto` 负责自动流程，`FB_Reset` 负责复位条件判定，`FB assemble` 完成基座/部件上料、机械臂真空拾取与 X/Z 气缸动作。

## 工程信息

| 项 | 内容 |
| --- | --- |
| 平台 | TIA Portal V16（`ProjectVersion` 16.0.0.0） |
| PLC | S7-1200（`PLC_1`），S7-PLCSIM 仿真 + Factory I/O 联调 |
| 编程 | 梯形图 / 函数块图（LAD、FBD） |
| 规模 | 42 个文件，约 2.1 MB |
| 最近修改 | 2026-10-01 |
| 来源 | 由 Factory I/O–PLCSIM 通信模板升级至 V16 |

## 程序块

| 块 | 类型 | 作用 |
| --- | --- | --- |
| `Main` | OB1 | 调用 `FC_IO_Map`、`FB_Auto`、`FB_Reset`、`assemble` |
| `FC_IO_Map` | FC | IO 映射：把 `%I`/`%Q` 整理进 `DB_IO`，由 `WriteOutputs` 引脚触发输出刷新 |
| `FB_Auto` | FB | 自动流程。输入 `Enable`、`AutoPermitRun`；静态 `mode`、`state`、`AutoStep_Lids`、`AutoStep_Bais` |
| `FB_Reset` | FB | 复位流程。`Reset_Premit`、`Reset_Enable`、`Reset_Finish`，输出 `ModeValid`、`AutoPermit` |
| `assemble` / `assemble_DB` | FB + 背景 DB | 装配动作。X/Z 轴（`F_Axis_X`、`F_Axis_Z`）、`F_LidsArrive`、`F_BaisArrive` 到位检测，配 `Timer0.5_1`–`Timer0.5_3`、`Timer1_4` 定时 |
| `DB_IO` | 全局 DB | IO 数据区：输入数组 `InSig`、输出数组 `OutCmd`、写输出触发 `WriteOutputs` |
| `MHJ-PLC-Lab-Function-S71200` | FC | 外部教学库函数（调用保留，非本项目编写） |
| `IEC_TIMER` | UDT | 定时器实例类型 |

## IO 分配

| 方向 | 地址 | 点数 |
| --- | --- | --- |
| 输入 | `%I0.0`–`%I0.7`、`%I1.0`–`%I1.7` | 16 |
| 输出 | `%Q0.0`–`%Q0.7`、`%Q1.0`–`%Q1.3` | 12 |
| 其他 | `%QD30` | 字输出 |

## 自动流程

程序内的状态文本按「待机 → 允许启动 → 自动生产 → 异常复位」组织：

| 阶段 | 程序中的文本 |
| --- | --- |
| 待机 | 待机，禁止生产循环 |
| 允许启动 | 自动，允许接受生产启动 |
| 生产中 | 正在自动生产 |
| 需要复位 | 初始位置未确认，或运行被中断，需要复位 |
| 复位中 | 正在执行自动复位动作 |
| 复位完成 | 复位完成，等待启动 |

模式分为 `自动模式` 与 `待机模式`：`FB_Auto` 用 `Enable` / `AutoPermitRun` 两个条件开门，`FB_Reset` 用 `Reset_Premit` / `Reset_Enable` 判断复位条件、`Reset_Finish` 给出完成。

## 信号与设备

| 分类 | 信号 |
| --- | --- |
| 传送带 | `基座传送带运行`、`部件传送带运行` |
| 机械臂 | `机械臂吸真空`、`部件吸住检测` |
| 气缸轴 | `轴气缸运行`、`轴气缸到位`、`轴运行中`、`轴极限位置检测` |
| 检测 | `基座到位`、`部件到位`、`读卡器/写卡器`、`产品计算检测`、`成品计数器` |
| 操作与指示 | `启动`、`停止`、`急停`、`复位`、`启动灯`、`停止灯`、`复位灯` |

用到的指令：`TON`、`R_TRIG`（1200/1500 版）、`F_TRIG`（1200/1500 版）、`Sequence`（脉冲发生器）。

## 实现要点

- **映射与逻辑分离**：`FC_IO_Map` 只做地址折算与输出刷新，工艺逻辑全部放在 FB 里，换现场只需改映射块与 `DB_IO` 对应关系。
- **自动与复位分离**：正常生产走 `FB_Auto`，异常走 `FB_Reset`，两者靠 `ModeValid` / `AutoPermit` 交接，复位不打断自动步序的状态保存。
- **到位确认**：`F_Axis_X`、`F_Axis_Z`、`F_LidsArrive`、`F_BaisArrive` 用 `F_TRIG` 取下降沿，再配 0.5 s / 1 s 多档定时器做延时确认，避免机械抖动误判。
- **步序记录**：`AutoStep_Lids`、`AutoStep_Bais` 分别记录两路动作步号，`state` 记录整机状态，便于在线监控时直接看步号定位。

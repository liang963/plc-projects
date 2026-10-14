# PLC 工程练习

本仓库按平台整理 PLC 练习：博图工程放在 `TIA_Portal/`，CODESYS 工程放在 `CODESYS/`。表里的项目名按程序内容命名；ZIP 解压后的博图工程仍保留原来的项目名。

## TIA Portal

| 项目 | 版本 | 程序内容 | 下载 |
| --- | --- | --- | --- |
| Factory I/O 起始工程 | V15 | 主程序块 `OB_Main2` | [工程 ZIP](TIA_Portal/projects/FactoryIO_Template_S7-1200_V15/FactoryIO_Template_S7-1200_V15.zip) |
| I/O 映射、自动与复位 | V16 | `FC_IO_Map`、`FB_Auto`、`FB_Reset` | [工程 ZIP](TIA_Portal/projects/FactoryIO_Template_S7-1200_V15_V16/FactoryIO_Template_S7-1200_V15_V16.zip) |
| 堆垛机随机存取 | V16 | `FB_WarreHouse`、`FB_AutoCtrl`、`FB_Inbound`、`Fb_Outbound`，含随机库位相关逻辑 | [工程 ZIP](TIA_Portal/projects/FactoryIO_Template_S7-1200_V15_V16_1/FactoryIO_Template_S7-1200_V15_V16_1.zip) |
| SCL 随机数生成练习 | V16 | `随机数` 程序块和 `随机数_DB` 数据块 | [工程 ZIP](<TIA_Portal/projects/生成随机数SCl/生成随机数SCl.zip>) |

堆垛机工程包含仓储、入库和出库相关程序；独立 SCL 工程用于单独查看随机数练习。两者分别保留，避免把算法练习和整机流程混在一起。

## CODESYS

| 项目 | 下载 |
| --- | --- |
| X-Z 双轴取放 | [liang.project](<CODESYS/CODESYS X Z 双轴取放/liang.project>) |

## 打开工程

博图工程 ZIP 解压后，请保留整个文件夹，并打开里面原名的 `.ap15` 或 `.ap16` 文件；同目录中的 `System` 等配套文件也需要保留。这里保存的是工程文件夹 ZIP，不是 TIA Portal 的 `.zap` 归档。

CODESYS 工程下载 `liang.project` 后即可在 CODESYS 中打开。个人 IDE 设置、缓存和仿真生成信息不放入仓库。

详细目录和历史备份见 [`TIA_Portal/README.md`](TIA_Portal/README.md)；归档文件校验信息见 [`catalog.json`](catalog.json)。

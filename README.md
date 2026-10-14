# PLC 工程练习

这个仓库保存 Factory I/O 练习用的 PLC 工程。博图项目放在 `TIA_Portal/`，CODESYS 项目放在 `CODESYS/`。

## 项目列表

| 平台 | 项目 | 下载入口 |
| --- | --- | --- |
| TIA Portal V15 | FactoryIO_Template_S7-1200_V15 | [完整工程 ZIP](TIA_Portal/projects/FactoryIO_Template_S7-1200_V15/FactoryIO_Template_S7-1200_V15.zip) |
| TIA Portal V16 | FactoryIO_Template_S7-1200_V15_V16 | [完整工程 ZIP](TIA_Portal/projects/FactoryIO_Template_S7-1200_V15_V16/FactoryIO_Template_S7-1200_V15_V16.zip) |
| TIA Portal V16 | FactoryIO_Template_S7-1200_V15_V16_1 | [完整工程 ZIP](TIA_Portal/projects/FactoryIO_Template_S7-1200_V15_V16_1/FactoryIO_Template_S7-1200_V15_V16_1.zip) |
| TIA Portal V16 | 生成随机数 SCL | [完整工程 ZIP](<TIA_Portal/projects/生成随机数SCl/生成随机数SCl.zip>) |
| CODESYS | liang | [CODESYS 工程](<CODESYS/CODESYS X Z 双轴取放/liang.project>) |

## 如何打开

### TIA Portal

下载对应的 ZIP，解压后打开里面的 `.ap15` 或 `.ap16` 文件。请保留解压出来的整个文件夹，工程还需要同目录中的 `System` 等配套文件。

这些 ZIP 是完整工程文件夹的压缩副本，不是 TIA Portal 自带的 `.zap` 归档。

### CODESYS

下载 `liang.project`，然后在 CODESYS 中打开。个人 IDE 设置、缓存和仿真生成信息由本机软件生成，仓库不保存这些文件。

## 文件夹

- [`TIA_Portal/projects/`](TIA_Portal/projects/)：四份博图工程。
- [`TIA_Portal/backups/`](TIA_Portal/backups/)：按项目和时间归档的博图备份。
- [`CODESYS/`](CODESYS/)：CODESYS 工程。
- [`catalog.json`](catalog.json)：已归档文件的清单和校验信息。
- [`GIT_HOOK.md`](GIT_HOOK.md)：提交前钩子的示例和启用步骤。

## 更新工程

在对应软件里修改并保存工程，然后更新本仓库对应项目的文件。`.gitignore` 会排除 IDE 设置、缓存和临时文件。

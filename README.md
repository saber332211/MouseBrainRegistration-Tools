# MouseBrainRegistration

MouseBrainRegistration 是一个面向小鼠全脑图像配准的 Windows 图形界面打包版本。它把 mBrainAligner 论文中的主要自动化流程封装到 Qt6 界面中，方便用户完成全脑图像预处理、全局配准、局部配准以及配准结果查看。

本项目主要面向大体积 3D 小鼠脑图像与标准图谱空间之间的配准，例如将实验采集的 whole-brain 图像映射到 Allen Common Coordinate Framework atlas / CCFv3 相关空间中。

## Download

完整可运行包体积较大，不建议直接提交到 GitHub 仓库。请从百度网盘下载打包文件：

- 百度网盘链接: https://pan.baidu.com/s/1gomlTv2KNbv0dzF1KEp5Cw
- 提取码: `ubw5`
- 分享文件名: `NEW`

下载后解压到任意英文路径或无特殊字符路径下，保持解压后的目录结构不变，然后运行：

```text
MouseBrainRegistration.exe
```

如果 Windows 提示缺少 DLL，可以尝试双击同目录下的：

```text
run.bat
```

## Background

该工具参考并封装了 mBrainAligner 的跨模态小鼠全脑配准流程。论文中提出的 mBrainAligner 使用 coherent landmark mapping 和深度神经网络特征来解决不同成像模态、不同样本制备方式导致的强烈形态和纹理差异问题，可用于将 fMOST、VISoR、MRI 等全脑数据配准到 Allen CCFv3 标准图谱空间。

论文中的整体流程包括：

1. 图像预处理：对图像尺寸、强度分布和条纹伪影进行处理。
2. 自动全局配准：基于脑外轮廓点集匹配，估计整体方向、尺度和仿射变换。
3. 自动局部配准：使用 CLM（Coherent Landmark Mapping）进行非线性局部形变配准。
4. 可选半自动修正：在局部区域进一步检查和微调配准质量。

当前打包版主要提供预处理、全局配准、局部配准、2D/3D 查看和融合显示等界面化操作，具体功能以本程序界面为准。

## Quick Start

1. 下载并解压完整运行包。
2. 双击 `MouseBrainRegistration.exe`。
3. 在菜单或界面中加载 Moving Image 和 Fixed Image。
4. 如需要，先点击预处理按钮生成预处理后的 moving 图像。
5. 点击全局配准按钮，程序会优先使用预处理结果；如果没有预处理结果，也可以直接使用原始 moving 图像进行全局配准。
6. 全局配准完成后，选择局部配准所需 marker 文件，再点击局部配准。
7. 在 Data Manager 和 Blend View 中查看 fixed、moving、global/local result 的配准效果。

## Package Layout

运行包需要保持如下相对目录结构：

```text
MouseBrainRegistration.exe
run.bat
data/
data/data_resut/
data/marker4/
tools/bin/global/CPU/release/GlobalRegistration_LYT.exe
tools/bin/local/local_hhm/CPU/release/local_registration_LYT.exe
tools/bin/local/config/config.txt
tools/Stps_warp_image_portable_x64/Stps_warp_image.exe
```

程序会以 `MouseBrainRegistration.exe` 所在目录作为运行根目录，并使用相对路径查找 `data/` 和 `tools/`。请不要把 exe 单独移动到其他文件夹中运行。
data/25um_568提供CCF标准脑，方便您直接配准，C1.v3draw_processed.v3draw为C1严重伪影图像方便您测试条纹伪影预处理，data/25um_pad为dti成像模态鼠脑图像，您可以测试其与CCF配准，注意局部配准请选用模式1

## Main Outputs

默认输出目录为：

```text
data/data_resut/
```

常见输出文件包括：

```text
C1.v3draw_processed_preprocess.v3draw
global.v3draw
local_registered_image.v3draw
```

其中：

- `C1.v3draw_processed_preprocess.v3draw` 是预处理后的 moving 图像。
- `global.v3draw` 是全局配准结果。
- `local_registered_image.v3draw` 是局部配准结果。

## Notes

- 推荐将运行包解压到磁盘空间充足的位置，大体积全脑数据和中间结果会占用较多空间。
- 如果配准失败，请优先检查输入图像、marker 文件、`data/` 目录和 `tools/` 目录是否完整。
- 如果在另一台电脑上运行失败，请确认没有只复制 exe，而是复制了完整解压后的运行包。
- 如果路径包含特殊字符导致外部配准程序启动失败，建议换到较短的英文路径下运行。

## Citation

如果该工具或 mBrainAligner 方法对你的工作有帮助，请引用相关论文：

Qu, L., Li, Y., Xie, P., et al. Cross-modality coherent registration of whole mouse brains. Nature Methods, 2021.

Published version DOI:

```text
https://doi.org/10.1038/s41592-021-01334-w
```

Preprint DOI:

```text
https://doi.org/10.21203/rs.3.rs-321118/v1
```

Original mBrainAligner source code is available from the Vaa3D tools repository:

```text
https://github.com/Vaa3D/vaa3d_tools/tree/master/hackathon/mBrainAligner
```

# 基于深度学习的图像超分辨率重建及其在医学影像上的应用

<p align="center"><img src="./Introduction.png"></img></p>

这是我的本科毕业设计。

## 组成部分

- **前言**

  介绍图像超分辨率问题、研究现状、前景，介绍在医学图像上进行超分辨率的重要性。

- **自然图像上的超分辨率研究**

  在 DIV2K 数据集（800 train + 100 val）进行实验。选取 baseline 模型为 ESPCN、DWSR、EDSR。针对这些模型的不足之处，提出改进：使用小波 + U-Net + 感知损失多任务学习的 LU-MWCNN模型，达到超越 baseline 的效果。

- **医学图像上的超分辨率应用**

  在 DeepLesion 数据集（CT 图像）的 Key_slices 上进行实验，同样与 baseline 模型进行对比。提出 CT-LPIPS，利用一个类 VGG 网络训练。

- **医学图像超分辨率平台开发**

  以 CT 图像为例，搭建 Web 服务，借助 Cornerstone.js 库，医生可预览 DICOM，或将图像发送至后端重建服务，以获得超分辨完成的结果。后端采用 Flask + PyTorch 进行部署和实时推理。

- **总结**

## 目录结构

### baselines / 基线模型

- 修改 `config/common.py`，选择数据集和输出路径。目前提供的数据集接口有
  - div2k.DIV2KDataset
  - deeplesion.DeepLesionDataset

- 训练

  ```bash
  $ cd script
  $ chmod 755 *
  $ ./train_dwsr.sh div2k # ./train_<model>.sh <data>
  # 修改脚本可以指定 GPU 编号
  ```

- 验证结果、权重、TensorBoard 日志会输出在 `output_<data>/<model>`下

### experiment / 小型实验与绘图

- bicubic-learnable：揭示双三次插值劣于可学习过程上采样。
- calc-ct-lpips：CT-LPIPS的集中计算。
- curve-merge：曲线绘制与原始数据。
- deeplesion-sample：DeepLesion效果取样。
- div2k-sample：DIV2K效果取样。
- haar-wavelet：哈尔小波变换样例。
- param-count：参数量统计。
- ycbcr：YCbCr色彩空间示意。

### nova / 改进与创新

- lu_mwcnn

  小波 U-Net + 多任务学习，超越 baseline。

  ```bash
  $ chmod 755 *.sh
  $ ./trian_div2k.sh # ./train_deeplesion.sh
  ```

- ct_lpips

  CT 图的感知损失（使用轻量化的类 VGG 网络）。

  ```bash
  $ chmod 755 *.sh
  $ ./trian.sh
  ```

- ablation

  消融实验相关。

  ```bash
  $ chmod 755 *.sh
  $ ./train_div2k.sh
  $ ./test_manual.sh
  ```

### paper / 文本材料

```
.
├── README.md
├── material       ~~~ 流程材料
├── thesis         ~~~ 毕业论文
└── translation    ~~~ 外文文献翻译
```

### platform / 平台开发

- frontend

  超分辨率平台前端。

  ```bash
  $ npm install
  # 配置 /ct-sr/script/config.js
  $ npm run dev # development
  $ npm run pack # production
  ```

- backend

  超分辨率平台后端。

  ```bash
  $ python main.py
  ```

### requirement / 环境说明

包括服务器（训练、部署）和本地机器（小型实验、客户端）的 Anaconda 库配置和 pip 库配置。

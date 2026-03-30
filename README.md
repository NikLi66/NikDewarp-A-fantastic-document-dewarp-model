<p align="right">
  English | <a href="./README.zh.md">中文</a>
</p>

# Introduction
This repository provides training and inference code for a document rectification model based on Control Points, with a series of improvements proposed by the author.

The training data format is largely consistent with Control Points, consisting of input images and their corresponding control points. It is recommended to refer to:
- [Control Points](https://github.com/gwxie/Document-Dewarping-with-Control-Points)
- [Data synthesis method](https://github.com/gwxie/Synthesize-Distorted-Image-and-Its-Control-Points)

Below is a comparison between the results of this method and a commercial product:

| ![](1.png) | ![](2.png) | ![](3.png) |
|:---:|:---:|:---:|
| Input Image | Commercial Product | Our Result |

As shown above, some commercial solutions may lose boundary information (e.g., titles and scores) during rectification. In contrast, due to improved data processing, our method preserves all content in the image. This is highly beneficial for downstream tasks such as OCR and layout analysis.

# Contributions
The main contributions of this work are:

- Improved the original data synthesis method. The original method generates only 32×32 control points, while the new method produces dense pixel-level correspondences, leading to significant performance gains.
- Introduced a novel (“fancy”) data processing strategy that ensures every input pixel is preserved in the output image.
- Enhanced the loss function by adding constraints (e.g., higher weights for specific regions) and fine-tuned the original loss design.
- Optimized the model architecture by replacing the traditional UNet with a Restormer-based structure, achieving notable improvements.

# Discussion
Overall, this method uses a relatively simple model (UNet also performs well in practice), combined with an advanced data processing pipeline and a redesigned data synthesis strategy. These improvements significantly enhance the quality of training data, which in turn leads to better model performance.

From empirical analysis, the biggest improvement comes from better data quality. The choice of model (Restormer, a commonly used architecture in 2024 for image processing) plays a secondary role. The loss modifications mainly correct issues in the original Control Points method.

The redesigned data preprocessing pipeline is the key factor enabling the model to preserve all information. However, in terms of overall performance, its contribution is still smaller than that of improved training data quality.

# Tips
## Training Data Format

- A `.lst` file is used, where each line contains the path to a `.gw` file.
- Data loading in the current implementation may be slow.
- It is recommended to implement multi-process data loading for better performance.

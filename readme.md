<h1 align="center">This is the modification of LivePortrait: Efficient Portrait Animation with Stitching and Retargeting Control for allowing video as a source </h1>

<div align='center'>
    <a href='https://github.com/cleardusk' target='_blank'><strong>Jianzhu Guo</strong></a><sup> 1†</sup>&emsp;
    <a href='https://github.com/KwaiVGI' target='_blank'><strong>Dingyun Zhang</strong></a><sup> 1,2</sup>&emsp;
    <a href='https://github.com/KwaiVGI' target='_blank'><strong>Xiaoqiang Liu</strong></a><sup> 1</sup>&emsp;
    <a href='https://scholar.google.com/citations?user=t88nyvsAAAAJ&hl' target='_blank'><strong>Zhizhou Zhong</strong></a><sup> 1,3</sup>&emsp;
    <a href='https://scholar.google.com.hk/citations?user=_8k1ubAAAAAJ' target='_blank'><strong>Yuan Zhang</strong></a><sup> 1</sup>&emsp;
</div>

<div align='center'>
    <a href='https://scholar.google.com/citations?user=P6MraaYAAAAJ' target='_blank'><strong>Pengfei Wan</strong></a><sup> 1</sup>&emsp;
    <a href='https://openreview.net/profile?id=~Di_ZHANG3' target='_blank'><strong>Di Zhang</strong></a><sup> 1</sup>&emsp;
</div>

<div align='center'>
    <sup>1 </sup>Kuaishou Technology&emsp; <sup>2 </sup>University of Science and Technology of China&emsp; <sup>3 </sup>Fudan University&emsp;
</div>

<br>
<div align="center">
  <!-- <a href='LICENSE'><img src='https://img.shields.io/badge/license-MIT-yellow'></a> -->
  <a href='https://arxiv.org/pdf/2407.03168'><img src='https://img.shields.io/badge/arXiv-LivePortrait-red'></a>
  <a href='https://liveportrait.github.io'><img src='https://img.shields.io/badge/Project-LivePortrait-green'></a>
  <a href ='https://github.com/KwaiVGI/LivePortrait'>official liveportrait</a>a
  <a href='https://huggingface.co/spaces/KwaiVGI/liveportrait'><img src='https://img.shields.io/badge/%F0%9F%A4%97%20Hugging%20Face-Spaces-blue'></a>
</div>
<br>

<p align="center">
  <img src="./assets/docs/showcase2.gif" alt="showcase">
  <br>
  🔥 For more results, visit our <a href="https://liveportrait.github.io/"><strong>homepage</strong></a> 🔥
</p>


## 🔥 Getting Started
### 1. Clone the code and prepare the environment
```bash
git clone https://github.com/KwaiVGI/LivePortrait
cd LivePortrait

# create env using conda
conda create -n LivePortrait python==3.9.18
conda activate LivePortrait
# install dependencies with pip
pip install -r requirements.txt
```

### 2. Download pretrained weights

Download the pretrained weights from HuggingFace:
```bash
# you may need to run `git lfs install` first
git clone https://huggingface.co/KwaiVGI/liveportrait pretrained_weights
```

Or, download all pretrained weights from [Google Drive](https://drive.google.com/drive/folders/1UtKgzKjFAOmZkhNK-OYT0caJ_w2XAnib) or [Baidu Yun](https://pan.baidu.com/s/1MGctWmNla_vZxDbEp2Dtzw?pwd=z5cn). We have packed all weights in one directory 😊. Unzip and place them in `./pretrained_weights` ensuring the directory structure is as follows:
```text
pretrained_weights
├── insightface
│   └── models
│       └── buffalo_l
│           ├── 2d106det.onnx
│           └── det_10g.onnx
└── liveportrait
    ├── base_models
    │   ├── appearance_feature_extractor.pth
    │   ├── motion_extractor.pth
    │   ├── spade_generator.pth
    │   └── warping_module.pth
    ├── landmark.onnx
    └── retargeting_models
        └── stitching_retargeting_module.pth
```

### 3. Inference 🚀

#### Fast hands-on
```bash
python inference.py
```

If the script runs successfully, you will get an output mp4 file named `animations/s6--d0_concat.mp4`. This file includes the following results: driving video, input image, and generated result.

<p align="center">
  <img src="./assets/docs/inference.gif" alt="image">
</p>

Or, you can change the input by specifying the `-s` and `-d` arguments:

```bash
python inference.py -s assets/examples/source/s9.jpg -d assets/examples/driving/d0.mp4

# disable pasting back to run faster
python inference.py -s assets/examples/source/s9.jpg -d assets/examples/driving/d0.mp4 --no_flag_pasteback

# more options to see
python inference.py -h
```

For video: you can change the input by specifying the `-sd` and `-d` arguments:

```bash
python inference.py -sd assets/examples/driving/d3.mp4 -d assets/examples/driving/d0.mp4 -vd True

# disable pasting back to run faster
python inference.py -sd assets/examples/driving/d3.mp4 -d assets/examples/driving/d0.mp4 -vd True --no_flag_pasteback

```
#### Driving video auto-cropping

📕 To use your own driving video, we **recommend**:
 - Crop it to a **1:1** aspect ratio (e.g., 512x512 or 256x256 pixels), or enable auto-cropping by `--flag_crop_driving_video`.
 - Focus on the head area, similar to the example videos.
 - Minimize shoulder movement.
 - Make sure the first frame of driving video is a frontal face with **neutral expression**.

Below is a auto-cropping case by `--flag_crop_driving_video`:
```bash
python inference.py -s assets/examples/source/s9.jpg -d assets/examples/driving/d13.mp4 --flag_crop_driving_video
```

If you find the results of auto-cropping is not well, you can modify the `--scale_crop_video`, `--vy_ratio_crop_video` options to adjust the scale and offset, or do it manually.

#### Template making
You can also use the `.pkl` file auto-generated to speed up the inference, and **protect privacy**, such as:
```bash
python inference.py -s assets/examples/source/s9.jpg -d assets/examples/driving/d5.pkl
```

**Discover more interesting results on our [Homepage](https://liveportrait.github.io)** 😊

### 4. Gradio interface 🤗

We also provide a Gradio interface for a better experience, just run by:

```bash
python app.py
```

You can specify the `--server_port`, `--share`, `--server_name` arguments to satisfy your needs!

**Or, try it out effortlessly on [HuggingFace](https://huggingface.co/spaces/KwaiVGI/LivePortrait) 🤗**

### 5. Inference speed evaluation 🚀🚀🚀
We have also provided a script to evaluate the inference speed of each module:

```bash
python speed.py
```



## Community Resources 🤗

Discover the invaluable resources contributed by our community to enhance your LivePortrait experience:

- [ComfyUI-LivePortraitKJ](https://github.com/kijai/ComfyUI-LivePortraitKJ) by [@kijai](https://github.com/kijai)
- [comfyui-liveportrait](https://github.com/shadowcz007/comfyui-liveportrait) by [@shadowcz007](https://github.com/shadowcz007)
- [LivePortrait hands-on tutorial](https://www.youtube.com/watch?v=uyjSTAOY7yI) by [@AI Search](https://www.youtube.com/@theAIsearch)
- [ComfyUI tutorial](https://www.youtube.com/watch?v=8-IcDDmiUMM) by [@Sebastian Kamph](https://www.youtube.com/@sebastiankamph)
- [LivePortrait In ComfyUI](https://www.youtube.com/watch?v=aFcS31OWMjE) by [@Benji](https://www.youtube.com/@TheFutureThinker)
- [Replicate Playground](https://replicate.com/fofr/live-portrait) and [cog-comfyui](https://github.com/fofr/cog-comfyui) by [@fofr](https://github.com/fofr)

And many more amazing contributions from our community!

## Acknowledgements
We would like to thank the contributors of [FOMM](https://github.com/AliaksandrSiarohin/first-order-model), [Open Facevid2vid](https://github.com/zhanglonghao1992/One-Shot_Free-View_Neural_Talking_Head_Synthesis), [SPADE](https://github.com/NVlabs/SPADE), [InsightFace](https://github.com/deepinsight/insightface) repositories, for their open research and contributions.

## Citation 💖
If you find LivePortrait useful for your research, welcome to 🌟 this repo and cite our work using the following BibTeX:
```bibtex
@article{guo2024liveportrait,
  title   = {LivePortrait: Efficient Portrait Animation with Stitching and Retargeting Control},
  author  = {Guo, Jianzhu and Zhang, Dingyun and Liu, Xiaoqiang and Zhong, Zhizhou and Zhang, Yuan and Wan, Pengfei and Zhang, Di},
  journal = {arXiv preprint arXiv:2407.03168},
  year    = {2024}
}
```

<p align="center">
    <img src="assets/logo_color.png" height=180>
</p>

# 😴 LucidDreamer: Domain-free Generation of 3D Gaussian Splatting Scenes 😴

<div align="center">

[![Project](https://img.shields.io/badge/Project-LucidDreamer-green)](https://luciddreamer-cvlab.github.io/)
[![ArXiv](https://img.shields.io/badge/Arxiv-2311.13384-red)](https://arxiv.org/abs/2311.13384)
[![Github](https://img.shields.io/github/stars/luciddreamer-cvlab/LucidDreamer)](https://github.com/luciddreamer-cvlab/LucidDreamer)
[![X](https://img.shields.io/twitter/url?label=_ironjr_&url=https%3A%2F%2Ftwitter.com%2F_ironjr_)](https://twitter.com/_ironjr_)
[![LICENSE](https://img.shields.io/badge/license-CC--BY--NC--SA--4.0-lightgrey)](https://github.com/luciddreamer-cvlab/LucidDreamer/blob/master/LICENSE)
[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/camenduru/LucidDreamer-Gaussian-colab/blob/main/LucidDreamer_Gaussian_colab.ipynb)
[![Open in Spaces](https://huggingface.co/datasets/huggingface/badges/raw/main/open-in-hf-spaces-sm.svg)](https://huggingface.co/spaces/ironjr/LucidDreamer-mini)

</div>

https://github.com/luciddreamer-cvlab/LucidDreamer/assets/12259041/35004aaa-dffc-4133-b15a-05224e68b91e


> #### [LucidDreamer: Domain-free Generation of 3D Gaussian Splatting Scenes](https://arxiv.org/abs/2311.13384)
> ##### \*[Jaeyoung Chung](https://robot0321.github.io/), \*[Suyoung Lee](https://esw0116.github.io/), [Hyeongjin Nam](https://hygenie1228.github.io/), [Jaerin Lee](http://jaerinlee.com/), [Kyoung Mu Lee](https://cv.snu.ac.kr/index.php/~kmlee/)
###### \*Denotes equal contribution.

<p align="center">
    <img src="assets/logo_cvlab.png" height=60>
</p>

---


## ⚡ Usage

We offer several ways to interact with LucidDreamer:

1. A demo is available on [`ironjr/LucidDreamer` HuggingFace Space](https://huggingface.co/spaces/ironjr/LucidDreamer) (including custom SD ckpt) and [`ironjr/LucidDreamer-mini` HuggingFace Space](https://huggingface.co/spaces/ironjr/LucidDreamer-mini) (minimal features / try at here in case of the former is down)
(We appreciate all the HF / Gradio team for their support).

https://github.com/luciddreamer-cvlab/LucidDreamer/assets/12259041/745bfc46-8215-4db2-80d5-4825e91316bc

2. Another demo is available on a [Colab](https://colab.research.google.com/github/camenduru/LucidDreamer-Gaussian-colab/blob/main/LucidDreamer_Gaussian_colab.ipynb), implemented by [@camenduru](https://github.com/camenduru)
(We greatly thank [@camenduru](https://github.com/camenduru) for the contribution).
3. You can use the gradio demo locally by running [`CUDA_VISIBLE_DEVICES=0 python app.py`](app.py) (full feature including huggingface model download, requires ~15GB) or [`CUDA_VISIBLE_DEVICES=0 python app_mini.py`](app_mini.py) (minimum viable demo, uses only SD1.5).
4. You can also run this with command line interface as described below.

### Prerequisite

- CUDA>=11.4 (higher version is OK).
- Python==3.9 (cannot use 3.10 due to open3d compatibility)

### Install
#### Ubuntu
```bash
conda create -n lucid python=3.9
conda activate lucid
pip install peft diffusers scipy numpy imageio[ffmpeg] opencv-python Pillow open3d torchvision gradio
# ZoeDepth
pip install torch==2.0.1 timm==0.6.7
# Gaussian splatting
pip install plyfile==0.8.1

cd submodules/depth-diff-gaussian-rasterization-min
# sudo apt-get install libglm-dev # may be required for the compilation.
python setup.py install
cd ../simple-knn
python setup.py install
cd ../..
```

#### Windows (Experimental, Tested on Windows 11 with VS2022)
**Note: Windows installation is experimental and may contain errors during installation. Please report the error to issues if occurs.**
- CUDA of the same version as the cudatoolkit used when installing pytorch must be installed in your system. Here we assume that CUDA version is 11.8.
- C++ (>=14) should be installed in your system. If not download it using the [Visual Studio build tools](https://visualstudio.microsoft.com/downloads/).
  
```bash
conda create -n lucid python=3.9
conda activate lucid
conda install pytorch==2.0.1 torchvision==0.15.2 torchaudio==2.0.2 pytorch-cuda=11.8 -c pytorch -c nvidia
pip install peft diffusers scipy numpy imageio[ffmpeg] opencv-python Pillow open3d gradio
# ZoeDepth
pip install timm==0.6.7
# Gaussian splatting
pip install plyfile==0.8.1

cd submodules\depth-diff-gaussian-rasterization-min\third_party
git clone https://github.com/g-truc/glm.git
cd ..\
python setup.py install
cd ..\simple-knn
python setup.py install
cd ..\..
```

### Run with your own samples

```bash
# Default Example
python run.py
``` 

To run with your own inputs and prompts, attach following arguments after ``run.py``.

- ``-img`` : path of input image.
- ``-t`` : text prompt. Can be either path to txt file or the text itself.
- ``-nt`` : negative text prompt. Can be either path to txt file or the text itself.
- ``-cg`` : camera extrinsic path for generating scenes. Can be one of "Rotate_360", "LookAround", or "LookDown".
- ``-cr`` : camera extrinsic path for rendering videos. Can be one of "Back_and_forth", "LLFF", or "Headbanging".
- ``--seed`` : manual seed for Stable Diffusion inpainting.
- ``--diff_steps`` : number of denoising steps for Stable Diffusion inpainting. Default is 50.
- ``-s`` : path to save results. 

### Visualize `.ply` files

There are multiple available viewers / editors for Gaussian splatting `.ply` files.

1. [@playcanvas](https://github.com/playcanvas)'s [Super-Splat](https://github.com/playcanvas/super-splat) project ([Live demo](https://playcanvas.com/super-splat)). This is the viewer we have used for our debugging along with MeshLab.

![image](https://github.com/luciddreamer-cvlab/LucidDreamer/assets/12259041/89c4b5dd-c66f-4ad2-b1be-e5f951273049)

2. [@antimatter15](https://github.com/antimatter15)'s [WebGL viewer](https://github.com/antimatter15/splat) for Gaussian splatting ([Live demo](https://antimatter15.com/splat/)).

3. [@splinetool](https://github.com/splinetool)'s [web-based viewer](https://spline.design/) for Gaussian splatting. This is the version we have used in our project page's demo.

## 🚩 **Updates**

- ✅ December 8, 2023: [HuggingFace Space demo](https://huggingface.co/spaces/ironjr/LucidDreamer) is out. We deeply thank all the HF team for their support!
- ✅ December 7, 2023: [Colab](https://colab.research.google.com/github/camenduru/LucidDreamer-Gaussian-colab/blob/main/LucidDreamer_Gaussian_colab.ipynb) implementation is now available thanks to [@camenduru](https://github.com/camenduru)!
- ✅ December 6, 2023: Code release!
- ✅ November 22, 2023: We have released our paper, LucidDreamer on [arXiv](https://arxiv.org/abs/2311.13384).

## 🌏 Citation

Please cite us if you find our project useful!

```latex
@article{chung2023luciddreamer,
    title={LucidDreamer: Domain-free Generation of 3D Gaussian Splatting Scenes},
    author={Chung, Jaeyoung and Lee, Suyoung and Nam, Hyeongjin and Lee, Jaerin and Lee, Kyoung Mu},
    journal={arXiv preprint arXiv:2311.13384},
    year={2023}
}
```

## 🤗 Acknowledgement

We deeply appreciate [ZoeDepth](https://github.com/isl-org/ZoeDepth), [Stability AI](), and [Runway](https://huggingface.co/runwayml/stable-diffusion-v1-5) for their models.

## 📧 Contact

If you have any questions, please email `robot0321@snu.ac.kr`, `esw0116@snu.ac.kr`, `jarin.lee@gmail.com`.

## ⭐ Star History

<a href="https://star-history.com/#luciddreamer-cvlab/LucidDreamer&Date">
  <picture>
    <source media="(prefers-color-scheme: dark)" srcset="https://api.star-history.com/svg?repos=luciddreamer-cvlab/LucidDreamer&type=Date&theme=dark" />
    <source media="(prefers-color-scheme: light)" srcset="https://api.star-history.com/svg?repos=luciddreamer-cvlab/LucidDreamer&type=Date" />
    <img alt="Star History Chart" src="https://api.star-history.com/svg?repos=luciddreamer-cvlab/LucidDreamer&type=Date" />
  </picture>
</a>

## Era3D: High-Resolution Multiview Diffusion using Efficient Row-wise Attention

This is the official implementation of *Era3D: High-Resolution Multiview Diffusion using Efficient Row-wise Attention*.

### [Project Page](https://penghtyx.github.io/Era3D_page/) | [Arxiv](https://arxiv.org/pdf/2405.11616) | [Weights](https://huggingface.co/pengHTYX/MacLab-Era3D-512-6view/tree/main) | <a href="https://huggingface.co/spaces/pengHTYX/Era3D_MV_demo"><img src="https://img.shields.io/badge/%F0%9F%A4%97%20Gradio%20Demo-Huggingface-orange"></a>
<video width="700" height="360" controls>
  <source src="assets/teaser.mp4" type="video/mp4">
  您的浏览器不支持 video 标签。
</video>

![Teaser](assets/teaser.jpg)

### Installation
```
conda create -n Era3D python=3.9
conda activate Era3D

# torch
pip install torch==2.1.2 torchvision==0.16.2 torchaudio==2.1.2 --index-url https://download.pytorch.org/whl/cu118

# install xformers, download from https://download.pytorch.org/whl/cu118
pip install xformers-0.0.23.post1-cp39-cp39-manylinux2014_x86_64.whl 

# for reconstruciton
pip install git+https://github.com/NVlabs/tiny-cuda-nn/#subdirectory=bindings/torch
pip install git+https://github.com/NVlabs/nvdiffrast

# other depedency
pip install -r requirement.txt
```

### Weights
You can directly download the model from [huggingface](https://huggingface.co/spaces/pengHTYX/Era3D_MV_demo). You also can download the model in python script:
```
from huggingface_hub import snapshot_download
snapshot_download(repo_id="pengHTYX/MacLab-Era3D-512-6view")
```

### Inference
1. we generate multivew color and normal images by running [test_mvdiffusion_unclip.py](test_mvdiffusion_unclip.py). For example,
```
python test_mvdiffusion_unclip.py --config configs/test_unclip-512-6view.yaml \
    pretrained_model_name_or_path='pengHTYX/MacLab-Era3D-512-6view' \
    validation_dataset.crop_size='420' \
    validation_dataset.root_dir=examples \
    seed=600 \
    save_dir='mv_res'  \
    save_mode='rgb'
``` 
You can adjust the ```crop_size``` (400 or 420) and ```seed``` (42 or 600) to obtain best results for some cases. 

2. Typically, we use ```rembg``` to predict alpha channel. If it has artifact, try to use [Clipdrop](https://github.com/xxlong0/Wonder3D?tab=readme-ov-file) to remove the background.

3. Instant-NSR Mesh Extraction
```
cd instant-nsr-pl
bash run.sh $GPU $CASE $OUTPUT_DIR
```
For example, 
```
bash run.sh 0 A_bulldog_with_a_black_pirate_hat_rgba  recon
```
The textured mesh will be saved in $OUTPUT_DIR.


### Related projects
We collect code from following projects. We thanks for the contributions from the open-source community!     
[diffusers](https://github.com/huggingface/diffusers)  
[Wonder3D](https://github.com/xxlong0/Wonder3D?tab=readme-ov-file)  
[Syncdreamer](https://github.com/liuyuan-pal/SyncDreamer)  
[Instant-nsr-pl](https://github.com/bennyguo/instant-nsr-pl)  


### Citation
If you find this codebase useful, please consider cite our work.
```
@article{li2024era3d,
  title={Era3D: High-Resolution Multiview Diffusion using Efficient Row-wise Attention},
  author={Li, Peng and Liu, Yuan and Long, Xiaoxiao and Zhang, Feihu and Lin, Cheng and Li, Mengfei and Qi, Xingqun and Zhang, Shanghang and Luo, Wenhan and Tan, Ping and others},
  journal={arXiv preprint arXiv:2405.11616},
  year={2024}
}
```
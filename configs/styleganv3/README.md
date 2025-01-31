# StyleGANv3

> [Alias-Free Generative Adversarial Networks](https://nvlabs-fi-cdn.nvidia.com/stylegan3/stylegan3-paper.pdf)

<!-- [ALGORITHM] -->

## Abstract
We observe that despite their hierarchical convolutional nature, the synthesis
process of typical generative adversarial networks depends on absolute pixel coordinates in an unhealthy manner. This manifests itself as, e.g., detail appearing to
be glued to image coordinates instead of the surfaces of depicted objects. We trace
the root cause to careless signal processing that causes aliasing in the generator
network. Interpreting all signals in the network as continuous, we derive generally
applicable, small architectural changes that guarantee that unwanted information
cannot leak into the hierarchical synthesis process. The resulting networks match
the FID of StyleGAN2 but differ dramatically in their internal representations, and
they are fully equivariant to translation and rotation even at subpixel scales. Our
results pave the way for generative models better suited for video and animation.


<!-- [IMAGE] -->
<div align=center>
<img src="https://user-images.githubusercontent.com/22982797/150353023-8f7eeaea-8783-4ed4-98d5-67a226e00cff.png"/>
</div>



## Results and Models

<div align="center">
  <b> Results (compressed) from StyleGAN3 config-T converted by MMGeneration</b>
  <br/>
  <img src="https://user-images.githubusercontent.com/22982797/150450502-c182834f-796f-4397-bd38-df1efe4a8a47.png" width="800"/>
</div>

|                Model                |  Dataset  | Comment     | FID50k |    EQ-T     | EQ-R     |                                                            Config                                                             |                                                                 Download                                                                 |
| :---------------------------------: | :-------------: |:-------------: | :----: | :-----------: | :-----------: |:---------------------------------------------------------------------------------------------------------------------------: | :--------------------------------------------------------------------------------------------------------------------------------------: |
|    stylegan3_config-T    |afhqv2 512x512 |official weight | 4.04 | 60.15 | 13.51   |  [config](configs/styleganv3/stylegan3_t_afhqv2_512_b4x8_official.py)       |  [model](https://download.openmmlab.com/mmgen/stylegan3/stylegan3_t_afhqv2_512_b4x8_cvt_official.pkl)  |
|    stylegan3_config-T    | ffhqu 256x256|official weight | 4.62 | 63.01 | 13.12   |  [config](configs/styleganv3/stylegan3_t_ffhqu_256_b4x8_official.py)       |  [model](https://download.openmmlab.com/mmgen/stylegan3/stylegan3_t_ffhqu_256_b4x8_cvt_official.pkl)  |
|    stylegan3_config-R     | afhqv2 512x512 |official weight |4.40    |64.89  | 40.34   |  [config](configs/styleganv3/stylegan3_r_afhqv2_512_b4x8_official.py)       |  [model](https://download.openmmlab.com/mmgen/stylegan3/stylegan3_r_afhqv2_512_b4x8_cvt_official.pkl)  |
|    stylegan3_config-R    | ffhqu 256x256 |official weight |  4.50| 66.65 |  40.48  |  [config](configs/styleganv3/stylegan3_r_ffhqu_256_b4x8_official.py)       |  [model](https://download.openmmlab.com/mmgen/stylegan3/stylegan3_r_ffhqu_256_b4x8_cvt_official.pkl)  |



## Interpolation
We provide a tool to generate video by walking through GAN's latent space.
Run this command to get the following video.
```bash
python apps/interpolate_sample.py configs/styleganv3/stylegan3_t_afhqv2_512_b4x8_official.py https://download.openmmlab.com/mmgen/stylegan3/stylegan3_t_afhqv2_512_b4x8_cvt_official.pkl --export-video --samples-path work_dirs/demos/ --endpoint 6 --interval 60 --space z --seed 2022 --sample-cfg truncation=0.8
```
https://user-images.githubusercontent.com/22982797/151506918-83da9ee3-0d63-4c5b-ad53-a41562b92075.mp4

## Equivarience Visualization && Evaluation

We also provide a tool to visualize the equivarience properties for StyleGAN3.
Run these commands to get the results below.

```bash
python tools/utils/equivariance_viz.py configs/styleganv3/stylegan3_r_ffhqu_256_b4x8_official.py https://download.openmmlab.com/mmgen/stylegan3/stylegan3_r_ffhqu_256_b4x8_cvt_official.pkl --translate_max 0.5 --transform rotate --seed 5432

python tools/utils/equivariance_viz.py configs/styleganv3/stylegan3_r_ffhqu_256_b4x8_official.py https://openmmlab-share.oss-cn-hangzhou.aliyuncs.com/mmgen/stylegan3/stylegan3_r_ffhqu_256_b4x8_cvt_official.pkl --translate_max 0.25 --transform x_t --seed 5432

python tools/utils/equivariance_viz.py configs/styleganv3/stylegan3_r_ffhqu_256_b4x8_official.py https://openmmlab-share.oss-cn-hangzhou.aliyuncs.com/mmgen/stylegan3/stylegan3_r_ffhqu_256_b4x8_cvt_official.pkl --translate_max 0.25 --transform y_t --seed 5432
```


https://user-images.githubusercontent.com/22982797/151504902-f3cbfef5-9014-4607-bbe1-deaf48ec6d55.mp4


https://user-images.githubusercontent.com/22982797/151504973-b96e1639-861d-434b-9d7c-411ebd4a653f.mp4


https://user-images.githubusercontent.com/22982797/151505099-cde4999e-aab1-42d4-a458-3bb069db3d32.mp4

If you want to EQ-Metric for StyleGAN3, just add following codes into config.
```python
metrics = dict(
    eqv=dict(
        type='Equivariance',
        num_images=50000,
        eq_cfg=dict(
            compute_eqt_int=True, compute_eqt_frac=True, compute_eqr=True)))
```
And we highly recommend you to use [slurm_eval_multi_gpu](tools/slurm_eval_multi_gpu.sh) script to accelerate evaluation time.


## Citation

```latex
@inproceedings{Karras2021,
  author = {Tero Karras and Miika Aittala and Samuli Laine and Erik H\"ark\"onen and Janne Hellsten and Jaakko Lehtinen and Timo Aila},
  title = {Alias-Free Generative Adversarial Networks},
  booktitle = {Proc. NeurIPS},
  year = {2021}
}
```

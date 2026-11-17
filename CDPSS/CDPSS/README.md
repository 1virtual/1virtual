# CDPSS
Code repository for "Robust Multi-Party Generative Steganography via Compensated Diffusion Sampling with Dual-Prompt Guidance and Secret Sharing" by Shuhao Feng and Jialiang Peng.

The proposed framework integrates endpoint residual compensation diffusion sampling (ERC-DPM), dual-prompt guidance, Blakley threshold secret sharing, reversible latent mapping, and bit-level consensus for robust multi-party secret recovery.

## Download Weights

The two models used in our paper:
+ Stable Diffusion v1-5: https://drive.google.com/file/d/1ISeumzN-JrhyAacOPlh1IjAv6MKffogU/view?usp=sharing
+ CLIP text encoder: https://huggingface.co/openai/clip-vit-large-patch14/tree/main

## Run Test Scripts
### Step 1: Modify the config file `configs/stable-diffusion-ldm/LDM.yaml`
> Line 72: 
> 
> version: /weight/path/to/clip

Change the path on line 72 to the local path of CLIP.

### Step 2: Run
> cd test
> 
> python main.py --ckpt /weight/path/to/stable/diffusion/xxx.ckpt --config ../configs/stable-diffusion/ldm.yaml --dpm_steps 20 --dpm_order 2 --scale 5. --test_prompts /path/to/test_prompt_file/test_prompts.txt --attack_layer storage --attack_factor 0.01 --mapping_func ours_mapping

For example, the following command tests secret extraction accuracy after PNG storage under different prompts (taken from test_prompts.txt):
> cd test
> 
> python main.py --ckpt /weight/path/to/stable/diffusion/xxx.ckpt --config ../configs/stable-diffusion/ldm.yaml --dpm_steps 20 --dpm_order 2 --scale 5. --test_prompts ./test_prompts.txt --attack_layer storage --attack_factor 0.01 --mapping_func ours_mapping

The currently supported attack scenarios are as follows:

|Attack Scenario|attack_layer|attack_factor|
|-------|-------|-------|
|Lossless|identity|-|
|PNG|storage|-|
|Resize|resize|0.5, 0.75, 1.25, 1.5|
|JPEG Compression|jpeg|90, 70, 50|
|Median Blur|mblur|3, 5, 7|
|Gaussian Blur|gblur|3, 5, 7|
|Gaussian Noise|awgn|0.01, 0.05, 0.1|

When the attack layer is mblur or gblur, the "factor" refers to the size of the kernel, where a factor of 3 corresponds to a 3x3 kernel.

## Notes
There may be unused code paths, including those related to image resolution. If you intend to use a different generative model, please modify the relevant parts accordingly. The commands above retain the original script interface; verify that the uploaded implementation supports the proposed multi-party mapping, ERC-DPM, dual-prompt guidance, and voting mechanisms before using them to reproduce the paper.


---
layout: post
---

As of March 2024, Sagemaker studio notebooks use `conda-forge` and come pre-installed with CUDA 11.2 which was released in 2020. While AWS may think that CUDA 11.2 is all we need, most of the newer LoRA, QLoRA, and other quanitzation-based libraries like `accelerate` and `bitesnadbytes` don't support it.
To top it off, Sagemaker makes it super-confusing to make any changes. After multiple hair-pulling hours, I was able to get the notebook use CUDA 11.8 using the following bash snippet. Good luck.


```sh
#!/bin/bash

pip uninstall torch
pip uninstall torch # yes, you need to run it twice
pip uninstall torchvision
conda remove pytorch-gpu
pip3 install torch torchvision torchaudio --index-url https://download.pytorch.org/whl/cu118
export LD_LIBRARY_PATH=/usr/local/nvidia/lib:/usr/local/nvidia/lib64:/usr/local/cuda-11.8/lib64/
conda list | grep torch
```

Check the build, it should now have `cu118` within it.

```
agemaker-user@default:~$ conda list | grep torch
pytorch-lightning         2.0.9              pyhd8ed1ab_0    conda-forge
pytorch-metric-learning   1.7.3              pyhd8ed1ab_0    conda-forge
torch                     2.2.1+cu118              pypi_0    pypi
torchaudio                2.2.1+cu118              pypi_0    pypi
torchmetrics              1.0.3              pyhd8ed1ab_0    conda-forge
torchvision               0.17.1+cu118             pypi_0    pypi
```

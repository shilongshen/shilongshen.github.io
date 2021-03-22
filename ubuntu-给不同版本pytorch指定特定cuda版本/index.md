# 给不同版本Pytorch指定特定CUDA版本


[参考](https://blog.csdn.net/u012387402/article/details/104023073)

在服务器上安装不同版本的pytorch

先创建一个虚拟环境

```bash
conda create -n pytorch1.0 python=3.7
```

切换到该虚拟环境(这样就不会影响原有的环境了)

```bash
conda activate pytorch1.0
```



方法一：

```bash
conda install pytorch=1.0.0 torchvision -c pytorch
```

这样就会安装

```bash
Collecting package metadata (current_repodata.json): done
Solving environment: done

Please update conda by running

    $ conda update -n base -c defaults conda

## Package Plan ##

  environment location: /home/wag/anaconda3/envs/pytorch1.0

  added / updated specs:
    - pytorch=1.0.0
    - torchvision


The following NEW packages will be INSTALLED:

  blas               anaconda/pkgs/main/linux-64::blas-1.0-mkl
  cffi               anaconda/pkgs/main/linux-64::cffi-1.14.4-py37h261ae71_0
  freetype           anaconda/pkgs/main/linux-64::freetype-2.10.4-h5ab3b9f_0
  intel-openmp       anaconda/pkgs/main/linux-64::intel-openmp-2020.2-254
  jpeg               anaconda/pkgs/main/linux-64::jpeg-9b-h024ee3a_2
  lcms2              anaconda/pkgs/main/linux-64::lcms2-2.11-h396b838_0
  libpng             anaconda/pkgs/main/linux-64::libpng-1.6.37-hbc83047_0
  libtiff            anaconda/pkgs/main/linux-64::libtiff-4.1.0-h2733197_1
  lz4-c              anaconda/pkgs/main/linux-64::lz4-c-1.9.2-heb0550a_3
  mkl                anaconda/pkgs/main/linux-64::mkl-2020.2-256
  mkl-service        anaconda/pkgs/main/linux-64::mkl-service-2.3.0-py37he8ac12f_0
  mkl_fft            anaconda/pkgs/main/linux-64::mkl_fft-1.2.0-py37h23d657b_0
  mkl_random         anaconda/pkgs/main/linux-64::mkl_random-1.1.1-py37h0573a6f_0
  ninja              anaconda/pkgs/main/linux-64::ninja-1.10.2-py37hff7bd54_0
  numpy              anaconda/pkgs/main/linux-64::numpy-1.19.2-py37h54aff64_0
  numpy-base         anaconda/pkgs/main/linux-64::numpy-base-1.19.2-py37hfa32c7d_0
  olefile            anaconda/pkgs/main/linux-64::olefile-0.46-py37_0
  pillow             anaconda/pkgs/main/linux-64::pillow-8.0.1-py37he98fc37_0
  pycparser          anaconda/pkgs/main/noarch::pycparser-2.20-py_2
  pytorch            pytorch/linux-64::pytorch-1.0.0-py3.7_cuda9.0.176_cudnn7.4.1_1
  six                anaconda/pkgs/main/linux-64::six-1.15.0-py37h06a4308_0
  torchvision        pytorch/noarch::torchvision-0.2.2-py_3
  zstd               anaconda/pkgs/main/linux-64::zstd-1.4.5-h9ceee32_0
```

注意

```bash
#此命令会安装
pytorch            pytorch/linux-64::pytorch-1.0.0-py3.7_cuda9.0.176_cudnn7.4.1_1
```

所以会直接安装pytorch1.0.0

这个命令也会直接将安装cuda9.0.176和cudnn7.4.1_1（具体的原因不清楚），所以后面也不用重新安装与pytorch对应的cuda版本了。

注：这种方法是通过conda安装的不完整CUDA

**常用指令：**

```bash
检查PyTorch版本

    torch.__version__ # PyTorch version
    
    torch.cuda.is_available()#是否能够使用cuda加速

    torch.version.cuda # Corresponding CUDA version

    torch.backends.cudnn.version() # Corresponding cuDNN version

    torch.cuda.get_device_name(0) # GPU type

```

---

更新：采用上述方法似乎有点问题，报错

```
RuntimeError: cuda runtime error (11) : invalid argument at /opt/conda/conda-bld/pytorch_1544176307774/work/aten/src/THC/THCGeneral.cpp:405
```





```bash
vi ~/.bashrc

source ~/.bashrc
```



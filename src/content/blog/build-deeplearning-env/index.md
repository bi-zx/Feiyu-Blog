---
title: '深度学习环境搭建'
description: '-'
publishDate: '2024-09-05'
heroImage: { src: './thumbnail.jpg', color: '#64574D' }
tags: ['tutorial']
language: '中文'
comment: true
---

## CUDA cuDNN安装

1. 下载对应版本CUDA，cuDNN，安装CUDA
2. 在cmd中输入`nvcc -V` ，返回CUDA版本则说明安装成功
3. 安装时若遇到“You already have a newer version of the NVIDIA Frameview SDK installed”，先把电脑已经存在的FrameView SDK 卸载掉，把C:\Program Files\NVIDIA Corporation\FrameViewSDK文件夹删掉
4. 将cuDNN解压，bin, include, lib文件夹复制到C:\Program Files\NVIDIA GPU Computing Toolkit\CUDA\vxx.x目录下并覆盖

## TensorFlow安装

CUDA 12.6	TensorFlow 2.6.0

```shell
conda create -n tf26 python=3.8.18
activate tf26

conda install cudatoolkit=11.2.0 cudnn=8.1.0.77
pip install tensorflow-gpu==2.6.0 keras==2.6.0 protobuf==3.20.0 -i https://pypi.tuna.tsinghua.edu.cn/simple/
conda install numpy=1.19.5 matplotlib=3.3.4 scipy scikit-learn pandas
```

CUDA 12.6	TensorFlow 2.10.0

```shell
conda create -n tf210 python=3.8.18
activate tf210

conda install cudatoolkit=11.3 cudnn=8.2.1
pip install tensorflow-gpu==2.10.0 keras==2.10.0 -i https://pypi.tuna.tsinghua.edu.cn/simple/
conda install numpy=1.22 matplotlib=3.7.2 scipy scikit-learn pandas
pip install opencv-python==4.4.0.44 tqdm imutils PyYAML tensorboard seaborn chardet -i https://pypi.tuna.tsinghua.edu.cn/simple/
```

检测TensorFlow能否使用GPU

```python
import tensorflow as tf
tf.config.list_physical_devices('GPU')
```

## PyTorch安装

CUDA 11.8	PyTorch2.3.1

```shell
conda create -n torch2 python=3.8.19
activate torch2

conda install pytorch==2.3.1 torchvision==0.18.1 torchaudio==2.3.1 pytorch-cuda=11.8 -c pytorch -c nvidia
```

检测PyTorch能否使用GPU

```python
import torch
print(torch.cuda.is_available())
print(torch.cuda.device_count())
```



## Tested build configurations on Windows

### GPU

| Version               | Python version | Compiler           | Build tools         | cuDNN | CUDA |
| :-------------------- | :------------- | :----------------- | :------------------ | :---- | :--- |
| tensorflow_gpu-2.10.0 | 3.7-3.10       | MSVC 2019          | Bazel 5.1.1         | 8.1   | 11.2 |
| tensorflow_gpu-2.9.0  | 3.7-3.10       | MSVC 2019          | Bazel 5.0.0         | 8.1   | 11.2 |
| tensorflow_gpu-2.8.0  | 3.7-3.10       | MSVC 2019          | Bazel 4.2.1         | 8.1   | 11.2 |
| tensorflow_gpu-2.7.0  | 3.7-3.9        | MSVC 2019          | Bazel 3.7.2         | 8.1   | 11.2 |
| tensorflow_gpu-2.6.0  | 3.6-3.9        | MSVC 2019          | Bazel 3.7.2         | 8.1   | 11.2 |
| tensorflow_gpu-2.5.0  | 3.6-3.9        | MSVC 2019          | Bazel 3.7.2         | 8.1   | 11.2 |
| tensorflow_gpu-2.4.0  | 3.6-3.8        | MSVC 2019          | Bazel 3.1.0         | 8.0   | 11.0 |
| tensorflow_gpu-2.3.0  | 3.5-3.8        | MSVC 2019          | Bazel 3.1.0         | 7.6   | 10.1 |
| tensorflow_gpu-2.2.0  | 3.5-3.8        | MSVC 2019          | Bazel 2.0.0         | 7.6   | 10.1 |
| tensorflow_gpu-2.1.0  | 3.5-3.7        | MSVC 2019          | Bazel 0.27.1-0.29.1 | 7.6   | 10.1 |
| tensorflow_gpu-2.0.0  | 3.5-3.7        | MSVC 2017          | Bazel 0.26.1        | 7.4   | 10   |
| tensorflow_gpu-1.15.0 | 3.5-3.7        | MSVC 2017          | Bazel 0.26.1        | 7.4   | 10   |
| tensorflow_gpu-1.14.0 | 3.5-3.7        | MSVC 2017          | Bazel 0.24.1-0.25.2 | 7.4   | 10   |
| tensorflow_gpu-1.13.0 | 3.5-3.7        | MSVC 2015 update 3 | Bazel 0.19.0-0.21.0 | 7.4   | 10   |
| tensorflow_gpu-1.12.0 | 3.5-3.6        | MSVC 2015 update 3 | Bazel 0.15.0        | 7.2   | 9.0  |
| tensorflow_gpu-1.11.0 | 3.5-3.6        | MSVC 2015 update 3 | Bazel 0.15.0        | 7     | 9    |
| tensorflow_gpu-1.10.0 | 3.5-3.6        | MSVC 2015 update 3 | Cmake v3.6.3        | 7     | 9    |
| tensorflow_gpu-1.9.0  | 3.5-3.6        | MSVC 2015 update 3 | Cmake v3.6.3        | 7     | 9    |
| tensorflow_gpu-1.8.0  | 3.5-3.6        | MSVC 2015 update 3 | Cmake v3.6.3        | 7     | 9    |
| tensorflow_gpu-1.7.0  | 3.5-3.6        | MSVC 2015 update 3 | Cmake v3.6.3        | 7     | 9    |
| tensorflow_gpu-1.6.0  | 3.5-3.6        | MSVC 2015 update 3 | Cmake v3.6.3        | 7     | 9    |
| tensorflow_gpu-1.5.0  | 3.5-3.6        | MSVC 2015 update 3 | Cmake v3.6.3        | 7     | 9    |
| tensorflow_gpu-1.4.0  | 3.5-3.6        | MSVC 2015 update 3 | Cmake v3.6.3        | 6     | 8    |
| tensorflow_gpu-1.3.0  | 3.5-3.6        | MSVC 2015 update 3 | Cmake v3.6.3        | 6     | 8    |
| tensorflow_gpu-1.2.0  | 3.5-3.6        | MSVC 2015 update 3 | Cmake v3.6.3        | 5.1   | 8    |
| tensorflow_gpu-1.1.0  | 3.5            | MSVC 2015 update 3 | Cmake v3.6.3        | 5.1   | 8    |
| tensorflow_gpu-1.0.0  | 3.5            | MSVC 2015 update 3 | Cmake v3.6.3        | 5.1   | 8    |


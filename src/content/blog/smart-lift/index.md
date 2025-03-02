---
title: '开发环境配置'
description: '-'
publishDate: '2025-02-24'
updatedDate: '2025-02-24'
tags: ['tutorial']
language: '中文'
comment: true
---


使用conda创建一个名为`GAN`的虚拟环境，Python版本为3.7.12

```shell
conda create -n GAN python=3.7.12
```

激活该虚拟环境

```shell
activate GAN
```

安装依赖

```shell
pip install paddlepaddle==1.8.5 parl==1.4 gym==0.18.0 atari-py==0.2.6 rlschool==0.3.1 protobuf==3.19.6 metagym==0.1.1 matplotlib==3.3.4 numpy==1.19.5 scikit-learn==0.24.2 make_env==0.0.7 pyglet==1.5.0 mock==3.0.5 -i https://mirrors.aliyun.com/pypi/simple
```

GPU CUDA10.1

```shell
pip install paddlepaddle-gpu==1.8.5.post107 parl==1.4 gym==0.18.0 atari-py==0.2.6 rlschool==0.3.1 protobuf==3.19.6 metagym==0.1.1 matplotlib==3.3.4 numpy==1.19.5 scikit-learn==0.24.2 make_env==0.0.7 pyglet==1.5.0 mock==3.0.5 -i https://mirrors.aliyun.com/pypi/simple
```

GPU CUDA12.0 Latest

```shell
conda create -n test python=3.9
activate test

python -m pip install paddlepaddle-gpu==2.5.2.post120 -f https://www.paddlepaddle.org.cn/whl/windows/mkl/avx/stable.html
pip install numpy==1.23.5 parl==2.2.1 gym==0.26.2 pygame -i https://mirrors.aliyun.com/pypi/simple
```

在Pycharm IDE中配置该虚拟环境，运行实例代码：

`LiftSim_example.py`

```python
from rlschool import make_env

env = make_env('LiftSim')
observation = env.reset()
action = [2, 0, 4, 0, 7, 0, 10, 0]
for i in range(100):
    env.render()    # use render to show animation
    next_obs, reward, done, info = env.step(action)
```

可尝试进行训练：

```shell
xparl start --port 8010
python xxx/LiftSim_baseline/A2C/train.py
```

> 参考链接：
>
> [电梯调度算法大赛_飞桨大赛-飞桨AI Studio星河社区](https://aistudio.baidu.com/competition/detail/11/0/introduction)
>
> [LiftSim_baseline - 飞桨AI Studio星河社区](https://aistudio.baidu.com/projectdetail/100632)


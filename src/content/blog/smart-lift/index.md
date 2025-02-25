---
title: 'conda虚拟环境配置'
description: '-'
publishDate: '2025-02-24'
updatedDate: '2025-02-24'
tags: ['tutorial']
language: '中文'
comment: true
---

# conda虚拟环境配置

创建一个名为`LiftSim`的虚拟环境，Python版本为3.7.12

```shell
conda create -n LiftSim python=3.7.12
```

激活该虚拟环境

```shell
activate LiftSim
```

安装依赖

```shell
python -m pip install pip==23.3.2 -i https://mirrors.aliyun.com/pypi/simple
pip install setuptools==57.5.0 wheel==0.37.0 -i https://mirrors.aliyun.com/pypi/simple

pip install rlschool==0.3.2 gym==0.18.0 metagym==0.1.1 matplotlib==3.3.4 numpy==1.20.3 scikit-learn==0.24.2 make_env==0.0.7 pyglet==1.5.0 mock==3.0.5 -i https://mirrors.aliyun.com/pypi/simple

pip install parl==1.3.1 protobuf==3.19.0 -i https://mirrors.aliyun.com/pypi/simple
```

在Pycharm IDE中配置该虚拟环境，运行实例代码：

`LiftSim_example.py`

```python
import gym
import metagym.liftsim

env = gym.make('liftsim-v0')
observation = env.reset()
action = [2, 0, 4, 0, 7, 0, 10, 0]
for i in range(100):
    env.render()    # use render to show animation
    next_obs, reward, done, info = env.step(action)
```

出现电梯动画即为安装成功

之后研究代码，可尝试训练(可能需自行安装依赖)

可参考链接：

[电梯调度算法大赛_飞桨大赛-飞桨AI Studio星河社区](https://aistudio.baidu.com/competition/detail/11/0/introduction)

[LiftSim_baseline - 飞桨AI Studio星河社区](https://aistudio.baidu.com/projectdetail/100632)



WSL:

```
conda create -n GAN python=3.6.15
conda activate GAN
pip install paddlepaddle==1.8.5 parl==1.4 gym==0.18.0 atari-py==0.2.6 rlschool==0.3.1 -i https://mirrors.aliyun.com/pypi/simple
```

```
conda create -n GAN python=3.6.15
conda activate GAN
pip install paddlepaddle-gpu==1.8.5.post97 parl==1.4 gym==0.18.0 atari-py==0.2.6 rlschool==0.3.1 -i https://mirrors.aliyun.com/pypi/simple
```



```
#rlschool可用与gym可用
conda create -n LiftSim python=3.7.12
activate LiftSim

python -m pip install pip==23.3.2 -i https://mirrors.aliyun.com/pypi/simple
pip install setuptools==57.5.0 wheel==0.37.0 -i https://mirrors.aliyun.com/pypi/simple

pip install rlschool==0.3.2 gym==0.18.0 metagym==0.1.1 matplotlib==3.3.4 numpy==1.20.3 scikit-learn==0.24.2 make_env==0.0.7 pyglet==1.5.0 mock==3.0.5 -i https://mirrors.aliyun.com/pypi/simple

pip install parl==1.3.1 protobuf==3.19.0 -i https://mirrors.aliyun.com/pypi/simple
#尝试降级
pip install paddlepaddle-gpu==1.8.5.post97 -i https://mirrors.aliyun.com/pypi/simple
pip install paddlepaddle==1.8.5 -i https://mirrors.aliyun.com/pypi/simple
```

```
#rlschool与gym可用
conda create -n LiftSim python=3.6.15
activate LiftSim

pip install rlschool==0.3.2 gym==0.18.0 metagym==0.1.1 matplotlib==3.3.4 numpy==1.19.5 scikit-learn==0.24.2 make_env==0.0.7 pyglet==1.5.0 mock==3.0.5 -i https://mirrors.aliyun.com/pypi/simple

pip install parl==1.4.1 -i https://mirrors.aliyun.com/pypi/simple

pip install paddlepaddle-gpu==1.8.5.post97 -i https://mirrors.aliyun.com/pypi/simple
pip install paddlepaddle==1.8.5 -i https://mirrors.aliyun.com/pypi/simple
```


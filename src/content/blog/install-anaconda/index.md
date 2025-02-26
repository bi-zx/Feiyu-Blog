---
title: 'Anaconda安装配置'
description: '-'
publishDate: '2024-09-03'
heroImage: { src: './thumbnail.jpg', color: '#64574D' }
tags: ['tutorial']
language: '中文'
comment: true
---

1. 以管理员运行安装程序，为所有用户安装，位置默认，暂不添加环境变量

2. 手动添加以下路径到至Path环境变量

```
C:\ProgramData\Anaconda3
C:\ProgramData\Anaconda3\Scripts
C:\ProgramData\Anaconda3\Library\bin
C:\ProgramData\Anaconda3\Library\usr\bin
C:\ProgramData\Anaconda3\Library\mingw-w64\bin
```

3. 换源

```shell
conda config --add channels http://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/free
conda config --add channels http://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/main
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud/conda-forge/
conda config --set show_channel_urls yes
```

4. 创建虚拟环境

```shell
#指定虚拟环境名称, python版本
conda create -n env_name python=3.x.x

#指定虚拟环境路径, 名称, python版本
conda create --prefix=D:\xxx\env_name python=3.x.x
```

5. 导出导入虚拟环境

```shell
#激活需要导出的环境
conda activate env_name

#生成相关yaml文件，文件会生成在C盘用户目录里(导出可能失败，需打开.yaml文件检查)
conda env export > environment.yaml

#在新电脑上根据yaml文件创建环境
conda env create -f environment.yaml

#上面的命令只会导出使用conda安装的，而pip安装的还需要下面的命令
pip freeze > requirements.txt

#导入pip安装的包
pip install -r requirements.txt
```


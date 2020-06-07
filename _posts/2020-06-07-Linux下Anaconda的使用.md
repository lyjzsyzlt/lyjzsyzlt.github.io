---
layout: post
title: Linux下Anaconda的使用
date: 2020-06-07
tags: 工具    
---

## 1.下载安装包

可以从清华镜像上选择合适的安装包[下载](https://mirrors.tuna.tsinghua.edu.cn/anaconda/archive/)，我使用的是```Anaconda3-5.3.0-Linux-x86_64.sh```

## 2.安装
```
bash Anaconda3-5.3.0-Linux-x86_64.sh
```
然后一路回车，空格跳过阅读license，然后输入'yes'。然后输入安装路径，比如输入```/Work/anaconda3```，安装完成后编辑环境变量，在```~/.bashrc```最后写入
```export PATH=/Work/anaconda3/bin:$PATH```，退出执行```source ~/.bashrc```使环境变量生效。最后询问你是否安装VSCode，选择no即可。

## 3.使用
### 3.1 创建虚拟环境
```
conda create --prefix=/Work/anaconda3/envs/py36 python=3.6
```
- --prefix 指定创建的环境存放的位置，最好为anaconda安装路径下面的envs文件夹，这样以后激活的时候，环境名就是py36，不会是很长的一串路径 <br>
- python=3.6 指定安装的python版本

### 3.2 删除虚拟环境
```
conda remove --prefix=/Work/anaconda3/envs/py36 --all
```

### 3.3 激活环境与退出
```
激活：
source activate /Work/anaconda3/envs/py36
退出：
source deactivate
```

### 3.4 python包的安装与删除
**首先得激活相应的环境，否则就安装在系统默认的python环境下了！**
```
安装：
conda install package==version_number
比如：conda install numpy，也可以使用pip install numpy
删除：
conda remove [pkg_name]
比如：conda remove numpy
```
### 3.5 查看已安装的虚拟环境
```
conda env list
```

---
layout: post
title: "Vitual env of Anaconda"
categories: 教程
---

# Conda虚拟环境运行方法

## 1.使用Conda虚拟环境

1. 激活Conda：进入左下角Windows菜单图标，搜索cmd，以**管理员方式**打开后，运行：

   ```shell
   conda init
   ```

   然后运行：

   ```shell
   conda activate your_venv
   ```

2. 移步到你所需的文件夹：

   ```shell
   D:
   cd D:/your_path
   ```

3. 执行python命令

## 2.安装Conda虚拟环境

1. 进入左下角Windows菜单图标，找到Anaconda3文件夹，以**管理员方式**打开Anaconda Prompt

2. 创建虚拟环境，运行：（意思是以-n后的标识符命名这个虚拟环境并指定python版本，python版本的指定可以省略，将安装conda缓存的最新版本）

   ```shell
   conda create -n your_venv_name python=(your_desired_version)
   ```

3. 激活虚拟环境，运行：

   ```shel
   conda activate your_venv_name
   ```

4. 你将看见左侧括号内是你的虚拟环境名，代表激活成功。“激活”指在当前所指定的python都会使用你的虚拟环境的python，而不是在全局安装的python。

5. 安装所需包，运行：

   ```shell
   conda install your_package
   pip install your_package
   ```

   第一句命令是使用conda安装一些包，第二个是使用python内置的pip安装包。所有的“import xx”都是包，python就是基于这一系列包而运行的。

## remind

The former method to create a virtual environment is not 'clean'

literally, it will inherit some package from the base environment

Use the following code to create a clean virtual environment

```txt
conda create -n your_env_name --no-default-packages python=<version>
```

## 3.Further operations of env

```python
conda deactivate #exit current env
```







 
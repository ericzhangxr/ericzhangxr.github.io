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

## 4.If you encountered some network problem

we only introduce the solution method under a VPN environment

```shell
echo %http_proxy%
echo %https_proxy%
#you can find your proxy of http link, if return %----%, this is mean you are not set a proxy
set set http_proxy=http://127.0.0.1:<your_vpn_channel>
#my default channel is 7897
#then you can pip
```

```
conda activate d2l-zh
pip install jupyter d2l torch torchvision -i https://pypi.tuna.tsinghua.edu.cn/simple --trusted-host pypi.tuna.tsinghua.edu.cn
```

## 5.Check out the information of virtual environment

All virtual environments you created

```shell
conda env list
```

Check out the basic information of current VE

```shell
conda info
```

list the package you installed in the current VE

```shell
conda list
```

# Jupyter notebook venv

方法一：通过命令行注册内核（推荐）

这是最常用的方法，只需在终端执行几条命令即可：

### 步骤1：激活你的虚拟环境

在命令提示符中，确保你的虚拟环境已激活（看到`(.venv)`前缀）：

bash

```
# 如果你已经在这个环境中，直接进行下一步
# 如果没有，先激活：
.venv\Scripts\activate
```

### 步骤2：安装ipykernel

在激活的虚拟环境中安装ipykernel包：

bash

```
pip install ipykernel
```

### 步骤3：将虚拟环境注册为Jupyter内核

bash

```
python -m ipykernel install --user --name=你的环境名 --display-name="你想显示的名称"
```

例如，对于你的项目：

bash

```
python -m ipykernel install --user --name=.venv --display-name="Python (.venv)"
```

参数说明：

- `--name`：Jupyter内部使用的名称（通常用环境名）
- `--display-name`：在Jupyter菜单中显示的名称（可以自定义，支持中文）
- `--user`：为当前用户安装（避免权限问题）

### 步骤4：启动Jupyter并选择内核

bash

```
jupyter notebook
```

在打开的页面中，点击右上角的"New"按钮，你应该能看到刚刚添加的"Python (.venv)"选项。如果已有打开的笔记本，可以通过"Kernel" → "Change Kernel"来切换。

**其实你也可以在Anaconda prompt中创建venv，pip packages and then jupyter notebook initialize**

# Anaconda+Jupyter notebook

#### 第一步：创建并激活Conda虚拟环境

首先，打开你的终端（或Anaconda Prompt），使用以下命令创建一个新的虚拟环境。你可以根据需要修改环境名称（这里是 `myenv`）和Python版本（这里是3.9）。

bash

```
# 创建一个名为 myenv，Python版本为3.9的新环境
conda create -n myenv python=3.9
```



命令执行后，conda会解析需要安装的包，并询问你是否继续。输入 `y` 并按回车确认。

环境创建完成后，使用以下命令激活它：

bash

```
# 激活名为 myenv 的环境
conda activate myenv
```



激活成功后，你的终端提示符前会出现 `(myenv)` 字样，这表明你已经在该虚拟环境中了。

#### 第二步：在虚拟环境中安装Jupyter并注册内核

这是最关键的一步，它能让你的Jupyter Notebook识别并使用这个新创建的虚拟环境。

1. **安装`ipykernel`包**：在已激活的虚拟环境中，安装`ipykernel`。这个包是Python环境与Jupyter之间的“桥梁”。

   bash

   ```
   # 在 myenv 环境中安装 ipykernel
   conda install ipykernel
   ```

   

2. **将虚拟环境注册为Jupyter内核**：执行以下命令，将当前虚拟环境注册为一个Jupyter内核。`--display-name` 后面的名字就是将来在Jupyter界面中看到的名字，可以自定义。

   bash

   ```
   # 注册内核，在Jupyter中显示为“Python (myenv)”
   python -m ipykernel install --user --name=myenv --display-name "Python (myenv)"
   ```

   

#### 第三步：启动Jupyter Notebook

完成注册后，**先退出当前虚拟环境，回到你的基础（base）环境**，然后再启动Jupyter Notebook。这样做可以确保Jupyter能够读取到所有已注册的内核。

bash

```
# 1. 退出虚拟环境，回到base环境
conda deactivate

# 2. 启动Jupyter Notebook
jupyter notebook
```



命令执行后，Jupyter Notebook会在你的默认浏览器中自动打开。

### 🔍 验证与使用

在Jupyter的文件列表页面，点击右上角的“New”按钮，你应该能在下拉菜单中看到你刚刚创建的 `Python (myenv)` 内核选项。

选择该内核创建一个新的Notebook。为了确认一切正常，可以在单元格中输入以下代码并运行：

python

```
import sys
print(sys.executable)
```



如果输出的路径中包含你的环境名 `myenv`，那就说明大功告成，你的Notebook正在你专属的虚拟环境中运行了！

`conda install -n base -c conda-forge nb_conda_kernels`

# 如何在jupyter notebook中查看你是否启动了虚拟环境？

1. 使用`%pip list`查看当前环境下都有哪些包！！记得是用`% `而不是`！`作为命令行注册器

2. ```shell
   import sys
   print(sys.executable)
   # 输出路径查看是否是虚拟环境的路径
   ```

# 如果发现很多环境被安装在了C盘，如何处理？

```shell
# 修改环境的默认保存路径
conda config --add envs_dirs D:\Anaconda\envs

# 修改安装包缓存的默认保存路径
conda config --add pkgs_dirs D:\Anaconda\pkgs
```

```shell
conda create --prefix D:\Anaconda\envs\AKquantL --clone AKquantL
# 克隆到一个新的文件夹，针对原来的这个环境
```

移除掉原有的环境

```shell
conda remove --prefix C:\Users\zhangeric\.conda\envs\AKquantL --all
```

并且将新的环境注册到ipynb

```shell
python -m ipykernel install --user --name 你的虚拟环境名称 --display-name "你想在Jupyter中看到的名字"
```


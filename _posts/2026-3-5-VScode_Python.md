---
layout: post
title: "How to Deploy your VScode？ An example of Python"
categories: 教程
---

# How to Deploy your VScode？ An example of Python

## Tips

When you are in the terminal, the command

```shell
python
```

not only print the version of your python, but also Enter the Python interactive environment

In this situation, you can find that in the terminal, a input() line begin with

```shell
>>>
```

this mean the input will in the python environment instead of Windows Powershell Command

```python	
>>>exit()
```

this will back to the windows powershell command environment

## Set Default Profile

the terminal of the VScode is recommended cmd

## "editor.mouseWheelZoom": true

## Venv

**create a virtual environment using VScode instead of Anaconda**

after you open your work dictionary, open a .py file and open one terminal that you can type some command, just run

```shell
python -m venv .venv
```

that command create a hidden dictionary in your current work dictionary, consist of

```text
.venv/
├── Scripts/          # Windows系统的可执行文件
│   ├── activate      # 激活虚拟环境的脚本
│   ├── pip.exe       # 独立的pip
│   ├── python.exe    # 独立的Python解释器
│   └── deactivate    # 退出虚拟环境的脚本
├── Lib/              # Python标准库
├── Include/          # C语言头文件
└── pyvenv.cfg        # 配置文件，指向基础Python安装
```




---
layout: post
title: "An Inro of Linux Server Command"
categories: 教程
---

# AN INTRODUCTION OF LINUX MINECRAFT SERVER COMMAND

this is an introduction to linux command when you are using it to run a Minecraft server

## Linux user management

When you are using the Windows system, you may not pay much attention to user management. Generally speaking, this computer is only used by you alone, i.e., a complicated user management process is redundant for you.

BUT when you facing a linux system, such as a ECS, or a cloud service, it is important to set up different users to manage different file, process or project.

### Users

| Comparison Item    | System User                                                  | Regular User                         |
| ------------------ | ------------------------------------------------------------ | ------------------------------------ |
| **Purpose**        | Running services, background tasks                           | For human use                        |
| **Login Allowed**  | No (Typically shell is nologin)                              | Yes                                  |
| **UID Range**      | 1–999                                                        | 1000+                                |
| **Has Password**   | Usually no password or locked                                | Has password                         |
| **Home Directory** | Sometimes none, or in /var/run etc.                          | /home/username                       |
| **Permissions**    | Usually low, only access to directories required by related services | Higher permissions than system users |
| **Examples**       | www-data, sshd, mysql                                        | ubuntu                               |

### Some code related to user management

- query the current user

  ```shell
  whoami
  ```

  return the current user's name

- get more information of a specific user
  ```shell
  id <username>
  ```

- add a user, with the propose if human using
  ```shell
  sudo adduser <name>
  ```

  then you need to set a password of the user you created

- add a user, intend to run a program or a progress
  ```shell
  sudo useradd --system mcserver
  ```

  this command will create a system user, and no password need, if you need a specific user to run your Minecraft Server, then make a new user instead of use root is a best practice

- if a system user will be activated, before you change user, some permission should be distributed to this system user, decided by the program, progress you want to run by the system user

  ```shell
  ls -la #check current file information
  sudo chown <username>:<username> /path/to/file #for a specific file
  sudo chown -R <username>:<username> /path/to/directory #for a entire directory
  ```

- change the user from root to the user you created

  ```shell
  su - <name>
  ```

### If you forget you password of system user

First, make sure you are **root**

```shell
passwd <username>
```

then just type new password in the shell

### Other commands

```shell
exit #logout current user, back to the root user
```



## Screen management

What is a screen?

Generally, screen is a Terminal Multiplexer, if you want to run a process in you linux server, it will break when you stop you SSH connection. But the process run by a specific screen will not break, in another way to say that use different screen to manage the progress is a best practice.

Create a screen

```shell
screen -S <name>
```

Back to your main shell window

```shell
#Ctrl+a then press d
```

Check the screen you created

```shell
screen -ls
```

Reattach the screen you created

```shell
screen -r <name>
```

Stop the current screen(typically screen that you created)

```shell
exit #moderate way
screen -X -S <screen_id> quit #forced quit
```

If you use `screen -ls` find that one specific screen is activated, i.e. attached but no information of the specific screen generated. you should use

```shell
screen -d <ID or name>
```

to detach the screen then activated it again

You can use a screen named `mc` to run your Minecraft server

**For a stopping process of mcdr but IT CANT BE STOPPED**

```shell
screen -S mc -X quit
#Quit the screen
ps -ef | grep java
#check of java process
#if return 

#mcserver 2490910 2490163  0 23:59 pts/0    00:00:00 grep --color=auto java

#this is the search command itself, this return means that no other java process
```



## How to manage Python in your linux server

Before you manage Python, you need to check some basic information or settings in your servers, which is important if you not sure your server have or not some python version or environment

First, as best practice as your windows computer , A virtual environment should be created before you intend to run some progress

### System python V.S. User_installed Python

System Python refers to the Python interpreter that the Linux operating system installs and relies on by default.

**DONT DO ANYTHING WITH REGARD TO SYSTEM PYTHON AND DONT DO ANY PIP IN THE DEFAULT SYSTEM ENVIRONMETN**

User_installed Python is the Python interpreter you install independently for personal development or specific project needs.

**USUALLY INSTALLED IN A VIRTUAL ENVIRONMENT**

```shell
python --version #check your python edition
python3 --version #check your python3 edition if distinguished
which python #check your path to python, return to a path

ls -l /usr/bin/python3.10 #check the detailed information of your python
```

### Virtual environment related

There are two way to create a virtual environment

| Feature              | pipx                                                         | pip + venv                                                   |
| :------------------- | :----------------------------------------------------------- | :----------------------------------------------------------- |
| **Purpose**          | Specifically for installing and running CLI tools.           | For installing libraries/dependencies for projects.          |
| **Env Management**   | Automatically creates and manages an isolated virtual environment for each application. | Requires manual creation of virtual environment (`python -m venv`) and activation. |
| **Install Location** | Centralized in `~/.local/pipx/venvs/`, with application binaries symlinked to `~/.local/bin/`. | Environment files are typically in the project directory, binaries in the environment's `bin/` directory. |

I recommend to use pipx [[https://pipx.pypa.io](https://pipx.pypa.io/)] to manage your program and virtual environment

Install in Linux with Ubuntu 23.04 or above

```shell
sudo apt update
sudo apt install pipx
pipx ensurepath
sudo pipx ensurepath --global # optional to allow pipx actions with --global argument
```

Initialize pipx 

```shell
pipx list
```

Install mcdreforeg with pipx

```shell
pipx install mcdreforged
```

Upgrade if feasible

```shell
pipx upgrade mcdreforged
```

Official document of mcdreforged https://docs.mcdreforged.com/zh-cn/latest/index.html

## Analysis of the status

when the server is running, you can use

```shell
top
```

to analysis the status of the server. the following is the detailed information of the **top** interface

| Field   | Meaning         | Explanation                                                  |
| ------- | --------------- | ------------------------------------------------------------ |
| PID     | Process ID      | Unique identifier for the process.                           |
| USER    | User            | User who owns/runs the process.                              |
| PR      | Priority        | The priority of the process for kernel scheduling.           |
| NI      | Nice Value      | Static priority of the process (-20 highest, +19 lowest). PR is calculated based on NI. |
| VIRT    | Virtual Memory  | Total virtual memory used by the process (code, data, shared libraries, etc.). |
| RES     | Resident Memory | Non-swapped physical memory used by the process. (Key Metric) |
| SHR     | Shared Memory   | Amount of memory shared with other processes.                |
| S       | Status          | Current process state (R Running, S Sleeping, D Uninterruptible Sleep, Z Zombie, T Stopped). |
| %CPU    | CPU Usage       | Percentage of CPU time used during the sampling period. (Key Metric) |
| %MEM    | Memory Usage    | Percentage of physical RAM used by the process.              |
| TIME+   | CPU Time        | Total CPU time used since process start (precision: 1/100 second). |
| COMMAND | Command/Name    | Command line or program name that started the process.       |

Quit

- type `q` on the keyboard
- type `ctrl+c`

## Linux Directory Structure

```shell
. #current directory
.. #parent directory
```

wildcards: `*`

```shell
example_file/* #means all child folder and files under the example_file
*.jar #means all files ending with .jar
```

**mv**

```shell
mv [options] source target
mv a.txt /path/to/dir #move a file to target
mv old.txt new.txx #rename
mv a.txt b.txt c.txt /path/to/dir #move a batch of files
mv dirA dirB/ #dirA becomes dirB/dirA
mv A/* B/ #move all child files in A into directory B

```

| options | explanation             |
| ------- | ----------------------- |
| `-i`    | prompt before overwrite |
| `-f`    | force overwrite         |
| `-v`    | verbose output          |

**unzip**

extracts `.zip` archives, only works for ZIP files

```shell
sudo apt install unzip #install

#Basic Syntax
unzip [options] archive.zip

unzip file.zip -d /path/to/dir #unzip a file to target directory
unzip -o ~ #overwrite existing files
unzip -i ~ #prompt before overwrite, a more safe way
```

## Some other common command

```shell
pwd #show current working directory
cd /path/to/directory#change you working directory
cd .. #back to the parent directory
ls #show the current directory contains
mkdir #make a new directory in the current working directory
cp #copy
rm #delete
mv #move
zip -r achieve.zip target_folder/ #zip a folder with all its child file
touch test.txt #create a file named text end with .txt
```






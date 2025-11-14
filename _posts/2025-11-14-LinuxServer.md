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

- change the user from root to the user you created
  ```shell
  ```

  




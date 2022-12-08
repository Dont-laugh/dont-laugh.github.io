---
layout: post
title: "局域网文件传输神器：File Browser"
author: "Dont Laugh"
categories: Misc
tags: [misc]
image: title/filebrowser.png
---

> 目标：搭建 File Browser 文件传输环境
>
> 关键词：File Browser



# 需求背景

假如你有一台 PC 和一台 Mac，你需要把一些文件从 PC 传到 Mac 上，你会怎么做？用微信、QQ，还是百度网盘？作为程序员你还可能会想到用 Git。但是这些都会走互联网传输，传输效率非常低。

🤔如果能直接通过局域网传输文件就好了。

File Browser 便是这样一款运用局域网来传输文件的软件。（其实操作系统级别已经有 SMB 协议支持共享了，但不是这篇博客的关注点）



# 软件下载

首先明确一点：你要在哪台电脑上安装 File Browser。

File Browser 是一个文件服务器应用，你需要把它安装在服务器上，即取决于你要用哪台电脑当服务器。官网传送门👉 [Welcome - File Browser](https://filebrowser.org/)

Mac 端通过 homebrew 安装很简单：

```shell
brew tap filebrowser/tap
brew install filebrowser
```

PC 端可以在 GitHub 下载安装包：[Releases · filebrowser/filebrowser (github.com)](https://github.com/filebrowser/filebrowser/releases/)

![filebrowser-download](../assets/img/content/2022-1207-1.png)

> 注意要点击展开之后才有 Windows 平台的安装包。

PC 端也可以用管理员身份运行命令行安装：

```shell
iwr -useb https://raw.githubusercontent.com/filebrowser/get/master/get.ps1 | iex
```

默认安装路径是 `C:\Program Files\filebrowser`，打开只有一个 exe 文件。



# Quick Start

File Browser 的使用非常简单，只需要一行命令：

```shell
filebrowser -r /path/to/your/files -a 0.0.0.0
```

然后用浏览器打开网址：127.0.0.1:8080，输入账号 admin，密码 admin，OK 成功了😆！现在你能在浏览器中看到你电脑 `/path/to/your/files` 文件夹下的所有文件。

在另一台电脑上访问，需要输入真正的 IP 地址。

Windows 平台查询 IP 地址的命令是：

```shell
ipconfig
```

Mac OS 平台查询 IP 地址的命令是：

```shell
ifconfig en0
```

其中字段 `inet` 的值就是 IPv4 的地址。或者你可以前往 `系统设置→网络→Wi-Fi→详细信息` 查看 IP 地址。



# 常用配置

File Browser 有专门的设置界面，就在左侧的 Settings 选项，在这里可以更改管理用户账号、密码和权限，读者可自行探索。

![filebrowser-setting](../assets/img/content/2022-1207-2.png)

File Browser 的所有命令行接口请参考官方文档👉 [Command Line Interface - File Browser](https://filebrowser.org/cli)

## Config

在这里介绍一些常用启动配置的命令，基础的配置设置命令为：

```
filebrowser config set [flags]
```

【注】配置名全写要加两个减号（e.g. `--address`），缩写只用一个减号（e.g. `-a`）。

| 参数名    | 缩写 | 描述                                                         |
| --------- | ---- | ------------------------------------------------------------ |
| `address` | `a`  | 监听的 IP 地址，0.0.0.0 表示监听所有 IP，可用来支持其他电脑访问 |
| `port`    | `p`  | File Browser 网页的端口号                                    |
| `root`    | `r`  | File Browser 网页打开的根目录                                |

## User

再介绍一下用户管理相关的命令：

| 命令                                             | 描述                           |
| ------------------------------------------------ | ------------------------------ |
| `filebrowser users ls`                           | 列举所有用户的账号、密码、权限 |
| `filebrowser users add <username> <password>`    | 添加新用户                     |
| `filebrowser users rm <id|username>`             | 删除用户                       |
| `filebrowser users update <id|username> [flags]` | 更新用户的设置                 |

其中，更新用户设置可用的 `flags` 有：

| 参数            | 缩写 | 描述           |
| --------------- | ---- | -------------- |
| `username`      | `u`  | 用户名         |
| `password`      | `p`  | 密码           |
| `perm.admin`    |      | 管理员权限     |
| `perm.create`   |      | 新建文件权限   |
| `perm.delete`   |      | 删除文件权限   |
| `perm.download` |      | 下载文件权限   |
| `perm.execute`  |      | 执行命令权限   |
| `perm.modify`   |      | 修改文件权限   |
| `perm.rename`   |      | 重命名文件权限 |
| `perm.share`    |      | 分享文件权限   |


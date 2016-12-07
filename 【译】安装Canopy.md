---
title: 【译】安装Canopy
date: 2016-09-29 15:44:31
tags:
layout: draft
---

本文主要翻译了Canopy相关的几篇文档，主要用于理解Canopy环境和快速安装/启动Canopy。

<!-- more -->

#  安装
首先从下载一个Canopy安装包。32位还是64位？标准还是完整？
在32位系统，你必须使用32位的安装包，64位系统
## 环境设置
一旦完成安装，最后一步就是完成你Python环境的设置。GUI将贯穿整个设置过程。

这一节剩余部分将描述标准GUI设置过程。但请注意，有两种主要方式设置你的Python环境：
- 对于用户从来不用Canopy GUI的*管理员*，请看这里[创建一个EPD-like的环境](＃创建一个EPD-like的环境)，例如，这些用户像在EPD环境一样，只用基础命令行。
- 对于那些对在一个多用户的机器或者网络设置Canopy感兴趣的系统管理员也许会对[创建一个共享的Canopy环境](#创建一个共享的Canopy环境)感兴趣。

### 标准GUI设置：
Canopy可以在安装目录直接启动，如果Canopy安装在`~/Canopy`，使用脚本：
> \$ ~/Canopy/canopy

Canopy第一次启动的时候，它会自动配置你的Python环境，要么在默认地址，要么用户自定义定制。这一步允许每一个用户在一个多用户的机器上
# Canopy命令行接口
## 创建一个EPD-like的环境
一个EPD-like的环境意味这个场景里，用户只使用命令行。

On Windows:
>Canopy\App\Canopy_cli.exe --no-gui-setup setup C:\Python27

On Mac:
>/Applications/Canopy.app/Contents/MacOS/Canopy_cli --no-gui-setup setup ~/canopy

On Linux:
>~/Canopy/canopy_cli --no-gui-setup setup ~/canopy

## 创建一个共享的Canopy环境



# Reference
[Linux Installation](http://docs.enthought.com/canopy/quick-start/install_linux.html)
[Where are all of the Python packages in my User Python Environment?](http://docs.enthought.com/canopy/configure/faq.html#venv-where-are-packages)
[Canopy Command Line Interface (CLI)](http://docs.enthought.com/canopy/configure/canopy-cli.html#create-epd-dist)





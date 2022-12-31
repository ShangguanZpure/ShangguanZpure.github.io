---
title: python-funda
date: 2020-10-21 11:07:21
tags:
categories:
description:
photos:
---

[TOC]
# 包导入和自定义包

## Python模块查找路径

import 语句执行时候，Python会查找相应的模块并导入，顺序为：

先查找内置模块

查找sys.path（先查找当前目录文件，PATHONPATH，系统PATH）

## \__init__.py

`__init__.py` 在包被导入时会被执行,为**所在包**进行一些初始化操作，不在所在包则不生效。

用于 导入该模块该目录下需要用到的一些依赖或者其他一些初始化操作。

## 如何自制包

在包的最外层同级目录新建setup.py

运行 `Python setup.py sdist`

## 如何下载依赖安装包

直接Google `依赖包==版本号`，下载即可。

.whl文件安装方法：`pip install <pkg.whl>`

.tar.gz文件：`tar -zxvf <.tar.gz> -C ./`            `python setup.py install`


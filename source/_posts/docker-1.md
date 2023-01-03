---
title: docker容器学习笔记-1
date: 2021-07-18 19:42:44
tags:
- 工作
- 技术
- docker
categories: 技术
description: docker入门
photos:
---
# 容器是什么？有什么用？ what why
容器是？ 软件打包技术
容器是主机上的一个独立进程，容器里面装的是一个个镜像；
镜像是服务更高一层的封装和打包，包含更多的基础信息，比如依赖包、配置文件、脚本、二进制文件、环境变量等。
容器是虚拟化技术的集大成者。
有什么用？ 容器提供一整套更为便捷的服务打包和部署方式。
容器是一种好用、快速的统一规范，是的各种交接流程简单、独立、隔离。

不知大家是否意识到，潘多拉盒子已经被打开。容器不但降低了我们学习新技术的门槛，更提高了效率。

>"That isolation leverages kernel namespaces and cgroups, features that have been in Linux for a long time."
> 这一句英语怎么翻译？

CLI：command-line interface


# docker基本框架 how

* 底层： runtime
* 管理工具： docker engine 提供操作接口
* 定义工具： docker file 生成 docker image 存放在docker registries 很多好的image 放在 docker Hub 
* 为了让docker安全稳定的运行 需要一系列可靠的工具 所以有了容器平台技术 包含管理分布式集群的容器编排技术 在这个基础上还有 支持一键部署等更抽象一层的容器管理平台和一系列的支持技术，支持技术的内容一般都是这几样：网络关系管理、服务发现、监控、数据管理、日志管理、安全性 目前找不到好的一个形象比喻，把它们好好串联起来

## docker的镜像

base -> 可以共享base 比喻：多个不同楼房可以共用一个地基， 或者是共用前N层（N > = 0)

copy-on-write：镜像只可读 不可写 可以复用 因为写才会产生冲突 写都在各自容器的容器层写

# docker初体验

参考的教程是在ubantu系统，我在Mac系统，所以我先看官方的入门介绍
链接为：https://docs.docker.com/get-started/


## 设置docker的镜像源

不设置的话，根本无法下载远程仓库资源，我设置的是亚马逊和网易源
设置方式参见这个链接：
* https://zhuanlan.zhihu.com/p/146876547
* http://t.zoukankan.com/aric2016-p-12423226.html
* https://blog.csdn.net/u012081441/article/details/104553145/

## dockerFile 配置和运行

dockerFile 运行先看看本层的镜像层是否已经存在，存在直接复用，不存在新建临时容器运行配置的命令后commmit为新的镜像层（有commit ID）
如此往复 直到结束。

### Dockerfile常用指令

 

### docker 仓库的使用

repository 的完整格式为：
`[registry-host]:[port]/[username]/xxx `
例如： `[host]:5000/sgzc/httpd:v1`

### docker 基本操作

#### 镜像常用操作

images    显示镜像列表

history   显示镜像构建历史

commit    从容器创建新镜像

build     从 Dockerfile 构建镜像

tag       给镜像打 tag

pull      从 registry 下载镜像

push      将 镜像 上传到 registry

rmi       删除 Docker host 中的镜像

search    搜索 Docker Hub 中的镜像

#### 容器常用操作

docker run --name "container-name" -d 

create      创建容器  

run         运行容器  

pause       暂停容器  

unpause     取消暂停继续运行容器  

stop        发送 SIGTERM 停止容器  

kill        发送 SIGKILL 快速停止容器  

start       启动容器  

restart     重启容器  

attach      attach 到容器启动进程的终端  

exec        在容器中启动新进程，通常使用 "-it" 参数  

logs        显示容器启动进程的控制台输出，用 "-f" 持续打印  

rm          从磁盘中删除容器


# 参考资料

* 官方文档：https://docs.docker.com/get-started
* [每日五分钟系列](https://mp.weixin.qq.com/mp/homepage?__biz=MzI5NDE3ODQyNg==&hid=3&sn=d1883dae2f3e73bc4bb3af24b12c6d00&scene=18)



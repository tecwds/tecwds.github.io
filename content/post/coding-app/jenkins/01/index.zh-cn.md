---
title: 'Jenkins'
description: systemd 安装 jenkins
date: 2024-06-19
image: title-image.png
categories:
  - CodingApp
  - Jenkins
  - Java
---

## 前言

### Jenkins 是什么？

&nbsp;&nbsp;&nbsp;&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;
**Jenkins** 是一款开源的 CI&CD 软件，用于自动化任务，说白了就是帮助我们完成那些繁琐重复的任务。Jenkins 由 Java 编写，因此可以通过 jar 包的形式进行运行，对 jar 包进行封装为 systemd 服务之后，就可以作为系统服务运行在系统上。当然实际中很少直接通过 jar 包的方式去管理一个 Jenkins。接下来会简要介绍安装它的几种方式。

### 如何安装 Jenkins

&nbsp;&nbsp;&nbsp;&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;
上面有说，可以直接运行 jar 包和封装为系统服务进行使用。除此之外，使用 Docker 容器化技术也可以部署运行一个 Jenkins 服务，这种方式的 Jenkins 更加易于管理，部署起来也更简单，也和部署平台无关，仅需要系统上存在 Docker 即可使用，不过可惜的是，目前（2024年6月25日） Docker 镜像已经全部挂掉，因此这篇文章主要记录作为系统服务进行安装使用。

## 安装依赖

既然使用系统服务进行部署使用，就需要目标机器存在其依赖环境，Jenkins 依赖较少，安装比较简单，这里就仅以类 centos 系统为例。

### openjdk-17

```bash
yum install java-17-openjdk -y
```

如果系统较老，则需要下载 rpm 包进行手动下载。这是[amd64平台的直链下载地址](https://download.oracle.com/java/17/latest/jdk-17_linux-x64_bin.rpm)

```shell
rpm -i jdk-17_linux-x64_bin.rpm
```

### fontconfig

```bash
yum install fontconfig
```

### jenkins

```bash
# 添加 jenkins 的仓库源
sudo wget -O /etc/yum.repos.d/jenkins.repo \
    https://pkg.jenkins.io/redhat-stable/jenkins.repo

# 导入 key
sudo rpm --import https://pkg.jenkins.io/redhat-stable/jenkins.io-2023.key

# 更新信息
sudo yum upgrade

# 安装 jenkins
sudo yum install jenkins
```

在使用命令安装 Jenkins 的时候，安装程序已经为 Jenkins 做好了系统服务，过后仅需要启动 jenkins 就能正常使用。

**Tips:** 如果因为网络安装失败，可以使用如下命令

```bash
yum --setopt=proxy=http://代理IP地址:代理端口 install jenkins
```

## 启动 jenkins

```bash
systemctl daemon-reload
systemctl enable jenkins --now
```

## 更新记录

1. **2024-06-19** 
2. **2024-06-25** 更新部分文字内容

## 参考文档

1. [Jenkins 官方文档](https://www.jenkins.io/doc/book/installing/linux)
2. [jdk17 - oracle](https://www.oracle.com/java/technologies/downloads/#java17)
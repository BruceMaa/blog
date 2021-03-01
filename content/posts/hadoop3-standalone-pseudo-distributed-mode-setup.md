---
title: "Hadoop3单机伪分布式部署"
subtitle: "CentOS7.4系统单机部署Hadoop3伪集群"
date: 2021-03-01
summary: 详细记录在CentOS7.4系统中Hadoop3伪集群环境的搭建
description: 文章描述
categories: ["BigData"]
tags: ["Hadoop"]
draft: true
---

## 背景

{{< admonition note "背景" true >}}
最近由于工作需要，转入大数据开发，由于之前只是片面的了解，近期开始系统的学习。
从Hadoop开始，之后会继续学习Hive、HBase、Storm、Spark、Flink、CarbonData等知识。
{{< /admonition >}}

## 目的

本文档介绍了如何设置和配置单节点Hadoop3安装，以便您可以使用Hadoop MapReduce和Hadoop分布式文件系统（HDFS）快速执行简单的操作。

## 前期准备

### 操作系统

Linux，CentOS7.4，关闭防火墙，或者打开对应端口

```bash
systemctl stop firewalld
systemctl disable firewalld
```

### 必要软件

> 必须确认`SSH`安装且运行在默认端口（修改ssh端口我没试过），使用`vim`编辑配置文件，使用`wget`下载其他软件

```bash
yum install -y ssh vim wget
```
3. 安装Java

Java和Hadoop对应的版本描述信息在 [HadoopJavaVersions](https://cwiki.apache.org/confluence/display/HADOOP/Hadoop+Java+Versions)。

在此我们安装`Java8`，下载地址使用[**清华大学开源软件镜像站**][tsinghua]。

```bash
wget https://mirrors.tuna.tsinghua.edu.cn/AdoptOpenJDK/8/jdk/x64/linux/OpenJDK8U-jdk_x64_linux_hotspot_8u282b08.tar.gz
```

安装Java并配置`JAVA_HOME`

```bash
mkdir -p /usr/local/java
tar -xzvf OpenJDK8U-jdk_x64_linux_hotspot_8u282b08.tar.gz -C /usr/local/java/
ln -sf /usr/local/java/jdk8u282-b08/ /usr/local/java/default
echo 'export JAVA_HOME=/usr/local/java/default' >> /etc/profile
echo 'export PAHT=$PATH:$JAVA_HOME/bin' >> /etc/profile
source /etc/profile
```

4. 下载Hadoop3

```bash
wget https://mirrors.bfsu.edu.cn/apache/hadoop/common/stable/hadoop-3.3.0.tar.gz
```

配置Hadoop


[tsinghua]: https://mirrors.tuna.tsinghua.edu.cn
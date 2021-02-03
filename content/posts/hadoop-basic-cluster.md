---
title: "Hadoop学习记录-搭建环境"
subtitle: "CentOS7.4系统部署Hadoop伪集群与集群"
date: 2021-02-01T22:40:01+08:00
summary: 详细记录在CentOS7.4系统中Hadoop集群环境的搭建
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

## [伪集群部署]^(单机部署单节点)

### 准备环境

#### Linux操作系统CentOS7.4

1. 关闭防火墙

```bash
systemctl stop firewalld
systemctl disable firewalld
```

2. 安装必要软件

```bash
yum install -y vim wget
```

#### JDK8

1. 通过[**清华大学开源软件镜像站**][tsinghua]下载OpenJDK8

```bash
wget https://mirrors.tuna.tsinghua.edu.cn/AdoptOpenJDK/8/jdk/x64/linux/OpenJDK8U-jdk_x64_linux_hotspot_8u282b08.tar.gz
```

#### Hadoop 3.3.0

## 集群部署





[tsinghua]: https://mirrors.tuna.tsinghua.edu.cn
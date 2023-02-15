---
title: Dbeaver使用记录
date: 2023-01-12
lastmod: 2023-02-15
draft: false
summary: 记录Dbeaver如何使用
description: Dbeaver是基于Eclipse开源的数据库管理工具，支持丰富的数据库连接，包括NoSQL和大数据 
categories: ["DevTools"]
tags: ["VIM", "Dbeaver", "ClickHouse"]
---

## Dbeaver 的使用记录

### 添加插件

![install](https://brucemaa.cn/images/dbeaver/dbeaver_install_new_software.png)

#### VIM插件

插件地址

```
http://vrapper.sourceforge.net/update-site/stable
```

![install](https://brucemaa.cn/images/dbeaver/dbeaver_vim.png)

### 连接ClickHouse时显示时间有时区问题

首先要保证ClickHouse所在服务器的时间和时区是正确的。

连接ClickHouse使用的JDBC驱动版本号：`0.3.2.11`

#### 修改Dbeaver连接ClickHouse驱动的连接属性

1. 点击菜单栏上的【数据库】，选择【驱动管理器】；
2. 选中【ClickHouse】，点击【编辑】；
3. 点击【连接属性】，鼠标右键点击【用户属性】，添加属性

![install](https://brucemaa.cn/images/dbeaver/dbeaver-clickhouse-jdbc.png)

#### 修改DataGrip连接ClickHouse驱动的连接属性

![install](https://brucemaa.cn/images/dbeaver/datagrip-clickhouse-jdbc.png)


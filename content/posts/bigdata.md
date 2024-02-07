---
title: 'Big Data'
date: 2024-02-05T18:00:36-08:00
type: post
draft: true
---
- [基本概念](#基本概念)
  - [数据分析的基本概念](#数据分析的基本概念)
    - [数据分析的三个方向](#数据分析的三个方向)
    - [数据分析的基本步骤](#数据分析的基本步骤)
    - [分布式与集群(Distributed, Cluster)](#分布式与集群distributed-cluster)
  - [Linux 操作系统(operating system)](#linux-操作系统operating-system)
  - [VMware虚拟机](#vmware虚拟机)

# 基本概念
## 数据分析的基本概念
### 数据分析的三个方向
现状分析（分析当下的数据）：现阶段的整体情况，各个部分的构成占比、发展、变动；      
原因分析（分析过去的数据）：某一现象为什么发生，确定原因，做出调整优化；         
预测分析（结合数据预测未来）：结合已有数据预测未来发展趋势；        

 - 离线分析（Batch Processing）     
   - 面向过去，分析已有的数据       
   - 批处理      
 - 实时分析（Real Time Processing | Streaming）
   - 分析实时产生的数据
   - 分秒级、毫秒级
 - 机器学习（Machine Learning）
   - 基于历史数据和当下产生的实时数据预测未来发生的事情
   - 侧重于数学算法的运用
  
### 数据分析的基本步骤
1. 明确分析目的和思路
2. 数据收集     
        - 业务数据、日志数据、爬虫数据、互联网公开数据
3. 数据处理（数据预处理）       
        - 数据清洗、数据转化、数据提取、数据计算            
        - 干净规整的结构化数据（二维表）
4. 数据分析
5. 数据展现（数据可视化）

- 大数据的5V特征               
  - Volume, Variety, Value, Velocity, Veracity

### 分布式与集群(Distributed, Cluster)
![backend_structure](/posts/images/hadoop1.png)
- 分布式、集群共同的点是：**都是多台机器（服务器）组成的**；


## Linux 操作系统(operating system)
- 管理与配置内存、决定系统资源供需的优先次序、控制输入设备与输出设备、操作网络与管理文件系统等基本事务；      
- 操作系统分类
  - 桌面操作系统（mac, windows, linux）             
  - 嵌入式操作系统（工业、军事、航空）      
  - 服务器操作系统（Unix, Linux, Windows Server, Netware）
  - 移动设备操作系统
- Linux内核（Kernel）
  - 核心部分
  - Linux操作系统 = linux kernel + GNU软件及系统软件 + 必要的应用程序
  - Linux发行版本（个人桌面版，企业服务器版）
  - Ubuntu, Redhat(红帽系列)以及延伸版本（Centos）

## VMware虚拟机
- 局域网的组成           
    服务器，交换机，网线，机架
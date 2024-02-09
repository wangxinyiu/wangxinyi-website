---
title: 'Hadoop'
date: 2024-02-06T14:51:57-08:00
type: post
---

- [Hadoop概述](#hadoop概述)
  - [Hadoop组成](#hadoop组成)
    - [HDFS](#hdfs)
    - [YARN](#yarn)
    - [MapReduce](#mapreduce)
    - [HDFS、YARN、MapReduce三者关系](#hdfsyarnmapreduce三者关系)
  - [大数据技术生态体系](#大数据技术生态体系)

# Hadoop概述
1. Hadoop核心组件
- Hadoop HDFS（分布式文件存储系统）：解决海量数据存储
- Hadoop YARN（集群资源管理和任务调度框架）：解决资源任务调度
- Hadoop MapReduce（分布式计算框架）：解决海量数据计算          
(广义上Hadoop是指围绕Hadoop的大数据生态圈)   

## Hadoop组成

### HDFS
1. NameNode(nn) -> 数据都存储在什么位置: 存储文件的元数据，如文件名，文件目录结构，文件属性（生成时间、副本数、文件权限），以及每个问价你的块列表和块所在的DataNode等
2. DataNode(dn) -> 具体存储数据: 在本地文件系统存储文件块数据，以及块数据的校验和
3. Secondary NameNode(2nn) -> 秘书: 每隔一段时间对NameNode元数据备份

### YARN
![yarn](/posts/images/yarn.jpg)

### MapReduce
MapReduce将计算过程分为俩个阶段：Map 和 Reduce
1) Map阶段并行处理输入数据
2) Reduce阶段对Map结果进行汇总

![mapreduce](/posts/images/mapreduce.jpg) 

### HDFS、YARN、MapReduce三者关系
![relationship](/posts/images/hadoop2.jpg)

## 大数据技术生态体系
![Hadoop3](/posts/images/hadoop3.jpg)
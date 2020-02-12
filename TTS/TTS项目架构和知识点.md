## TTS项目架构图

![](https://github.com/user-wbgithub/BigData/blob/master/TTS/Images/1.png)

## 知识内容

###### 1.flueme的hive-sink

###### 2.hive的事物表机制

###### 3.RCFile，ORCFile文件格式

###### 4.通过SpringDataJPA规范操作Hive的Mysql

###### 5.数仓的建模和设计

## 概述

###### 一、之前直接将flume中的数据存储在HDFS中，然后利用Hive进行数据的ETL。

###### 二、现在将flume中的数据存储在Hive表中进行管理。去除Hive定时通过外部表分区表管理HDFS数据的过程。

###### 三、通过flume向hive中添加数据，数据格式是json格式。对于flume需要满足两个条件：hive的jar包；hive中处理json文件的jar包hcatlog；

###### 四、flume向hive表中添加数据时hive需要支持hive的事物表机制，这样就开始支持事物和行级更新。需要配置hive支持事物。

###### 五、hive中表的建立也必须时文件格式是ORC格式，必须是分桶表，必须是事物表。才可以正常进行行级别更新。
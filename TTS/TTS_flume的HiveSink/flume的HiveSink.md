## Flume的HiveSink总体是下面这样的

![](https://github.com/user-wbgithub/BigData/blob/master/TTS/Images/2020-02-13_004451.jpg)

## 细节

###### 一、首先在Hive中存储json格式的数据

###### 	hive存储json格式的数据需要使用Hive中含有的hcatlog包，建表语句中需要使用row format serde 'org.apache.hive.hcatalog.data.JsonSerDe'

###### 二、Flume的HiveSink

###### 	1.从前台埋点来的数据为json格式，在Flume中将json格式的数据处理好。导入Hive的hcatlog包到Flume中。

###### 	2.Flume中的数据需要存储到Hive中导入Hive包到Flume中。

###### 	3.Hive的元数据库因为是mysql自动建立的hive元数据库，导致hive的元表不全，需要手动初始化元数据库。删除mysql上的hive元数据库；删除HDFS上的hive目录；在mysql下创建hive源数据库；在hive下执行；完成后查看hive元表共53个

```
create database hive character set latin1;
./schematool -initSchema -dbType mysq
```

###### 	4.启动hive的远程数据库服务：flume中的hive sink操作的hive时是通过hive的元数据信息进行操作的。所以连接hive的远程数据库需要启动此服务；

```
进入hive的bin目录，执行：sh  hive --service metastore
```

###### 	5.在Hive中建立分区、分桶、文件存储格式为ORC格式、必须是事物表的表。这里是hive是因为只有事物表才可以支持行级别的更新。由于在hive中已经对json格式数据已经通过hcatlog处理过了，这里建表时不再需要写row format serde 'org.apache.hive.hcatalog.data.JsonSerDe'这句话。

###### 	6.Flume的配置文件。数据源：这里使用监听本地文件的数据源模拟前台传来的json数据，在spooldir数据源中设置时间拦截器，作为分区；通道：使用内存通道；sink使用hive sink，需要设置metastore的地址thrift://hadoop01:9083，设置hive库名、表名、设置拦截器格式、设置序列化器JSON
---
title: Scrapy 抓取数据输出到 Kafka 通过 Flink 及时清洗 并保存到Hbase 数仓.
date: 2019-12-14 21:44:17
description: 都串起来,分布式系统.
---





2. Kafka 配置和启动,以及设置.

下载
```
wget http://mirror.cogentco.com/pub/apache/kafka/2.4.0/kafka_2.12-2.4.0.tgz
```

解压
```
tar -zxvf kafka_2.12-2.4.0.tgz
```

启动zookeeper
```
bin/zookeeper-server-start.sh config/zookeeper.properties
```

启动kafka
```
bin/kafka-server-start.sh config/server.properties
```

创建Topic
```
bin/kafka-topics.sh --create --bootstrap-server localhost:9092 --replication-factor 1 --partitions 1 --topic test
```
查看Topic
```
bin/kafka-topics.sh --list --bootstrap-server localhost:9092
```
测试发送数据
```
bin/kafka-console-producer.sh --broker-list localhost:9092 --topic test
```
测试接受数据
```
bin/kafka-console-consumer.sh --bootstrap-server localhost:9092 --topic test --from-beginning
```

1. scrapy 抓取和管道.
需要安装pykafka
增加kafka管道.
连接管道
编码发送数据
KafkaTopic

3. Flink ETL脚本编写

4. Hbase启动并存储数据.




从yelp 和google 和bbb采集的数据

通过phone进行关联.

于是用phone做键 通过kafka输出采集后存储到hbase中去.
flink也去消费kafuka,获得每一个phone后.通过phone去hbase中获取其他数据源的数据.并组合.将结果更新到mysql库里.同时在redis中存储一个汇总表.这个表的数据是实时更新新的.得分数据. 前端接口每隔若干秒去查询一下当前的redis表或者mysql表,获得及时的排序数据.
按照时间的统计信息,进redis. 运算结果增量批处理进mysql.批量添加或者个更新.

这是ETL的玩法.


在采集的时候就要对一些字段做对齐.

日期 转换utf
电话 去除格式 增加地区编码
review 进行清洗,
标签,service进行统一
得分之类进行 数值化.

地址进行校验
电话进行校验

flink中进行更进一步的处理.

对于采集来的埋点数据.

统计关键指标.
抽取特征.
进入数仓.

进行协同过滤运算.
怎么去查找血缘关系.

增加一个结构 : s1:google,s2:Etl,s3.bbbb,s4.pppp这样就能在最后找到这个数据最初的来源了.




终于把整个流运算流程走通了.好费劲啊.

scrapy -> kafka -> flink -> file.

flink 打包直接用mvn. 然后在页面上提交到
http://localhost:8081/#/job/18cd8c1b3f3a9d0bbe5634e959bbe78f/overview/90bea66de1c231edf33913ecd54406c1/metrics

开启多个窗口,就可以看到不同的topic中传输过来的数据在flink里的一个窗口期中运算完毕并保存到本地文件中.

关键源码参考了这位的.
https://github.com/mabdulrazzak/flink-kafka-prototype/tree/master/src/main/java

我发先java的包管理好用是好用,但是导致系统变得异常复杂.版本依赖.再加上IDE,死的更惨.

所以还是离开这些东西,从头开始整,才能搞清楚.

下一步用新的java语法写,会舒服很多.不过还是熟悉一下java语法.以及流运算的架构用法.这些.

最终发现还是在github上找源码看来的更直接有效. 看别人的教程实在是太绕弯了.
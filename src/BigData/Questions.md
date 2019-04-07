4.项目里面从数据采集到最终的数据可视化，每个环节都有可能丢数据，怎么判断数据有没有丢，如果丢了如何定位到在哪一个环节丢的
6.项目里面用到了时序数据库opentsdb，为什么要用这个，有没有跟其它的时序数据库对比过 

8.数据接入的时候，怎么往kafka topic里面发的，用的什么方式，起了几个线程，producer是线程安全的吗 
9.kafka集群有几台机器，怎么确定你们项目需要用几台机器，有评估过吗，吞吐量测过吗 
10.spark streaming是怎么跟kafka交互的，具体代码怎么写的，程序执行流程是怎样的，这个过程中怎么确保数据不丢
11.kafka监控是怎么做的，kafka中能彻底删除数据吗，怎么做的
12.kafka如何保证高吞吐的，了不了解kafka零拷贝，具体怎么做的 
13.hbase中row key该怎么设计 
14.hdfs文件上传流程，hdfs的容错机制 
15.怎么解决hive数据倾斜问题 
16.hive数据倾斜产生的原因，怎么解决 
17.数据建模，星型模型和雪花模型 
18.spark程序的运行流程 
19.spark streaming从kafka中读数据的两种方式 
20.kafka怎么保证高吞吐量，项目中有测过吞吐量吗，相比于其它MQ，为什么会选择kafka，kafka怎么保证exactly once语义 
21.了解hbase吗，hbase为什么查询速度快 
22.hive sql怎么转换成底层的MapReduce程序，以及shuffle的过程 
23.谈谈对kafka的理解，能讲多少讲多少 
24.spark怎么划分stage，宽窄依赖，各包括哪些***作 
25.zookeeper怎么保证原子性，怎么实现分布式锁 
26.hdfs读写过程
27.MR原理
28.shuffer版本迭代的过程中更新了什么？ 
29.kafka原理，从生产者生产产品到消费者消费过程是怎样的？ 
30.flume框架的原理，soure有哪些？sink有哪些？ 
31.hive如何去重？hive如何行转列？（内置函数） 
32.hadoop中Combiner的作用 
33.hql的join，用过没？类似hive的连接查询吧。 
34.hive得架构，hbase得架构。
35.问了storm架构，flume架构。然后实现10亿数据的appid进行pv，uv操作。其中uv去重不要堆机器，设计一个数据结构做出来。 
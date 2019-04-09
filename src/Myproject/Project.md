### 项目介绍：

1.(1)Spark Streaming + Kafka + Spark SQL + MySQL
  (2)先发送消息到Kafka集群中
	2.1Kafka集群是受到zookeeper管理的，具体就是目前消费者读取kafka数据的偏移量是写到zookeeper中的
  (3)SparkStreaming从Kafka集群中拉取数据进行实时流处理
     使用No Receirvers直接抓取Kafka数据，没有缓存，不会出现溢出问题
  (4)SparkStreaming处理数据后写入到MySQL中

总体流程
1.Spark初始化
	1.1配置SparkConf (SparkConf conf = new SparkConf().setMaster("spark://192.168.189.1:7077").setAppName("xxxx"))
	1.2创建SparkStreamingContext (JavaStreamingContext jsc = new JavaStreamingContext(conf,Durations.seconds(10)))
	1.3创建SparkStreaming输入数据来源 (采用 No Receivers直接从kafka中抓取数据，没有缓存，不会出现内存溢出)
		JavaPairInputDStream<String,String> adClickedStreaming = 
			KafkaUtils.createDirectStream(jsc,key值类型，val值类型，解码器，kafka配置，kafka主题)
	1.4对DStream进行编程，对DStream的编程实质是把每个Batch的DStream翻译成对RDD的操作
///////////////业务代码//////////////////
2.在线黑名单过滤
	2.1从MySQl中查询到黑名单，并将黑名单转换成RDD
		使用jdbc,select * from balcklisttable,在封装成RDD
	2.2从kafka中读取数据，并将流处理和黑名单进行左连接，进行过滤
	2.3动态将黑名单写入数据库
		jdbcWrapper.doBatch("insert into blacklisttable values(?) ",blackList)
		(使用单例模式获取jdbcInstance)
3.计算Batch Duration中每个User的广告点击量
	3.1主要使用mapToPair切分每一行，使用reduceByKey对进行分组聚合
4.判断用户点击是否属于黑名单点击
5.广告点击累计动态更新
	使用updateStateByKey可以带有历史记录的统计广告点击总量
6.对广告点击进行TopN计算，分区域计算每个广告点击量
	使用到了HiveContext进行查询
7.计算过去一段时间（半小时）广告点击的趋势
	使用reduceByKeyAndWindow，窗口长度是30min,滑动间隔是10min
////////////////////////////////////////
8.Spark Streaming执行引擎Driver开始运行
	jsc.start();
	jsc.awaitTermination();
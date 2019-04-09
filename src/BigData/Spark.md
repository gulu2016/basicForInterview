### spark面试相关
1.看你写了实时计算的程序，你怎么保证计算的结果肯定是对的 

2.spark程序的运行流程
 
3.spark怎么划分stage，宽窄依赖，各包括哪些***作 

4.shuffer版本迭代的过程中更新了什么？

5.spark streaming从kafka中读数据的两种方式 

6.spark streaming是怎么跟kafka交互的，具体代码怎么写的，程序执行流程是怎样的，这个过程中怎么确保数据不丢


### 来自《Spark实战三部曲》的知识点

1.基本概念
   - master和worker、Driver和Executor（2,59页）
   - SparkContext（2，61页，63），DAGScheduler(3页),TaskScheduler(3页)
   - job,stage,task,RDD,partition(3页)
   
2.DataFrame和RDD(7页)
   - DataFrame和DataSet（10页）
   
3.RDD底层存储原理（30页）
   - task,Block,Partition与RDD的关系(30页)
     (RDD包含(Partition,Block)，Partition->task)
   
4.RDD的恢复与持久化（31）

5.SparkContext(32)

6.RDD的五大特性是什么（32-33页）
   - 分区列表（32）
   - 计算函数（32）
   - 依赖其他RDD的列表：宽依赖和窄依赖（32，43，46）
   - 分区器（33）
   - 优先位置列表（33）
   
7.转换算子和行动算子（34）

8.RDD弹性特性七个方面（36,40,了解）

9.窄依赖的两类（43,了解）

10.关于DAG视图（46,了解）

11.RDD和stage的关系（32，46页，75页）

12.DAG生成stage(47页，了解)

13.关于task(49页)

14.task启动（49页）

15.RDD容错的三个层次（57）

16.job运行流程（62,65,66页）

17.DataSet转化为RDD(78页)


## Flink

### 运行时架构

- https://www.codenong.com/cs105691815/
- 提交

![在这里插入图片描述](https://i2.wp.com/img-blog.csdnimg.cn/20200422202423595.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5NjU3OTA5,size_16,color_FFFFFF,t_70)

这里的RM是Flink的RM。在yarn模式下，Flink的RM和JM都运行在AppMaster的节点上。

- yarn 提交

![在这里插入图片描述](https://i2.wp.com/img-blog.csdnimg.cn/20200422202750350.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5NjU3OTA5,size_16,color_FFFFFF,t_70)

- JobManager  schedule, checkpoint, graph -  对应spark的driver  -  yarn AppMaster

  将逻辑JobGraph 转换为 物理ExecutionGraph

- [ ] TaskManager (process):  task slot (thread) Resources, 向RM注册Slot

- [ ] Resource Manager：启动TM。管理task slots；区别于Yarn的RM管理Container

- Communication: actor system

- Dispather 

  WebUI

  

### 编程框架

- #### Source

val benv = ExecutionEnvironment.getExecutionEnvironment  // 批处理  -  Batch Processing - bound stream

val env = StreamExecutionEnvironment.getExecutionEnvironment // 流处理   unbound stream

val dataStreamSource = env.addSource(xxSource类) //自定义xxSink类   extend RichSinkFunction

- Transformation

ds 转换.......    .window().keyby.map().flatmap() // 可以设置window slide

ds.setParallelism(1).print()

- Sink

ds.addSink(***Sink类)  //自定义***Source类   extend RichSourceFunction

env.execute("启动执行")



### 并行度

jvm <-> task manager <-> 1个进程

task slot <-> thread：计算资源单位，数量=最大并行度。注：不同的slot，内存隔离，cpu共享。

env.setParallelism()  //设置整个程序并行度, 可以在不同的step中设置
ds.setParallelism()  // 设置操作并行度
        windowCounts.print().setParallelism(1);
        windowCounts.setParallelism(1).print()

spark Partition个数 = BatchInterval / blockInterval
spark window size 设置：val ssc = new StreamingContext(conf, Seconds(1))

任务划分 方式 与 spark 相同，只要没有shuffle，操作不需要跨分区，就能将多步操作合并为一步。

One-to-one 操作 合并

Redistribution 操作 不能合并 （keyBy， broadcase 操作）

### 计算流图

flink code -> streaming graph -> job graph -> execution graph

One-to-one 操作 合并



### Watermark 

#### 产生方式

punctuated watermark

periodic watermark



#### 触发计算的条件

watermark时间（max_eventTime-t） >= window_end_time；
在[window_start_time,window_end_time)中有数据存在。



### Window

tumbling window 

sliding window

session window



### Backpressure





## Related 

### Kafka

kafka数据的两种方式：Receiver与Direct的方式

发布订阅
Events are organized and durably stored in topics. 

事件保序
exactly the same order

扩展容错
Topics are partitioned, meaning a topic is spread over a number of "buckets" located on different Kafka brokers.

consumer rebalance
event 在 partition 内有序，最好能consumer和partition数量对等

kafka优势主要体现在吞吐量上，虽然可以通过策略实现数据不丢失，但从严谨性角度来讲，大不如rabbitmq；而且由于kafka保证每条消息最少送达一次，有较小的概率会出现数据重复发送的情况；

RabbitMQ：以broker为中心，有消息的确认机制
kafka：以consumer为中心，无消息的确认机制

### HDFS

hadoop vs hadoop yarn（ YARN (Yet Another Resource Negotiator) is the resource manager. ）

jobtracker  分拆为 ResourceManager、NodeManager 和 Application Master

每个节点的map slot，reduce slot 不固定

hadoop只是一个框架，主要包含hdfs和mapreduce；yarn是一个平台，其上可插拔storm spark


### Redis
hash set list 三种基本数据类型
mhset mhget 批量操作
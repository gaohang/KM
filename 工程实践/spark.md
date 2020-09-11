## 核心概念

![Spark cluster components](https://spark.apache.org/docs/latest/img/cluster-overview.png)

### Context

spark context,  sql context, hive context

#### SparkContext 是什么?

1. 驱动程序使用SparkContext与集群进行连接和通信，它可以帮助执行Spark任务，并与资源管理器(如YARN 或Mesos)进行协调。
2. 使用SparkContext，可以访问其他上下文，比如SQLContext和HiveContext。
3. 使用SparkContext，我们可以为Spark作业设置配置参数。val sc = new SparkContext(sparkConf)

#### spark session

SparkSession是在Spark 2.0中引入的，不用担心不同的上下文.

```
// 通过session获取context
sc = spark.sparkContext.broadcast()
```

![图片描述](https://segmentfault.com/img/bVOfCt?w=597&h=646)

### RDD

#### 序列化

serialized closure -> serialization

#### 共享变量

accumulator







#### 血缘关系

不必纠结宽窄依赖概念。区别宽窄的目的就一个：划分 Stage。

stage的英文意思是阶段，也就是说要等前一个stage的tasks全部计算完成才能进行下一个stage。Stage 之间做 shuffle，Stage 之内做 pipeline。pipeline是可以独立执行的最长链路。

stage划分方法：从后向前，遇到宽依赖就划分为一个stage

从失败恢复的角度：partition之间独立，就可以并行恢复，不需要同步。

https://www.zhihu.com/question/37137360

除了countByKey， 其他*ByKey操作都会引发shuffle。

#### 持久化

cache persist checkpoint





## 常用操作

### repartition 

- coalesce 减少分区数， 用于小文件合并
- repartition可增可减

### mapPartition 



### foreachPartition



## Spark sql

### rdd ds df 关系

rdd (1,zhangsan,30) -  df (id, name, age)  -  ds (class Persion)





### UDF

https://dongkelun.com/2018/08/02/sparkUDF/

#### Spark Sql用法

##### 匿名注册

- ```
  spark.udf.register("strLen", (str: String) => str.length())  //匿名注册
  ```

- ```
  spark.sql("select name,strLen(name) as name_len from user").show //使用
  ```

  ```
  +-----+--------+
  | name|name_len|
  +-----+--------+
  |  Leo|       3|
  |Marry|       5|
  | Jack|       4|
  |  Tom|       3|
  +-----+--------+
  ```

##### 实名函数注册

```
/** * 根据年龄大小返回是否成年 成年：true,未成年：false*/
def isAdult(age: Int) = {  if (age < 18) {    false  } else {    true  }}

spark.udf.register("isAdult", isAdult _)

```



#### DataFrame用法

##### 使用

```
import org.apache.spark.sql.functions._
//注册自定义函数（通过匿名函数）
val strLen = udf((str: String) => str.length())
//注册自定义函数（通过实名函数）
val udf_isAdult = udf(isAdult _)


//通过withColumn添加列
userDF.withColumn("name_len", strLen(col("name"))).withColumn("isAdult", udf_isAdult(col("age"))).show
//通过select添加列
userDF.select(col("*"), strLen(col("name")) as "name_len", udf_isAdult(col("age")) as "isAdult").show

```

##### 结果

```
+-----+---+--------+-------+
| name|age|name_len|isAdult|
+-----+---+--------+-------+
|  Leo| 16|       3|  false|
|Marry| 21|       5|   true|
| Jack| 14|       4|  false|
|  Tom| 18|       3|   true|
+-----+---+--------+-------+
```





### 写redis

http://bourneli.github.io/scala/spark/pit/2017/10/27/spark-to-redis.html

- redis客户端数量 = partition数量

```scala
// 写Redis
sampleData.repartition(500).foreachPartition(rows => {
  val rc = new Jedis(redisHost, redisPort)
  rc.auth(redisPassword)
  val pipe = rc.pipelined

  rows.foreach(r => {
    val redisKey = r.getAs[String]("key")
    val redisValue = r.getAs[String]("value")
    pipe.set(redisKey, redisValue)
    pipe.expire(redisKey, expireDays * 3600 * 24)
  })

  pipe.sync()
})
```

- 注意不要影响线上服务

  Redis一般提供在线服务，在更新Redis的同时，它可能在前端提供服务。所以在写Redis时，不能使用太多executor。否则会使得QPS过高，影响在线服务响应，甚至导致Redis瘫痪。推荐的实践方法是提高数据的分区数量，确保每个partition的数量较小，然后逐步提高并发数量（executor数量）。观察在不同数量executor下，并发写入Redis的QPS，直到QPS达到一个可以接受的范围。



### java对象与scala对象的互相转化

```
val map1 = JavaConversions.mapAsJavaMap(map)
```




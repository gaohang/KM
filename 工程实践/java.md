# java

## 关键字

tansient

Java中transient关键字的作用，简单地说，就是让某些被修饰的成员属性变量不被序列化，节省空间。

例如，长方形类有三个属性：长度、宽度、面积（示例而已，一般不会这样设计），那么在序列化的时候，面积这个属性就没必要被序列化了。

## 数组集合

Java Array 和 Collection
既然有数组这种数据类型，为什么还需要其他集合类？这是因为数组有如下限制：
数组初始化后大小不可变；
数组只能按索引顺序存取。

List：有序
ArrayList 
LinkList
contains()依赖equals() 方法， string、Integer等包装类已经实现equals()

Set: HashSet SortedSet->TreeSet

Map: HashMap SortedMap->TreeMap
Map中不存在重复的key，因为放入相同的key，只会把原有的key-value对应的value给替换掉。
HashMap  hashCode()  equals()
如果a和b相等，那么a.equals(b)一定为true，则a.hashCode()必须等于b.hashCode()；
如果a和b不相等，那么a.equals(b)一定为false，则a.hashCode()和b.hashCode()尽量不要相等。
EnumMap
无需hashCode和equals

Scala Array 和 Collection
数组是用来存储固定大小的同类型元素
Scala 集合分为可变的和不可变的集合。



## 日志

门面+实现

Commons Logging和Log4j这一对好基友

又会蹦出来SLF4J和Logback





java 反射
-----------------------------------------------------------------

Class.forname()
.class
getClass

class对象：成员，函数，构造函数
可执行函数提示就是由于class对象被装载入内存

junit单元测试
-----------------------------------------------------------------
assertEquals
@Before
@After

注解
-----------------------------------------------------------------



AOP
-----------------------------------------------------------------
切面：从不同service中抽象出公共需求。例如，打印日志，数据库操作



## 多线程



## IO

IO过程: 设备<->内核缓存<->进程

### BufferedInput/outputStream

缓存是为了加速IO。它维护一个内部缓冲区以存储从底层输入流读取的字节。



### DataInput/OutputStream  VS  ObjectInput/OutputStream 

DataInput/OutputStream performs generally better because its much simpler. It can only read/write primtive types and Strings.

ObjectInput/OutputStream can read/write any object type was well as primitives. It is less efficient but much easier to use if you want to send complex data.  (记得将自定义类 implements Serializable )

I would assume that the Object*Stream is the best choice until you *know* that its performance is an issue.

# git

思想

分布式去中心化，不同的分支就是平行线。

commit在线上画点做标记

## 区域

![img](D:\Users\11109539\todolist\img\v2-af3bf6fee935820d481853e452ed2d55_1440w.jpg)



## 分支管理

- master 为主分支，也是用于部署生产环境的分支
- release 为预上线分支，发布提测阶段，会release分支代码为基准提测
- 开发新功能时，以develop为基础创建feature分支
- develop 为开发分支，始终保持最新完成以及bug修复后的代码
- 分支命名: hotfix/ 开头的为修复分支，它的命名规则与 feature 分支类似

## 实践案例

**增加新功能**

```
(dev)$: git checkout -b feature/xxx            # 从dev建立特性分支
(feature/xxx)$: blabla                         # 开发
(feature/xxx)$: git add xxx
(feature/xxx)$: git commit -m 'update ...'
(dev)$: git merge feature/xxx --no-ff          # 把特性分支合并到dev
```

**修复紧急bug**

```
(master)$: git checkout -b hotfix/xxx         # 从master建立hotfix分支
(hotfix/xxx)$: blabla                         # 开发
(hotfix/xxx)$: git add xxx
(hotfix/xxx)$: git commit -m 'commit comment'
(master)$: git merge hotfix/xxx --no-ff       # 把hotfix分支合并到master，并上线到生产环境
(dev)$: git merge hotfix/xxx --no-ff          # 把hotfix分支合并到dev，同步代码
复制代码
```

**测试环境代码**

```
(release)$: git merge dev --no-ff             # 把dev分支合并到release，然后在测试环境拉取并测试
```

**生产环境上线**

```
(master)$: git merge release --no-ff      # 把release测试好的代码合并到master，运维人员操作
(master)$: git tag -a v0.1 -m '部署包版本名'  #给版本命名，打Tag
```



# Maven

## 生命周期

​	![img](D:\Users\11109539\todolist\img\7642256-c967b2c1faeba9ce.png)



## 实践

mvn -Uclean install

mvn -Uclean package

## pom文件



# quartz

## 参看文档

- 中文 https://www.w3cschool.cn/quartz_doc/quartz_doc-2put2clm.html
- quartz官网 http://www.quartz-scheduler.org/documentation/quartz-2.3.0/
- spring官网 https://docs.spring.io/spring/docs/3.2.2.RELEASE/spring-framework-reference/html/scheduling.html#scspringheduling-quartz

## 主要组件

- Scheduler - 与调度程序交互的主要API。
- Job - 由希望由调度程序执行的组件实现的接口。
- JobDetail - 用于定义作业的实例。JobDetail对象是在将job加入scheduler时，由客户端程序（你的程序）创建的。
- Trigger（即触发器） - 定义执行给定作业的计划的组件。
- JobBuilder - 用于定义/构建JobDetail实例，用于定义作业的实例。
- TriggerBuilder - 用于定义/构建触发器实例。
- 
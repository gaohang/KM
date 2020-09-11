# springboot

## 配置文件

xml

```
<server>

	<port>8080</port>

</server>
```

yaml 面向数据，简洁

```
server:
	port:8081
person：
    Name:{lastName：张，age：三}
    pets：
        - cat
        - dog
```

### 配置注入

```
@Component
@ConfigurationProperties（prefix="persion"）
Class Persion{
	Map<String,String> Name;
	Lsit<String> pets;
}
```



### 配置文件加载

顺序

application.properties 顺序：

jvm参数  > 工程文件夹 > 工程文件夹/config/ > classpath > classpath/config > @Configuration

**ClassPath对应着Jar包的根目录，对应着编译后的target的classes目录**。


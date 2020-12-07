# 产品分析（漏斗分析）

打开app -> 点击搜索 -> 点击 -> 有效播放（i.e.播放时长>60s）

日活

7/30d 留存

# 评估

## 排序指标

online： top1/top3/first page ctr

offline： ndcg/ gauc

## Ner指标

意图准确率

实体准确率

## 联想搜索

点击率

## 纠错指标

 



# DSL



```
{
	"query": {
		"fuzzy": {
			"lyricList.keyword": {
				"value": "驮着唐三藏和仨徒弟",
				"fuzziness": 2
			}
		}
	},
	"_source": [
		"singerName",
		"songName",
		"id",
		"lyric",
		"lyricList"
	]
}
```



```
{
	"query": {
		"match": {"singerName":"张学友"
		}
	},
    "_source": [
		"singerName",
		"songName",
		"id",
		"lyric",
		"lyricList"
	]
}
```





## 基本查询

### 正则匹配

```
{
    "query": {
        "wildcard": {
            "_id": "^[^0-9]" 
        }
    },
    "_source": [
		"songAndSingerName",
		"albumName",
		"tenListenNum",
		"combineMultiValues"
	]
}
```

### 指定analyzer

```
{
	"query": {
		"match": {"lyric":{"query": "还是不是当初的冬天 ","analyzer": "keyword"}
		}
	}
}
```

### 搜歌名

```
{
	"query": {
		"match": {"singerName":"张学友"
		}
	}
}
```

### 搜歌手

```
{
	"query": {
		"term": {"id":2814
		}
	}
}
```

### 随便返回几条

```json
{
	"query": {
		"match_all": {
		}
	},
    "_source": [
		"l_play_user_num",
    "m_play_user_num",
		"s_play_user_num",
		"like_num"
	]
}
```



```json
{
	"query": {
		"term": {
			"id": 100000491
		}
	},
	"_source": [
		
	]
}
```



## 分析器

```
{"analyzer":"ik_smart","text":"最甜情:-"}
```



# 案例

## badcase1

### 现象

高亮3个字的排在高亮2个字的前面

<img src="C:\Users\11109539\AppData\Roaming\Typora\typora-user-images\image-20200315120415110.png" alt="image-20200315120415110" style="zoom:33%;" />

### 分析

首先说明查询json结构，往下还有详细json

<img src="C:\Users\11109539\AppData\Roaming\Typora\typora-user-images\image-20200315164327217.png" alt="image-20200315164327217" style="zoom:35%;" />

指定functionScore，返回字段，

- from 返回起始文档

- size 返回文档上限

- query

  ​	function_score表示这是一个使用function打分的查询

  ​		query内定义匹配条件，满足条件才能返回

  ​		functions中定义6个打分函数，

  ​			1）match查询 songAndSingerName，weight为4

  ​			2）match查询 songNameLowerCase，weight为2

  ​			3）term查询 songNameLowerCase.primitive，weight为5

  ​			4）match查询 singerNameLowerCase，weight为1.5

  ​			5）match查询 albumName， weight为1

  ​			6）match查询 combineMultiValue， weight为3

  ​		score_mode 定义6个函数打分融合方式

  ​		boost_mode 定义文档匹配分 与 函数打分融合方式

- fields定义返回字段

- sort定义排序逻辑   这里按照 status，recentListenNum，listennum 降序排序，没有考虑_score（虽然定义了打分方法，但排序时_score：{}，说明不按照_score排序）

- highlight定义高亮标签和字段

详细json如下，用这个工具看吧 https://c.runoob.com/front-end/53，或者head

```
{
  "from" : 0,
  "size" : 10,
  "query" : {
    "function_score" : {
      "query" : {
        "bool" : {
          "filter" : {
            "bool" : {
              "must_not" : {
                "term" : {
                  "blackStatus" : 1
                }
              }
            }
          },
          "should" : {
            "multi_match" : {
              "query" : "最甜情",
              "fields" : [ "songName", "singerName", "albumName", "songAndSingerName^6" ],
              "operator" : "OR",
              "analyzer" : "ik_smart",
              "minimum_should_match" : "50%"
            }
          },
          "minimum_should_match" : "1"
        }
      },
      "functions" : [ {
        "filter" : {
          "multi_match" : {
            "query" : "最甜情",
            "fields" : [ "songAndSingerName" ],
            "operator" : "AND",
            "analyzer" : "ik_smart"
          }
        },
        "weight" : 4.0
      }, {
        "filter" : {
          "multi_match" : {
            "query" : "最甜情",
            "fields" : [ "songNameLowerCase" ],
            "operator" : "AND",
            "analyzer" : "ik_smart"
          }
        },
        "weight" : 2.0
      }, {
        "filter" : {
          "terms" : {
            "songNameLowerCase.primitive" : [ "最甜情" ]
          }
        },
        "weight" : 5.0
      }, {
        "filter" : {
          "multi_match" : {
            "query" : "最甜情",
            "fields" : [ "singerNameLowerCase" ],
            "operator" : "AND",
            "analyzer" : "ik_smart"
          }
        },
        "weight" : 1.5
      }, {
        "filter" : {
          "multi_match" : {
            "query" : "最甜情",
            "fields" : [ "albumName" ],
            "operator" : "AND",
            "analyzer" : "ik_smart"
          }
        },
        "weight" : 1.0
      }, {
        "filter" : {
          "multi_match" : {
            "query" : "最甜情",
            "fields" : [ "combineMultiValue" ],
            "operator" : "AND",
            "analyzer" : "ik_smart"
          }
        },
        "weight" : 3.0
      } ],
      "score_mode" : "sum",
      "boost_mode" : "sum"
    }
  },
  "explain" : false,
  "fields" : [ "id", "songName", "albumId", "albumName", "singerId", "singerName", "dgitalAlbum", "listenNum", "recentListenNum", "status" ],
  "sort" : [ {
    "status" : {
      "order" : "desc"
    }
  }, {
    "_score" : { }
  }, {
    "recentListenNum" : {
      "order" : "desc"
    }
  }, {
    "listenNum" : {
      "order" : "desc"
    }
  } ],
  "highlight" : {
    "pre_tags" : [ "<font color=red>" ],
    "post_tags" : [ "</font>" ],
    "fields" : {
      "songName" : { },
      "singerName" : { },
      "albumName" : { }
    }
  }
}
```



### 解决

## badcase2

### 现象

用户想通过歌词搜一首歌，但返回的歌曲 是  songname为搜索搜索词且并不热门的歌曲

<img src="C:\Users\11109539\AppData\Roaming\Typora\typora-user-images\image-20200317154755334.png" alt="image-20200317154755334" style="zoom:45%;" />

<img src="C:\Users\11109539\AppData\Roaming\Typora\typora-user-images\image-20200317154724545.png" alt="image-20200317154724545" style="zoom:25%;" />

### 分析

lyric搜索缺失，在如下代码中

```
public void initBoolQueryBuilder(SearchParam searchParam,SearchRequestBuilder searchRequestBuilder,
```

包含 歌手 歌曲 tag video album 和 other搜索。当intent为lyric时，走other。不合理

### 解决

增加lyric意图搜索。

召回：lyric召回 + songName召回

排序：根据热度 or 分别排序后各取topN，然后再排序

# 使用

### 逻辑操作 operator

查询中包含张 且 包含三

{
    "query":{
        "match":{
            "name":{
                "query":"张三",
                "operator":"and"
            }
        }
    }
}

### analyzer测试

<img src="C:\Users\11109539\AppData\Roaming\Typora\typora-user-images\image-20200314221158969.png" alt="image-20200314221158969" style="zoom:50%;" />

{"analyzer":"ik_smart",
"text":"最甜情"
}

# 排序

## 特征

query features 

relevance features 

doc quality

ctr stats

recency features

# gxq

## 组合字段

1 查询时又使用组合字段又使用单个字段，为什么

{
  "multi_match" : {
    "query" : "最甜情",
    "fields" : [ "songName", "singerName", "albumName", "songAndSingerName^6" ],
    "operator" : "OR",
    "analyzer" : "ik_smart"
  }
}

用户搜索词包含歌手和歌名，就要用组合字段了

2 为何不用copy_to而是手动拼接？ 为了对字段做更复杂处理，而不仅仅是singerlowername



## 歌曲搜索时考虑album名称

分别将 歌曲 歌手 专辑 。。搜索结果拼到一起。为何在搜索歌曲的时候还要考虑album关键字？

## index中 处理前缀 dealThesuffixAndGetStr

1 注释掉了这些。挺有用，为什么不用了？
//                str = str.replaceAll("\\(.*\\)|\\（.*\\）|\\（.*\\)|\\(.*\\）", "").trim();

正则性能差，性能还是java好

2 es索引时，songname保存的是过滤    “（**版本）”的歌名。若用户想搜索特定版本，就无法检索了

## 两个dictionary

没有NLP服务的时候使用；有了NLP服务就不需要了，用于容错



## 版本折叠



## sort字段返回多个字段的匹配得分

```
 "sort" : [ {
    "status" : {
      "order" : "desc"
    }
  }, {
    "_score" : { }
  }, {
    "recentListenNum" : {
      "order" : "desc"
    }
  }, {
    "listenNum" : {
      "order" : "desc"
    }
  } ],
```

<img src="C:\Users\11109539\AppData\Roaming\Typora\typora-user-images\image-20200318145732265.png" alt="image-20200318145732265" style="zoom:50%;" />


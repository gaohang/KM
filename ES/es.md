# 实例用法



## 创建删除语句

### 查看集群索引

```
curl -X GET 10.192.46.139:11600/_cat/indices?v -H "Content-Type: application/json" 

curl -H "Content-Type: application/json" -XGET '10.192.46.139:11600/_cat/nodes?v'

curl  -H "Content-Type: application/json" -XGET '10.192.29.5:11600/_cluster/health?pretty'

curl -H "Content-Type: application/json" -XGET '10.192.29.5:11600/_nodes/stats?pretty'
```

### 删除索引

```
curl -H "Content-Type: application/json" -XDELETE 10.192.29.5:11600/musicsearch_playlist_v1.1?pretty
curl -H "Content-Type: application/json" -XDELETE 10.192.29.5:11600/musicsearch_fm_v1.1?pretty
curl -H "Content-Type: application/json" -XDELETE 10.192.29.5:11600/musicsearch_suggest_v1.1?pretty
```



### 创建索引（settings mappings）

mini实例

```
curl -H "Content-Type: application/json" -XPUT 10.192.29.5:11600/musicsearch_song_v1.1 -d'
{
	"mappings": {
		"music_song_vivo": {
			"_all": {
				"enabled": false
			},
			"properties": {
				"id": {
					"type": "long",
					"index": "not_analyzed"
				},
				"albumId": {
					"type": "long",
					"index": "not_analyzed"
				},
				"singerId": {
					"type": "long",
					"index": "not_analyzed"
				},
				"songName": {
					"type": "string",
					"analyzer": "ik_smart",
					"fields": {
						"primitive": {
							"type": "string",
							"index": "not_analyzed"
						}
					},
					"fielddata": {
						"loading": "eager"
					}
				},
				"translateName": {
					"type": "string",
					"analyzer": "ik_smart"
				},
				"singerName": {
					"type": "string",
					"analyzer": "ik_smart",
					"fields": {
						"primitive": {
							"type": "string",
							"index": "not_analyzed"
						}
					},
					"fielddata": {
						"loading": "eager"
					}
				},
				"singerTranslateName": {
					"type": "string",
					"analyzer": "ik_smart",
					"fields": {
						"primitive": {
							"type": "string",
							"index": "not_analyzed"
						}
					}
				},
				"songAndSingerName": {
					"type": "string",
					"analyzer": "ik_smart",
					"fields": {
						"primitive": {
							"type": "string",
							"index": "not_analyzed"
						}
					},
					"fielddata": {
						"loading": "eager"
					}
				},
				"albumName": {
					"type": "string",
					"analyzer": "ik_smart",
					"fielddata": {
						"loading": "eager"
					}
				},
				"albumTranslateName": {
					"type": "string",
					"analyzer": "ik_smart"
				},
				"albumNameLowerCase": {
					"type": "string",
					"index": "not_analyzed",
					"fielddata": {
						"loading": "eager"
					},
					"store": false
				},
				"songNameLowerCase": {
					"type": "string",
					"analyzer": "ik_smart",
					"fields": {
						"primitive": {
							"type": "string",
							"index": "not_analyzed"
						}
					},
					"fielddata": {
						"loading": "eager"
					},
					"store": false
				},
				"singerNameLowerCase": {
					"type": "string",
					"analyzer": "ik_smart",
					"fields": {
						"primitive": {
							"type": "string",
							"index": "not_analyzed"
						}
					},
					"fielddata": {
						"loading": "eager"
					}
				},
				"dgitalAlbum": {
					"type": "boolean",
					"fielddata": {
						"loading": "eager"
					}
				},
				"albumSmallImage": {
					"type": "string",
					"index": "not_analyzed"
				},
				"smallImage": {
					"type": "string",
					"index": "not_analyzed"
				},
				"status": {
					"type": "integer",
					"index": "not_analyzed",
					"fielddata": {
						"loading": "eager"
					}
				},
				"thirdId": {
					"type": "string",
					"index": "not_analyzed"
				},
				"source": {
					"type": "integer",
					"index": "not_analyzed"
				},
				"listenNum": {
					"type": "long",
					"index": "not_analyzed",
					"fielddata": {
						"loading": "eager"
					}
				},
				"likeNum": {
					"type": "long",
					"index": "not_analyzed"
				},
				"singerHot": {
					"type": "long",
					"index": "not_analyzed"
				},
				"hot": {
					"type": "integer",
					"index": "not_analyzed"
				},
				"genre": {
					"type": "string",
					"index": "not_analyzed",
					"fielddata": {
						"loading": "eager"
					}
				},
				"blackStatus": {
					"type": "integer",
					"index": "not_analyzed",
					"fielddata": {
						"loading": "eager"
					}
				},
				"genreList": {
					"index": "not_analyzed",
					"type": "string",
					"fielddata": {
						"loading": "eager"
					}
				},
				"tags": {
					"type": "string",
					"analyzer": "ik_smart",
					"fields": {
						"primitive": {
							"type": "string",
							"index": "not_analyzed"
						}
					},
					"fielddata": {
						"loading": "eager"
					}
				},
				"recentListenNum": {
					"type": "long",
					"index": "not_analyzed"
				},
				"combineMultiValue": {
					"type": "string",
					"analyzer": "ik_max_word",
					"fields": {
						"primitive": {
							"type": "string",
							"index": "not_analyzed"
						}
					},
					"fielddata": {
						"loading": "eager"
					}
				},
				"subtitle": {
					"type": "string",
					"analyzer": "ik_smart",
					"fields": {
						"primitive": {
							"type": "string",
							"index": "not_analyzed"
						}
					}
				},
				"tenListenNum": {
					"type": "integer",
					"index": "not_analyzed",
					"fielddata": {
						"loading": "eager"
					}
				},
				"duration": {
					"type": "long",
					"index": "not_analyzed",
					"fielddata": {
						"loading": "eager"
					}
				},
				"sqSize": {
					"type": "integer",
					"index": "not_analyzed",
					"fielddata": {
						"loading": "eager"
					}
				},				
				"quality": {
					"type": "string",
					"index": "not_analyzed",
					"fielddata": {
						"loading": "eager"
					}
				},
				"lyric": {
					"type": "string",
					"analyzer": "ik_smart",
					"fields": {
						"primitive": {
							"type": "string",
							"index": "not_analyzed"
						}
					},
					"fielddata": {
						"loading": "eager"
					}
				}
			}
		}
	},
	"settings": {
		"analysis": {
			"analyzer": {
				"caseSensitive": {
					"filter": "lowercase",
					"type": "custom",
					"tokenizer": "keyword"
				}
			}
		},
		"index": {
			"analysis": {
				"analyzer": {
					"my_pinyin_full_analyzer": {
						"tokenizer": "my_pinyin_full_tokenizer"
					},
					"my_pinyin_first_analyzer": {
						"tokenizer": "pinyin_first_letter"
					},
					"my_pinyin_analyzer": {
						"filter": [
							"word_delimiter"
						],
						"tokenizer": "my_pinyin_tokenizer"
					}
				},
				"tokenizer": {
					"my_pinyin_tokenizer": {
						"type": "pinyin",
						"padding_char": " ",
						"first_letter": "append"
					},
					"my_pinyin_full_tokenizer": {
						"type": "pinyin",
						"padding_char": "",
						"first_letter": "none"
					}
				}
			},
			"number_of_shards": "5",
			"number_of_replicas": "0"
		}
	}
}'
```

完整实例

```
curl -H "Content-Type: application/json" -XPUT 10.192.40.140:11408/musicsearch_song_v1.2 -d'
{
  "settings": {
    "index": {
      "number_of_shards": "5",
      "number_of_replicas": "1"
    },
    "analysis": {
      "filter": {
        "trigrams_filter": {
          "type": "ngram",
          "min_gram": 1,
          "max_gram": 3
        },
        "ngf3": {
          "type": "edge_ngram",
          "min_gram": 3,
          "max_gram": 100
        }
      },
      "char_filter": {
        "my_char_filter": {
          "type": "mapping",
          "mappings": [
            ",=> ",
            "，=> "
          ]
        }
      },
      "analyzer": {
        "trigrams": {
          "type": "custom",
          "tokenizer": "ik_smart",
          "filter": [
            "stemmer",
            "trigrams_filter"
          ]
        },
        "optimized_ik_smart": {
          "type": "custom",
          "tokenizer": "ik_smart",
          "filter": [
            "stemmer"
          ]
        },
        "optimized_ik_max_word": {
          "type": "custom",
          "tokenizer": "ik_max_word",
          "filter": [
            "stemmer"
          ]
        },
        "lyric_analyzer_reverse": {
          "type": "custom",
          "char_filter": "my_char_filter",
          "tokenizer": "whitespace",
          "filter": [
            "reverse",
            "ngf3",
            "reverse"
          ]
        },
        "lyric_analyzer": {
          "type": "custom",
          "char_filter": "my_char_filter",
          "tokenizer": "whitespace",
          "filter": [
            "ngf3"
          ]
        }
      }
    }
  },
  "mappings": {
  "music_song_vivo": {
    "properties": {
      "boutique":{
        "type": "integer"
      },
      "songName": {
        "fielddata": true,
        "analyzer": "ik_smart",
        "type": "text",
        "fields": {
          "primitive": {
            "type": "keyword"
          },
          "trigrams": {
            "type": "text",
            "analyzer": "trigrams"
          }
        }
      },
      "albumName": {
        "fielddata": true,
        "analyzer": "ik_smart",
        "type": "text"
      },
      "combineMultiValue": {
        "fielddata": true,
        "analyzer": "ik_max_word",
        "type": "text",
        "fields": {
          "primitive": {
            "type": "keyword"
          }
        }
      },
      "smallImage": {
        "type": "keyword"
      },
      "songAndSingerName": {
        "fielddata": true,
        "analyzer": "ik_smart",
        "type": "text",
        "fields": {
          "primitive": {
            "type": "keyword"
          }
        }
      },
      "docId": {
        "type": "long"
      },
      "albumId": {
        "store": true,
        "type": "long"
      },
      "genreList": {
        "type": "keyword"
      },
      "source": {
        "type": "integer"
      },
      "hot": {
        "type": "integer"
      },
      "singerTranslateName": {
        "analyzer": "ik_smart",
        "type": "text",
        "fields": {
          "primitive": {
            "type": "keyword"
          }
        }
      },
      "likeNum": {
        "type": "long"
      },
      "duration": {
        "type": "long"
      },
      "singerName": {
        "fielddata": true,
        "analyzer": "ik_smart",
        "store": true,
        "type": "text",
        "fields": {
          "primitive": {
            "type": "keyword"
          },
          "trigrams": {
            "type": "text",
            "analyzer": "trigrams"
          }
        }
      },
      "singerId": {
        "store": true,
        "type": "long"
      },
      "genre": {
        "type": "keyword"
      },
      "singerNameLowerCase": {
        "fielddata": true,
        "analyzer": "ik_smart",
        "type": "text",
        "fields": {
          "primitive": {
            "type": "keyword"
          }
        }
      },
      "id": {
        "store": true,
        "type": "long"
      },
      "albumNameLowerCase": {
        "type": "keyword"
      },
      "dgitalAlbum": {
        "type": "boolean"
      },
      "sqSize": {
        "type": "integer"
      },
      "listenNum": {
        "type": "long"
      },
      "thirdId": {
        "type": "keyword"
      },
      "albumTranslateName": {
        "analyzer": "ik_smart",
        "type": "text"
      },
      "blackStatus": {
        "type": "integer"
      },
      "translateName": {
        "analyzer": "ik_smart",
        "type": "text"
      },
      "songNameLowerCase": {
        "fielddata": true,
        "analyzer": "ik_smart",
        "type": "text",
        "fields": {
          "primitive": {
            "type": "keyword"
          },
          "maxword": {
            "type": "text",
            "analyzer": "ik_max_word"
          }
        }
      },
      "quality": {
        "type": "keyword"
      },
      "tags": {
        "fielddata": true,
        "analyzer": "ik_smart",
        "type": "text",
        "fields": {
          "primitive": {
            "type": "keyword"
          }
        }
      },
      "albumSmallImage": {
        "type": "keyword"
      },
      "lyric": {
        "type": "text",
        "fields": {
          "keyword": {
            "ignore_above": 256,
            "type": "keyword"
          }
        }
      },
      "recentListenNum": {
        "type": "long"
      },
      "subtitle": {
        "analyzer": "ik_smart",
        "type": "text",
        "fields": {
          "primitive": {
            "type": "keyword"
          }
        }
      },
      "tenListenNum": {
        "type": "integer"
      },
      "singerHot": {
        "type": "long"
      },
      "status": {
        "type": "integer"
      },
      "listen_num": {
        "type": "integer"
      },
      "like_num": {
        "type": "integer"
      },
      "l_user_play_cnt": {
        "type": "integer"
      },
      "s_user_play_cnt": {
        "type": "integer"
      }
    }
  }
}
}'
```

### 创建别名

```
curl -H "Content-Type: application/json" -XPOST 'http://10.192.46.139:11600/_aliases' -d '
{
    "actions" : [
        { "add" : { "index" : "musicsearch_suggest_v1.0", "alias" : "musicsearch_suggest" }},
        { "add" : { "index" : "musicsearch_fm_v1.0", "alias" : "musicsearch_fm" }},
        { "add" : { "index" : "musicsearch_playlist_v1.0", "alias" : "musicsearch_playlist" }},
        { "add" : { "index" : "musicsearch_song_v1.1", "alias" : "musicsearch_song" }},
        { "add" : { "index" : "musicsearch_singer_v1.1", "alias" : "musicsearch_singer" }},
        { "add" : { "index" : "musicsearch_album_v1.1", "alias" : "musicsearch_album" }}
    ]
}'
```

### 查看别名

```
curl -H "Content-Type: application/json" -XGET 'http://10.192.11.136:11404/musicsearch_song_v1.0/_alias/*?pretty' 
```

### 切换别名

```
curl -H "Content-Type: application/json" -XPOST 'http://10.192.12.135:11404/_aliases' -d '
{
    "actions" : [
        {"remove" : { "index" : "musicsearch_song_v1.2", "alias" : "tmp_musicsearch_song"}},
        { "remove" : { "index" : "musicsearch_top_song_v1.2", "alias" : "tmp_musicsearch_top_song" }}
    ]
}'
```

### 按照条件删除doc

1）先count

```
curl -X POST "10.192.12.135:11404/musicsearch_top_song/_count?pretty" -H 'Content-Type: application/json'  -d'
{
	"query": {
		"bool": {
			"must_not": {
				"exists": {
					"field": "songName"
				}
			}
		}
	}
}
'
```

### 2）再删除

```
curl -X POST "10.192.12.135:11404/musicsearch_top_song/_delete_by_query?pretty" -H 'Content-Type: application/json'  -d'
{
	"query": {
		"bool": {
			"must_not": {
				"exists": {
					"field": "songName"
				}
			}
		}
	}
}
'
```





## 文档操作

### 创建文档



### 更新文档

```
curl -H "Content-Type: application/json" -XPOST 'http://10.192.12.135:11404/_update' -d '
{
    "upsert": {
    "status": 1
  }
}'
```



```
curl -H "Content-Type: application/json" -XPOST 'http://10.192.12.135:11404/musicsearch_song_1.0/384933848/_update' -d '{
   "doc" : {
      "tags" : [ "testing" ],
      "views": 0
   }
}
```



## 查询语句

### term

```json
{
	"query": {
		"term": {
			"id": "529264941"
		}
	}
}
```



### match

```json
{
	"query": {
		"multi_match": {
			"query": "最甜情",
			"fields": [
				"songName",
				"singerName",
				"albumName",
				"songAndSingerName^6"
			],
			"operator": "OR",
			"analyzer": "ik_smart"
		}
	}
}
```



### _source

```
{
	"query": {
		"term": {
			"singer_name": "权志龙"
		}
	},"_source": {
		"include": [
			"song_name",
			"singer_name"
		]
	}
}
```

### bool

```json
{
	"query": {
		"bool": {
			"must": [
				{
					"match": {
						"songName": "下山"
					}
				},
				{
					"match": {
						"singerName": "要不要买菜"
					}
				}
			]
		}
	},
	"_source": {
		"include": [
			"songName",
			"singerName",
			"status",
			"listen_num",
			"lyricList"
		]
	}
}
```

```
{
	"query": {
		"bool": {
			"must": [
				{
					"match": {
						"songName": "少年"
					}
				},
				{
					"match": {
						"singerName": "梦然"
					}
				}
			]
		}
	},
	"_source": {
		"include": [
			"songName",
			"singerName",
			"status",
			"listen_num",
			"lyricList"
		]
	}
}
```





### exists

```
{
    "query": {
        "exists": {
            "field": "lyric"
        }
    }
}
```

返回所有

```
{
    "query": {
        "match_all": {}
    }
}
```

range

```
{
  "query": {
    "range": {
      "listen_num": {
        "gte": 0, 
        "lte": 1000000000
      }
    }
  }
}

```

### match_phrase

```
{
    "query": {
        "match_phrase": {
        "lyricList":{
            "query": "漫漫黄沙气势",
            "slop":  1
        }}
    },
    "_source": {
		"include": [
			"songName",
			"singerName",
			"status",
			"listen_num",
			"lyricList"
		]
	}
}
```

### match_phrase打分

must条件较宽泛，用于召回；should条件较严格，用于相关性打分

```
{
  "query": {
    "bool": {
      "must": {
        "match": { 
          "lyricList": {
            "query":                "在夜深人静的时候想起他",
            "minimum_should_match": "-20%"
          }
        }
      },
      "should": {
        "match_phrase": { 
          "lyricList": {
            "query": "在夜深人静的时候想起他",
            "slop":  2
          }
        }
      }
    }
  },
	"_source": {
		"include": [
			"id",
			"songName",
			"singerName",
			"lyricList"
		]
	}
}
```



### fuzzy 

注意：

- fuzzy只能对keyword字段匹配
- 耗时较长，200w歌词匹配耗时在100ms级别。

```
{
	"query": {
		"fuzzy": {
			"lyricList.keyword": {
				"value": "好啊给我一杯忘情水",
				"fuzziness": 3
			}
		}
	},
	"_source": {
		"include": [
			"songName",
			"singerName",
			"lyricList"
		]
	}
}
```



## 快照操作

### 建立snapshot glusterfs仓库

```
curl -X PUT "10.192.11.136:11404/_snapshot/my_backup" -H 'Content-Type: application/json' -d'
{
    "type": "fs", 
    "settings": {
        "location": "/backup/mysql_data_backup/es/ai-music-index-165" 
    }
}'
```

### 建立snapshot local仓库

1）先登录机器，创建 存放快照仓库的路径 /data/

2）运行以下命令

```
curl -H "Content-Type: application/json" -XPUT  10.192.40.140:11408/_snapshot/musicsearch_bk -d'
{
    "type": "fs", 
    "settings": {
        "location": "/musicsearch_bk" 
    }
}'
```

### 查看 repository

```
curl -H "Content-Type: application/json" -XGET 10.192.11.136:11404/_cluster/stats?pretty
```

```
curl -H "Content-Type: application/json" -XGET 10.192.11.136:11404/_snapshot/_all?pretty
```

### 做快照

对cluster中的所有index

```
curl -H "Content-Type: application/json" -XPUT 10.192.11.136:11404/_snapshot/my_backup/snapshot_1?wait_for_completion=true
```

仅对指定index做快照

```
curl -H "Content-Type: application/json" -XPUT 10.192.11.136:11404/_snapshot/my_backup/snapshot_2?wait_for_completion=true&pretty" -d'
{
  "indices": "search_musicsong_v1",
  "ignore_unavailable": true,
  "include_global_state": false,
  "metadata": {
    "taken_by": "xxx",
    "taken_because": "test snapshot"
  }
}'

```

### 恢复

```
curl -H "Content-Type: application/json" -XPOST 10.192.11.136:11404/_snapshot/my_backup/snapshot_2/_restore?pretty
```

### 删除

```
curl -H "Content-Type: application/json" -XDELETE 10.192.11.136:11404/_snapshot/my_backup/snapshot_2?pretty
```

查看snapshot

```
curl -H "Content-Type: application/json" -XGET 10.192.11.136:11404/_snapshot/my_backup/snapshot_2/_status?pretty
```



# 索引（Index）

### 数据类型

https://www.elastic.co/guide/cn/elasticsearch/guide/current/complex-core-fields.html  复杂数据类型

index中的field类型种类：

NULL值；字符串list；整数list；map；嵌套

### 字段属性

#### 预加载

 "fielddata": true,

加载内存 fielddata 的默认行为是 *延迟* 加载 。 当 Elasticsearch 第一次查询某个字段时，它将会完整加载这个字段所有 Segment 中的倒排索引到内存中，以便于以后的查询能够获取更好的性能。

#### 倒排索引中包含field value

Doc Values` 是在索引时创建的，当字段索引时，`Elasticsearch` 为了能够快速检索，会把字段的值加入倒排索引中，同时它也会存储该字段的 `Doc Values`。

### Source filtering

If you only want to retrieve the value of a single field or of a few fields, instead of the whole `_source`, then this can be achieved with [source filtering](https://www.elastic.co/guide/en/elasticsearch/reference/current/run-a-search.html#source-filtering). 以下默认在匹配后返回的字段不是全部字段，而是title和date。

```
 "mappings": {
    "properties": {
      "title": {
        "type": "text",
        "store": true 
      },
      "date": {
        "type": "date",
        "store": true 
      },
      "content": {
        "type": "text"
      }
    }
  }
```





### 分析器（analyzer）

Analyzer 一般由三部分构成，character filters、tokenizers、token filters。掌握了 Analyzer 的原理，就可以根据应用场景配置 Analyzer。

Elasticsearch 有10种分词器（Tokenizer）、31种 token filter，3种 character filter，一大堆配置项。此外，还有还可以安装 plugin 扩展功能。这些都是搭建 analyzer 的原材料。

#### 流程

1. 使用字符过滤器CharacterFilters对文档中的不需要的字符过滤（例如html语言的<br/>等等）
2. 用Tokenizer分词器大段的文本分成词（Tokens）（例如可以空格基准对一句话进行分词）
3. 用TokenFilter在对分完词的Tokens进行过滤、处理（比如除去英文常用的量词：a，the，或者把去掉英文复数等）

<img src=".\img\analyzerDemo.png" alt="image-20200216084924241" style="zoom:75%;" />

#### 自定义分析器

```js
"settings": {
        "analysis": {
            "char_filter": { ... custom character filters ... },
            "tokenizer":   { ...    custom tokenizers     ... },
            "filter":      { ...   custom token filters   ... },
            "analyzer":    { ...    custom analyzers      ... }
        }
    }
```

如上格式，我们可以基于es自带或ik、pinyin、stconvert等插件分析器定义自定义analyzer。



#### 插件分析器

##### ik

https://github.com/medcl/elasticsearch-analysis-ik

支持自定义词典、停用词

- ik_max_word 细粒度，默认将“华为手机”->“华为手机”。当将“华为手机”添加到自定义词库后，分析结果：“华为”，“手机”，“华为手机”。
- ik_smart 粗粒度，默认将“华为手机”->“华为手机”。当将“华为手机”添加到自定义词库后，分析结果：“华为手机”。
- 索引时，要提高覆盖范围，通常采用ik_max_word分析器，以最细粒度分词；
- 搜索时，要提高准确度，通常采用ik_smart分析器，以粗粒度分词

##### pinyin

https://github.com/medcl/elasticsearch-analysis-pinyin

- `limit_first_letter_length` set max length of the first_letter result, default: 16
- `keep_full_pinyin` when this option enabled, eg: `刘德华`> [`liu`,`de`,`hua`], default: true





##### stconvert



### multi_fields

目的是给一个字段设置不同的类型或分析器。如下，city是文本类型，city.raw是关键字类型。

```console
"mappings": {
    "properties": {
      "city": {
        "type": "text",
        "fields": {
          "raw": { 
            "type":  "keyword"
          }
        }
      }
    }
  }
```

### 组合字段

_all字段

将所有字段以空格连接拼成一个大字段。默认不开启false。当不知道文档都有哪些字段时使用。

- 可以通过设置minmum_should_match放宽匹配条件

- 通过copy_to，实现自定义_all字段

  copy_to字段 以空格为分隔符组合成一个大字符串，然后被分析和索引，但是不存储。i.e. 它能被查询，但不能被取回显示。

_source字段

包含所有字段。不能被index。只是存储。

### 预加载

https://www.elastic.co/guide/cn/elasticsearch/guide/current/preload-fielddata.html

Elasticsearch 加载内存 fielddata 的默认行为是 *延迟* 加载 。 当 Elasticsearch 第一次查询某个字段时，它将会完整加载这个字段所有 Segment 中的倒排索引到内存中，以便于以后的查询能够获取更好的性能。



### ignore_above

对超过 `ignore_above` 的字符串，analyzer 不会进行处理；所以就不会索引起来。导致的结果就是最终搜索引擎搜索不到了。这个选项主要对 `not_analyzed` 字段有用，这些字段通常用来进行过滤、聚合和排序。而且这些字段都是结构化的，所以一般不会允许在这些字段中索引过长的项。

```bash
PUT my_index
{
  "mappings": {
    "my_type": {
      "properties": {
        "message": {
          "type": "string",
          "index": "not_analyzed",
          "ignore_above": 20                ...... (1)
        }
      }
    }
  }
}

PUT my_index/my_type/1               ...... (2)
{
  "message": "Syntax error"
}

PUT my_index/my_type/2               ...... (3)
{
  "message": "Syntax error with some long stacktrace"
}

GET _search                                    ...... (4)
{
  "aggs": {
    "messages": {
      "terms": {
        "field": "message"
      }
    }
  }
}
```

### JavaList 写入 String字段

javaList中任一一个元素匹配即可。以歌词索引和检所为例说明，一下analyzer为standard，对中文逐字切分

当以text方式索引时，检索通过match_phrase

![image-20200613150830499](D:\Users\11109539\km\ES\javalist-phrase.png)

以keyword方式索引时，能够使用fuzzy匹配方式

检索时，只要匹配list中的任一文本即可，如下，lyricList字段，索引时写入一个javaList；检索“是不是我们都不”时，即可匹配，此法可实现歌词模糊匹配

![image-20200613150949936](D:\Users\11109539\km\ES\javalist-fuzzy.png)



# 查询（Query）

## 基本查询方式

### 精确匹配 term

也称结构化查询。查询时不对query分词，要求：query=field value

### 匹配 match

也成模糊查询。查询时先对query分词；再用分词进行查询。要求：(or：只要有一个分词查到就算匹配，and：所有分词出现在field value的分词集合中) 。

### 短语匹配 match_phrase

介于以上两种方式之间。先对query分词，再用分词查询。默认匹配要求：所有分词必须查到且出现顺序相同。

1. match_phrase还是分词后去搜的
2. 目标文档需要包含分词后的所有词
3. 目标文档还要保持这些词的相对顺序和文档中的一致

可通过slop参数放松要求

`slop` 参数告诉 `match_phrase` 查询词条相隔多远时仍然能将文档视为匹配 。 相隔多远的意思是为了让查询和文档匹配你需要移动词条多少次？

用例：为了让查询 `quick fox` 能匹配一个包含 `quick brown fox` 的文档， 我们需要 `slop` 的值为 `1`:

```
            Pos 1         Pos 2         Pos 3
-----------------------------------------------
Doc:        quick         brown         fox
-----------------------------------------------
Query:      quick         fox
Slop 1:     quick                 ↳     fox
```

用例：使用phrase查询控制相关度

```
{
  "query": {
    "bool": {
      "must": {
        "match": { 
          "lyricList": {
            "query":                "在夜深人静的时候想起他",
            "minimum_should_match": "-20%"
          }
        }
      },
      "should": {
        "match_phrase": { 
          "lyricList": {
            "query": "在夜深人静的时候想起他",
            "slop":  2
          }
        }
      }
    }
  },
	"_source": {
		"include": [
			"id",
			"songName",
			"singerName",
			"lyricList"
		]
	}
}
```

用例

```
{
	"query": {
		"bool": {
			"must": {
				"match": {
					"lyricList": {
						"query": "要记得回家",
						"minimum_should_match": "-20%"
					}
				}
			},
			"should": [
				{
					"match_phrase": {
						"lyricList": {
							"query": "要记得回家",
							"slop": 0
						}
					}
				},
				{
					"term": {
						"lyricList": "要记得回家"
					}
				}
			]
		}
	},
	"_source": {
		"include": [
			"id",
			"songName",
			"singerName",
			"lyricList"
		]
	}
}
```



### 模糊匹配fuzzy

通过计算query和doc的编辑距离实现，其中，fuzziness参数指定编辑距离。由于需要计算编辑距离，因此性能相对其他查询较差。通过参数**prefix_length**（前n项必须匹配）和**max_expansions（**最多匹配到的doc数目）提高性能。

```
{
  "query": {
    "fuzzy": {
      "text": {
        "value": "surprize",
        "fuzziness": 1
      }
    }
  }
}
```







## 复杂查询

### 多词查询

多词查询就是match匹配。如下，首先，quick brown dog被切分为quick、brown和dog。然后，三个term分别去匹配。

```sense
GET /my_index/my_type/_search
{
  "query": {
    "match": {
      "title": {
        "query":                "quick brown dog",
        "minimum_should_match": "75%"
      }
    }
  }
}
```

### 多字段查询（multi_match）

在多个字段（field）中查询。评分方法：

- best_field 使用场景：匹配度最高的字段作为多字段查询的匹配度
- most_field 使用场景：匹配的字段越多越好
- cross_field 使用场景：

### 查询组合（bool query）

- should (or) 默认方式
- must（and）
-  must_not

### 模糊匹配

es允许查询词和文档字段词不完全一样。根据编辑距离控制模糊度（fuzziness）

```json
"query": {
    "fuzzy": {
      "text": {
        "value": "surprize",
        "fuzziness": 1
      }
    }
  }
```

## 打分排序

参考：https://www.scienjus.com/elasticsearch-function-score-query/

### 基本方法

1. 全文搜索时，搜索结果默认会以文档的相关度（_score）进行排序

2. sort`指定一个或多个排序字段

3. functionScore 利用es自带函数打分

   应用场景：希望将文本匹配程度与其他字段要求综合考虑。例如，找餐厅：5公里内有wifi更好

   性能问题：fs仅仅对match返回的文档score，但还是增加了开销，特别是使用script自定义打分。

### FunctionScore用法

#### 实例1



```
{
  "query": {
    "function_score": {
      "query": {
        "match": {
          "title": "雨伞"
        }
      },
      "field_value_factor": {
        "field": "sales",
        "modifier": "log1p",
        "factor": 0.1
      },
      "boost_mode": "sum"
    }
  }
}
```

​	将标题中带有雨伞的商品检索出来，然后对这些文档计算一个与库存相关的分数，并与之前相关度的分数相加，对应的公式为：

```
_score = _score + log (1 + 0.1 * sales)
```

#### 实例2

大众点评的餐厅应用。该应用希望向用户推荐一些不错的餐馆，特征是：范围要在当前位置的 5km 以内，有停车位是最重要的，有 Wi-Fi 更好，餐厅的评分（1 分到 5 分）越高越好，并且对不同用户最好展示不同的结果以增加随机性。

```
{
  "query": {
    "function_score": {
      "filter": {  //匹配返回的记录必须满足这个filter
        "geo_distance": {
          "distance": "5km",
          "location": {
            "lat": $lat,
            "lon": $lng
          }
        }
      },
      "functions": [
        {
          "filter": {//functions中的filter是打分依据，区别于上面的匹配条件中的filter
            "term": {
              "features": "wifi"
            }
          },
          "weight": 1
        },
        {
          "filter": {
            "term": {
              "features": "停车位"
            }
          },
          "weight": 2
        },
        {
            "field_value_factor": {
               "field": "score",
               "factor": 1.2
             }
        },
        {
          "random_score": {
            "seed": "$id"
          }
        }
      ],
      "score_mode": "sum",
      "boost_mode": "multiply"
    }
  }
}
```

### BM25和TFIDF

https://my.oschina.net/stanleysun/blog/1617727

#### Lucence

早期的Lucence是直接把TF-IDF作为默认相似度。 新版的lucence采用了BM25(BM是Best Matching的意思)。BM25是基于TF-IDF并做了改进的算法。 BM25在传统TF-IDF的基础上增加了几个可调节的参数，使得它在应用上更佳灵活和强大，具有较高的实用性。

#### TF Score影响

从图中可以看到，当tf增加时，TF Score跟着增加，但是BM25的TF Score会被限制在0~k+1之间。它可以无限逼近k+1，但永远无法触达它。这在业务上可以理解为某一个因素的影响强度不能是无限的，而是有个最大值，这也符合我们对文本相关性逻辑的理解。 在Lucence的默认设置里，k＝1.2，使用者可以修改它。![img](https://static.oschina.net/uploads/space/2018/0202/211145_qLh0_2616203.gif)

![img](https://static.oschina.net/uploads/space/2018/0202/212839_lO3i_2616203.png)

#### 文档长度

 从图可看，文档越短，它逼近上限的速度越快，反之则越慢。这是可以理解的，对于只有几个词的内容，比如文章“标题”，只需要匹配很少的几个词，就可以确定相关性。而对于大篇幅的内容，比如一本书的内容，需要匹配很多词才能知道它的重点是讲什么。

![img](https://static.oschina.net/uploads/space/2018/0202/212913_GH8s_2616203.png)



### 联想建议

#### 实现联想功能的4种方法

https://medium.com/@mourjo_sen/a-detailed-comparison-between-autocompletion-strategies-in-elasticsearch-66cb9e9c62c4

- Approach #1: Prefix Query + Aggregations  较慢，通过**max_expansions**
- Approach #2: NGram Analyzer + Aggregations  耗内存，速度较快
- Approach #3: Completion Suggester  Trie树，速度最快
- Approach #4: Separate Index

elastic search 的suggesters用于查找相似的文本，7.6 doc中表示此功能尚在开发升级。

```
curl -X POST "localhost:9200/twitter/_search?pretty" -H 'Content-Type: application/json' -d'
{
  "query" : {
    "match": {
      "message": "tring out Elasticsearch"
    }
  },
  "suggest" : {
    "my-suggestion" : {
      "text" : "tring out Elasticsearch",
      "term" : {
        "field" : "message"
      }
    }
  }
}
'
```

4种类别的Suggester，分别是:

- Term Suggester
- Phrase Suggester
- Completion Suggester
- Context Suggester
- 

Google搜索框的补全/纠错功能，如果用ES怎么实现呢？我能想到的一个的实现方式:

1. 在用户刚开始输入的过程中，使用Completion Suggester进行关键词前缀匹配，刚开始匹配项会比较多，随着用户输入字符增多，匹配项越来越少。如果用户输入比较精准，可能Completion Suggester的结果已经够好，用户已经可以看到理想的备选项了。 
2. 如果Completion Suggester已经到了零匹配，那么可以猜测是否用户有输入错误，这时候可以尝试一下Phrase Suggester。
3. 如果Phrase Suggester没有找到任何option，开始尝试term Suggester。

精准程度上(Precision)看： Completion >  Phrase > term， 而召回率上(Recall)则反之。从性能上看，Completion Suggester是最快的，如果能满足业务需求，只用Completion Suggester做前缀匹配是最理想的。 Phrase和Term由于是做倒排索引的搜索，相比较而言性能应该要低不少，应尽量控制suggester用到的索引的数据量，最理想的状况是经过一定时间预热后，索引可以全量map到内存。



# 架构



## 分片和副本

https://www.zhihu.com/question/26446020

index有N个shard（i.e. 分片），分片有助于index横向扩展。好像一桶水（index的所有docs）分N个杯子（shard）装。每个shard是一个独立的lucence实例，searchRequest在每个shard独立执行。

replica shard 与 primary shard不会被放在同一个node上。若cluster只有一个node，则不会有replica。

当index的某个shard所在磁盘利用率超过95%时，index默认被置为 readonly。s
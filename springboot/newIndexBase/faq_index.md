#FAQ生成训练数据的接口文档

###一.创建ElasticSearch的索引库结构
**PUT**    **http://localhost:9200/qa_base**


	{
	  "settings" : {
			"index" : {
				"number_of_shards" : 5,
				"number_of_replicas" : 1
			}
	  },
	  "mappings": {
		"question" : {
		  "properties" : {
			"id" : {
			  "type" :    "long"
			},
			"bot" : {
			  "type" :   "keyword"
			},
			"qaid" : {
			  "type" :   "long"
			},
			"content" : {
			  "type" :   "text",
			  "analyzer": "ik_max_word"
			}
		  }
		}
	  }
	}
>id代表同一种问题为question在数据库中的id<br>
>bot是系统要求字段,在管理平台的操作都与特定bot相关<br>
>qaid用于标识给定标准问法和相似问题是同一性质问题<br>
>content为问题的内容，对该字段建立映射时,使用了ik中文分析器

###二.FAQ的训练数据接口文档

####1.生成训练数据的接口
GET http://localhost:8080/question/model/generate/${botName}

####2.上传语料数据text
POST http://localhost:8080/questions/txt/${botName}<br>
上传文件表单数据的key为file<br>
文件内容示例:
```
问题的内容1 标准问法 关键词 答案
问题的内容2 相似问法 关键词 答案
问题内容3  标准问法  关键词 答案
```
每一行的每个段落必须用\t进行分隔

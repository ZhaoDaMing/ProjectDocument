语料模块接口文档
=======
${corpusId} 的意思是corpusId为一个变量 <br>
eg:
- $bot/{botName}/corpus
- if botName="wali" --> bot/wali/corpus
------

添加语料
------
**Request Line**
```
POST    http://localhost:8080/bot/${botName}/corpus
```
**Request Body**
```
{
  "content": "测试添加方法语料的内容",
  "annotations": [
    {
      "from": 1,
      "to": 3,
      "category": "Annotation分类1",
      "type": "type啊"
    },
    {
      "from": 5,
      "to": 9,
      "category": "Annotation分类2",
      "type": "type啊"
    }
    ],
    "status": "UNCONFIRMED",
    "intents": [
    {
      "intent": "测试",
      "tag": "NEGATIVE"
    }
  ]
}
```
修改语料
----
**Request Line**
```
PUT http://localhost:8080/corpus/${corpusId}
```
**Request Body**
```
{
  "intents": [
    {
      "id": null,
      "intent": "测试修改哈哈",
      "tag": "POSITIVE"
    }
  ],
  "annotations": [
    {
      "from": 24,
      "to": 38,
      "category": "分类修改了2",
      "type": "type啊"
    }
  ]
}
```

删除语料的意图
----
**Request Line**
```
DELETE http://localhost:8080/corpus/intent/{corpusIntentId}
```

批量添加语料（txt文件上传）
----
**Request Line**
```
POST  http://localhost:8080/bot/${botName}/corpus/upload

```
**Request Body**
```
表单域
key:file
value:选择一个文本文件，每行一个普通文本
```

查询语料
-----
**Request Line**
```
POST http://localhost:8080/bot/${botName}/corpus/condition?page=${pageNumber}$size=${pageSize}
```
**Request Body**
```
{
  "keyword":"用户在搜索框输入的文本"
  "intent":"吃饭"
  "hasIntent":"NO"                   //YES or NO  
}
```
>**查询语料 方法说明**
>>以上变量只有botName路径变量必须包括，其他可根据业务选择传递<br>
>>请求体每个值都可以选择性传递，根据用户是否选择意图或者是否在使用了搜索框<br>
>>如果什么都没选，则要传递一个{}请求体，如果没有传分页参数，默认回传50条<br>

>**查询操作如果输入关键词，则需要则会使用ES提供的同步请求API搜索ElasticSearch,到其中进行全文检索，然后将符合条件的语料的id返回，再去Mysql中根据主键查找完整的语料并返回<br>
修改操作会直接修改数据库，在修改数据库后，获取语料对象，调用ElasticSearch的异步API对文档重新索引，保证ElasticSearch保证了数据一致，并且不会过多影响应用程序的性能，该异步接口提供了异步请求失败时的回调，现在仅仅在该方法中输入了一下失败的提示信息。等以后在项目中引入日志系统的时候用日志把重建索引的语料ID记录到日志文件中？之后再想想该怎么做，日志系统的正规用法和配置暂时没用过，之后再看。**

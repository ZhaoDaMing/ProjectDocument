语料库创建文档
====

**PUT**  **http://localhost:9200/labrador**
```
{
  "settings" : {
        "index" : {
            "number_of_shards" : 5,
            "number_of_replicas" : 1
        }
  },
  "mappings": {
    "corpus" : {
      "properties" : {
        "id" : {
          "type" :    "long"
        },
        "bot" : {
          "type" :   "keyword"
        },
        "intents" : {
          "type" :   "keyword"
        },
        "content" : {
          "type" :   "text",
          "analyzer": "ik_max_word"
        }
      }
    }
  }
}

```

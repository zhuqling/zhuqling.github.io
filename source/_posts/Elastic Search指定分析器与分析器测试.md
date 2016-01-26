---
title: Elastic Search指定分析器与分析器测试
date: 2016-01-26 12:50:44
tags:
- Elastic Search
- 全文检索
---
# 中文全文检索,使用mmseg

## 创建索引
PUT http://localhost:9200/index 

## 设置mapping
```
POST /index/fulltext/_mapping
{
    "fulltext": {
        "_all": {
            "indexAnalyzer": "mmseg",
            "searchAnalyzer": "mmseg",
            "term_vector": "no",
            "store": "false"
        },
        "properties": {
            "content": {
                "type": "string",
                "store": "no",
                "term_vector": "with_positions_offsets",
                "indexAnalyzer": "mmseg",
                "searchAnalyzer": "mmseg",
                "include_in_all": "true",
                "boost": 8
            }
        }
    }
}
```

## 检查mapping
GET /index/fulltext/_mapping

## 批量索引
```
POST /index/fulltext/_bulk
{"index":{"_id":1}}
{content:"美国留给伊拉克的是个烂摊子吗"}
{"index":{"_id":2}}
{content:"公安部：各地校车将享最高路权"}
{"index":{"_id":3}}
{content:"中韩渔警冲突调查：韩警平均每天扣1艘中国渔船"}
{"index":{"_id":4}}
{content:"中国驻洛杉矶领事馆遭亚裔男子枪击 嫌犯已自首"}
```

## 搜索
```
POST /index/fulltext/_search?pretty=true
{
    "query" : { "term" : { "content" : "中国" }},
    "highlight" : {
        "pre_tags" : ["<tag1>", "<tag2>"],
        "post_tags" : ["</tag1>", "</tag2>"],
        "fields" : {
            "content" : {}
        }
    }
}
```

## 测试中文分词

DELETE /test/

```
// 指定分析器和检索器
POST /test/
{
    "index": {
        "number_of_shards": 1,
        "number_of_replicas": 0,
        "analysis": {
            "analyzer": {
                "mmseg_search": {
                    "type": "mmseg",
                    "seg_mode": "search",
                    "stop": true
                },
                "mmseg_index": {
                    "type": "mmseg",
                    "seg_mode": "index",
                    "stop": true
                }
            }
        }
    }
}
```

// 测试分析器
`POST /test/_analyze?analyzer=mmseg_index`
{"中华人民共和国"}


POST /test/1
`{"title": "中华人民共和国"}`

// elastic search Ready to fly suite
https://github.com/medcl/elasticsearch-rtf

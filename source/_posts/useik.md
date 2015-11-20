layout: elasticsearch
title: ElasticSearch使用elasticsearch-analysis-ik
date: 2015-11-19 16:31:48
tags: [elasticsearch,plugin,ik,分词]
---
感谢Medcl带来的ik分词插件。</br>
为什么要用ik?</br>
因为Elasticsearch本身不支持中文分词，使用默认的解析器会把中文分解成单个字，查询的时候很不方便。
<!--more-->
#### 下载ik插件
```
https://github.com/medcl/elasticsearch-analysis-ik
```
#### Maven打包，生成jar包
```
mvn clean package
```
### 安装插件
```
plugin —install analysis-ik —url file:///#{project_path}/elasticsearch-analysis-ik/target/releases/elasticsearch-analysis-ik-1.4.0.zip
```
其实这种方式就是将jar包拷贝到`elasticsearch/plugins`目录下。
<img src="ik01.png" />
然后，将`elasticsearch-analysis-ik-master\config\ik`目录复制到`elasticsearch\config`目录中。
修改`elasticsearch/config/elasticsearch.yml`文件，在最下方添加：
```
################################## ik ################################
index:
  analysis:
    analyzer:
      ik:
          alias: [ik_analyzer]
          type: org.elasticsearch.index.analysis.IkAnalyzerProvider
          char_filter: html_strip
      ik_max_word:
          type: ik
          use_smart: false
      ik_smart:
          type: ik
          use_smart: true
    tokenizer:
      ik_smart:
          type: ik
          use_smart: true
```
启动elasticsearch服务，至此ik插件就完成安装了。<br/>
现在我们来测试一下,首先我们来创建一个索引<br/>
```
POST /index  #创建索引

POST /index/iktest/_mapping  #创建映射
{
    "iktest":{
        "properties": {
            "test":{
                "type": "string",
                "analyzer": "ik"
            }
        }
    }
}
```
返回结果
```
{
   "acknowledged": true
}
```
使用elasticsearch的分析器测试api
```
GET /index/_analyze?analyzer=ik&pretty=true
{
    "地球如此大"
}
```
返回结果
```
{
   "tokens": [
      {
         "token": "地球",
         "start_offset": 8,
         "end_offset": 10,
         "type": "CN_WORD",
         "position": 1
      },
      {
         "token": "如此",
         "start_offset": 10,
         "end_offset": 12,
         "type": "CN_WORD",
         "position": 2
      },
      {
         "token": "如",
         "start_offset": 10,
         "end_offset": 11,
         "type": "CN_WORD",
         "position": 3
      },
      {
         "token": "此",
         "start_offset": 11,
         "end_offset": 12,
         "type": "CN_CHAR",
         "position": 4
      },
      {
         "token": "大",
         "start_offset": 12,
         "end_offset": 13,
         "type": "CN_WORD",
         "position": 5
      }
   ]
}
```
现在我们插入一条数据
```
POST /index/iktest/1
{
    "test":"我们要保护地球"
}
```
```
{
   "_index": "index",
   "_type": "iktest",
   "_id": "1",
   "_version": 1,
   "created": true
}
```
搜索一下
```
POST /index/iktest/_search
{
    "query": {
        "term": {
           "test": {
              "value": "我们"
           }
        }
    }
}
```
返回结果
```
{
   "took": 2,
   "timed_out": false,
   "_shards": {
      "total": 5,
      "successful": 5,
      "failed": 0
   },
   "hits": {
      "total": 1,
      "max_score": 0.11506981,
      "hits": [
         {
            "_index": "index",
            "_type": "iktest",
            "_id": "1",
            "_score": 0.11506981,
            "_source": {
               "test": "我们要保护地球"
            }
         }
      ]
   }
}
```
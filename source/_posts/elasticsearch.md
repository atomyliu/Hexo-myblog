title: elasticsearch
---
date: 2015-11-18 14:05:41
tags:elasticsearch
---
[Elasticsearch](https://www.elastic.co/) 来源于作者 Shay Banon 的第一个开源项目 Compass 库，而这个 Java 库最初的目的只是为了给 Shay 当时正在学厨师的妻子做一个菜谱的搜索引擎。2010 年，Elasticsearch 正式发布。至今已经成为 GitHub 上最流行的 Java 项目，不过 Shay 承诺给妻子的菜谱搜索依然没有面世……
<!--more-->
## 一、如何开始
### 1.1下载Elasticsearch
```
https://www.elastic.co/downloads/elasticsearch
```
### 1.2运行Elasticsearch
```
bin/elasticsearch
```
### 1.3打开浏览器查看运行结果
```
http://localhost:9200
```
```
{
  "status" : 200,
  "name" : "NODE01",
  "cluster_name" : "cluster01",
  "version" : {
    "number" : "1.7.2",
    "build_hash" : "e43676b1385b8125d647f593f7202acbd816e8ec",
    "build_timestamp" : "2015-09-14T09:49:53Z",
    "build_snapshot" : false,
    "lucene_version" : "4.10.4"
  },
  "tagline" : "You Know, for Search"
}
```
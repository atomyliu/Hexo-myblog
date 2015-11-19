layout: elasticsearch
title: elasticsearch 开始
date: 2015-11-18 14:05:41
tags: elasticsearch
---
[Elasticsearch](https://www.elastic.co/) 来源于作者 [Shay Banon](http://www.linkedin.com/in/shaybanon) 的第一个开源项目 Compass 库，而这个 Java 库最初的目的只是为了给 Shay 当时正在学厨师的妻子做一个菜谱的搜索引擎。2010 年，Elasticsearch 正式发布。至今已经成为 GitHub 上最流行的 Java 项目，不过 Shay 承诺给妻子的菜谱搜索依然没有面世……
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
至此，跟Search说Hello吧！
## 二、接口使用
### 2.1 增删改查
增删改查是数据库的基础操作方法。ES 虽然不是数据库，但是很多场合下，都被人们当做一个文档型 NoSQL 数据库在使用，原因自然是因为在接口和分布式架构层面的相似性。
#### 数据写入
ES 的一大特点，就是全 RESTful 接口处理 JSON 请求。所以，数据写入非常简单：
```
$ curl -XPOST 'http://127.0.0.1:9200/myindex/test' -d '{
    "user":"atomyliu",
    "msg":"The first message into Elasticsearch"
}'
```
命令相应结果为：
```
{"_index":"myindex","_type":"test","_id":"AVEdgOTcAe4-VLSojoKP","_version":1,"created":true}
```
#### 数据获取
可以看到，在数据写入的时候，会返回该数据的 `_id` 。这就是后续用来获取数据的关键：
```
$ curl -XGET 'http://localhost:9201/myindex/test/AVEdgOTcAe4-VLSojoKP'
```
命令返回响应结果为：
```
{"_index":"myindex","_type":"test","_id":"AVEdgOTcAe4-VLSojoKP","_version":1,"found":true,"_source":{
    "user":"atomyliu",
    "msg":"first"
}}
```
这个`_source`里的内容，正是之前写入的数据。
#### 数据删除
要删除数据，修改发送的 HTTP 请求方法为 DELETE 即可：
```
$ curl -XDELETE 'http://localhost:9201/myindex/test/AVEdgOTcAe4-VLSojoKP'
```
#### 数据更新
已经写过的数据，同样还是可以修改的。有两种办法，一种是全量提交，即指明`_id`再发送一次写入请求。
另一种是局部更新，使用 `/_update` 接口：
```
$ curl -XPOST 'http://127.0.0.1:9200/myindex/test/AVEdgOTcAe4-VLSojoKP/_update' -d '{
    "user":"atomyliu123",
    "msg":"The first message into Elasticsearch"
}'
```
layout: elasticsearch
title: ElasticSearch REST API 集锦
date: 2015-11-19 16:08:02
tags: [elasticsearch,api,REST]
---
列出索引名称
```
curl 'http://localhost:9200/_aliases?pretty=1'
```
查看所有索引状态
```
curl 'http://localhost:9200/_stats?pretty=1'
```
<!--more-->
列出集群索引
```
curl  'http://127.0.0.1:9200/_cat/indices?pretty=1'
```
查看索引大小
```
curl 'http://localhost:9200/_cat/indices?bytes=kb'
```
获取所有mapping
```
curl -XGET 'http://localhost:9200/_mapping?pretty=1'
```
集群健康查看
```
curl 'http://localhost:9200/_cat/health?v'
```
查看集群线程池
```
curl 'http://localhost:9200/_cat/thread_pool?v'
```
查看磁盘使用情况
```
curl 'http://localhost:9200/_cat/allocation?v'
```
节点健康查看
```
curl 'http://127.0.0.1:9200/_cat/nodes?v'
```
查看进程信息 打开文件数，是否锁定内存等
```
curl 'http://127.0.0.1:9200/_nodes/process?pretty'
```
集群健康
```
curl -XGET 'http://localhost:9200/_cluster/health?pretty=true'
```
关闭所有节点
```
curl -XPOST 'http://localhost:9200/_shutdown'
```
关闭指定节点
```
curl -XPOST 'http://localhost:9200/_cluster/nodes/nodeId1,nodeId2/_shutdown'
```
延迟关闭
```
curl -XPOST 'http://localhost:9200/_cluster/nodes/_local/_shutdown?delay=10s'
```
---
在Elasticsearch集群中可以监控统计很多信息，集群健康(cluster health)。ES中用三种颜色状态表示:green,yellow,red.</br>
`Green`：所有主分片和副本分片都可用</br>
`Yellow`：所有主分片可用，但不是所有副本分片都可用</br>
`Red`：不是所有的主分片都可用</br>

---
一些安全性的设置
```
action.disable_close_all_indices: true     #禁止关闭索引
action.disable_delete_all_indices: true    #禁止删除索引
action.disable_shutdown: true              #禁止关闭节点
```
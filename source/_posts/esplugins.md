layout: elasticsearch
title: elasticsearch 插件安装
date: 2015-11-19 11:06:39
tags: elasticsearch
---
Elasticsearch可扩展性很强，插件也相当完善，下面介绍几个常用插件的安装和使用。
### elasticsearch-head 插件
首先进入 /elasticsearch/bin 目录，使用命令行：
```
./plugin -install mobz/elasticsearch-head
```
<!--more-->
安装完成后在 /elasticsearch/plugins目录下看到head的文件夹
在浏览器中输入`http://localhost:9200/_plugin/head/`可看到：
<img src="head01.png" />
### bigdesk 插件
安装命令：
```
./plugin -install lukas-vlcek/bigdesk
```
安装完成后在 /elasticsearch/plugins目录下看到bigdesk的文件夹
在浏览器中输入`http://localhost:9200/_plugin/bigdesk/`可看到：
<img src="bigdesk01.png" />
### kopf 插件
```
./plugin -install lmenezes/elasticsearch-kopf
```
安装完成后在 /elasticsearch/plugins目录下看到kopf的文件夹
在浏览器中输入`http://localhost:9200/_plugin/kopf/`可看到：
<img src="kopf01.png" />
---
除了这些属于UI类别的插件外，还有其他许多功能上的插件
比如中文分词插件：elasticsearch-analysis-ik
比如拼音分词插件：elasticsearch-analysis-pinyin
可以同步数据的river插件等。

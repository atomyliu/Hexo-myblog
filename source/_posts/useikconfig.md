layout: elasticsearch
title: ElasticSearch使用elasticsearch-analysis-ik 进阶篇
date: 2015-11-20 10:05:03
tags: [elasticsearch,plugin,ik,分词]
---
### 分词配置
上一篇我们介绍了如何使用ik分词，这一篇我们讲讲ik分词的高级使用方式。<br/>
<!--more-->
ik分词在作为elasticsearch的分词插件中，目前运用的比较广泛，因此其扩展性也是很强的。<br/>
下面我们从配置文件来看：`elasticsearch-1.7.1\config\elasticsearch.yml`
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
这段配置文件中应以了三个分析器，分别是`ik`,`ik_max_word`,`ik_smart`,其实，这就给我们提供了多种颗粒度的分词选择，其中`ik`是使用默认词典进行通用的分词，`ik_max_word`继承于`ik`,两者基本相同，`ik_smart`使用了``use_smart``,提供了比较粗颗粒度的分析效果。
这些参数都可以进行自行配置，详细的可以参考一下官方[elasticsearch-analysis-ik](https://github.com/medcl/elasticsearch-analysis-ik)。 <br/>
当然，如果你只是想简单的运用，不想那么繁琐的设置，上面那一大段配置文件也可以简写成：
```
index.analysis.analyzer.ik.type : "ik"
```
或者可以在创建索引时写在`settings`里。<br/>
### 词典配置
ik给了我们对词典的选择性，首先找到ik的配置文件<br/>
`config/ik/IKAnalyzer.cfg.xml`
```
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE properties SYSTEM "http://java.sun.com/dtd/properties.dtd">
<properties>
    <comment>IK Analyzer 扩展配置</comment>
    <!--用户可以在这里配置自己的扩展字典 -->
    <entry key="ext_dict">custom/mydict.dic;custom/single_word_low_freq.dic</entry>
    <!--用户可以在这里配置自己的扩展停止词字典-->
    <entry key="ext_stopwords">custom/ext_stopword.dic</entry>
    <!--用户可以在这里配置远程扩展字典 -->
    <entry key="remote_ext_dict">location</entry>
    <!--用户可以在这里配置远程扩展停止词字典-->
    <entry key="remote_ext_stopwords">http://xxx.com/xxx.dic</entry>
</properties>
```
所有可以使用的字典都在这里进行配置，我们也可以自己来整理和维护词典。
 #### 热词更新ik分词使用方法
 ```
     <!--用户可以在这里配置远程扩展字典 -->
     <entry key="remote_ext_dict">location</entry>
     <!--用户可以在这里配置远程扩展停止词字典-->
     <entry key="remote_ext_stopwords">location</entry>
 ```
其中`location`是指url，比如`http://localhost:8080/mydic.txt`,该请求满足两个条件即可完成热词更新。<br/>
1. 该 http 请求需要返回两个头部(header)，一个是 `Last-Modified`，一个是 `ETag`，这两者都是字符串类型，只要有一个发生变化，该插件就会去抓取新的分词进而更新词库。<br/>
2. 该 http 请求返回的内容格式是一行一个分词，换行符用 `\n` 即可。<br/>
满足上面两点要求就可以实现热更新分词了，不需要重启 ES 实例。<br/>
可以将需自动更新的热词放在一个 UTF-8 编码的 .txt 文件里，放在 nginx 或其他简易 http server 下，当 .txt 文件修改时，http server 会在客户端请求该文件时自动返回相应的 `Last-Modified` 和 `ETag`。可以另外做一个工具来从业务系统提取相关词汇，并更新这个 .txt 文件。
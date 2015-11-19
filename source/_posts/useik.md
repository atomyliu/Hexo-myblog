layout: elasticsearch
title: ElasticSearch使用elasticsearch-analysis-ik
date: 2015-11-19 16:31:48
tags: [elasticsearch,plugin,ik,分词]
---
### ElasticSearch使用elasticsearch-analysis-ik
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
启动elasticsearch服务，至此ik插件就完成安装了。

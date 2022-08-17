# ElasticSearch概述

![image-20220807212105252](C:\Users\X\AppData\Roaming\Typora\typora-user-images\image-20220807212105252.png)	

![image-20220807212150273](C:\Users\X\AppData\Roaming\Typora\typora-user-images\image-20220807212150273.png)	



# 安装

## 安装ElasticSearch-Head



![image-20220807212644456](C:\Users\X\AppData\Roaming\Typora\typora-user-images\image-20220807212644456.png)	



```shell
安装镜像:
elasticsearch: https://mirrors.huaweicloud.com/elasticsearch/?C=N&O=D
logstash: https://mirrors.huaweicloud.com/logstash/?C=N&O=D
kibana: https://mirrors.huaweicloud.com/kibana/?C=N&O=D

```

![image-20220807223405103](C:\Users\X\AppData\Roaming\Typora\typora-user-images\image-20220807223405103.png)	



![image-20220807224033075](C:\Users\X\AppData\Roaming\Typora\typora-user-images\image-20220807224033075.png)	![image-20220807224408182](C:\Users\X\AppData\Roaming\Typora\typora-user-images\image-20220807224408182.png)

```shell
访问http://127.0.0.1:9200/
```

​	![image-20220807224525228](C:\Users\X\AppData\Roaming\Typora\typora-user-images\image-20220807224525228.png)	

> 可视化界面---> head



![image-20220808003417153](C:\Users\X\AppData\Roaming\Typora\typora-user-images\image-20220808003417153.png)	![image-20220808003424794](C:\Users\X\AppData\Roaming\Typora\typora-user-images\image-20220808003424794.png)	

> 存在跨域问题（只有当两个页面同源，才能交互）
>
> 同源（端口，主机，协议三者都相同）
>
> https://blog.csdn.net/qq_38128179/article/details/84956552

![存在跨域问题](https://liuyou-images.oss-cn-hangzhou.aliyuncs.com/markdown/20201124220501.png)	





**开启跨域（在elasticsearch解压目录config下elasticsearch.yml中添加）**

```shell
# 开启跨域
http.cors.enabled: true
# 所有人访问
http.cors.allow-origin: "*"
```



> 重启elasticsearch

**再次连接**



## ![img](https://liuyou-images.oss-cn-hangzhou.aliyuncs.com/markdown/20201124221100.png)	安装Kibana

> 这里的kibana也需要nodejs环境

![image-20220808092948296](C:\Users\X\AppData\Roaming\Typora\typora-user-images\image-20220808092948296.png)	



​	![image-20220808093236056](C:\Users\X\AppData\Roaming\Typora\typora-user-images\image-20220808093236056.png)

```shell
注意：
	#启动kibana的时候需要提前启动es，不然启动不了！
	访问地址:  	http://localhost:5601
```



> 汉化操作如下:

![image-20220808094431661](C:\Users\X\AppData\Roaming\Typora\typora-user-images\image-20220808094431661.png)		

![image-20220808094502689](C:\Users\X\AppData\Roaming\Typora\typora-user-images\image-20220808094502689.png)	



## ES的核心概念

![image-20220808095200696](C:\Users\X\AppData\Roaming\Typora\typora-user-images\image-20220808095200696.png)	 

> 核心

![image-20220808100534272](C:\Users\X\AppData\Roaming\Typora\typora-user-images\image-20220808100534272.png)	





# IK分词器插件

> **IK分词器：中文分词器**

分词：即把一段中文或者别的划分成一个个的关键字，我们在搜索时候会把自己的信息进行分词，会把数据库中或者索引库中的数据进行分词，然后进行一一个匹配操作，**默认的中文分词是将每个字看成一个词**（<mark>不使用用IK分词器的情况下</mark>），比如“我爱狂神”会被分为”我”，”爱”，”狂”，”神” ，这显然是不符合要求的，所以我们需要安装中文分词器ik来解决这个问题。

**IK提供了两个分词算法**: `ik_smart`和`ik_max_word` ,其中`ik_smart`为**最少切分**, `ik_max_word`为**最细粒度划分**!

### 1、下载

> 版本要与ElasticSearch版本对应

下载地址：https://github.com/medcl/elasticsearch-analysis-ik/releases

### 2、安装

> ik文件夹是自己创建的

加压即可（但是我们需要解压到ElasticSearch的plugins目录ik文件夹下）

![img](https://liuyou-images.oss-cn-hangzhou.aliyuncs.com/markdown/20201125005908.png)

### 3、重启ElasticSearch

> 加载了IK分词器

![img](https://liuyou-images.oss-cn-hangzhou.aliyuncs.com/markdown/20201125010232.png)

### 4、使用 `ElasticSearch安装补录/bin/elasticsearch-plugin` 可以查看插件

```
E:\ElasticSearch\elasticsearch-7.6.1\bin>elasticsearch-plugin list
```

![img](https://liuyou-images.oss-cn-hangzhou.aliyuncs.com/markdown/20201201144846.png)

### 5、使用kibana测试

`ik_smart`：最少切分

![img](https://liuyou-images.oss-cn-hangzhou.aliyuncs.com/markdown/20201125013948.png)

`ik_max_word`：最细粒度划分（穷尽词库的可能）

![img](https://liuyou-images.oss-cn-hangzhou.aliyuncs.com/markdown/20201125014210.png)

> 从上面看，感觉分词都比较正常，但是大多数，分词都满足不了我们的想法，如下例

![img](https://liuyou-images.oss-cn-hangzhou.aliyuncs.com/markdown/20201125015809.png)





# Restful风格说明

**一种软件架构风格**,而不是标准,只是提供了一组设计原则和约束条件。它主要用于客户端和服务器交互类的软件。基于这个风格设计的软件可以**更简洁**，**更有层次**，**更易于实现缓存**等机制。

##### **基本Rest命令说明：**

|      method      |                     url地址                     |          描述          |
| :--------------: | :---------------------------------------------: | :--------------------: |
| PUT（创建,修改） |     localhost:9200/索引名称/类型名称/文档id     | 创建文档（指定文档id） |
|   POST（创建）   |        localhost:9200/索引名称/类型名称         | 创建文档（随机文档id） |
|   POST（修改）   | localhost:9200/索引名称/类型名称/文档id/_update |        修改文档        |
|  DELETE（删除）  |     localhost:9200/索引名称/类型名称/文档id     |        删除文档        |
|   GET（查询）    |     localhost:9200/索引名称/类型名称/文档id     |   查询文档通过文档ID   |
|   POST（查询）   | localhost:9200/索引名称/类型名称/文档id/_search |      查询所有数据      |



#### 1、创建一个索引，添加

```shell
PUT /test1/type1/1
{
  "name" : "流柚",
  "age" : 18
}
```

![img](https://liuyou-images.oss-cn-hangzhou.aliyuncs.com/markdown/20201201144937.png)	



![img](https://liuyou-images.oss-cn-hangzhou.aliyuncs.com/markdown/20201201144942.png)	



#### 2、字段数据类型

- 字符串类型

  - text、

    keyword

    - text：支持分词，全文检索,支持模糊、精确查询,不支持聚合,排序操作;text类型的最大支持的字符长度无限制,适合大字段存储；
    - keyword：不进行分词，直接索引、支持模糊、支持精确匹配，支持聚合、排序操作。keyword类型的最大支持的长度为——32766个UTF-8类型的字符,可以通过设置ignore_above指定自持字符长度，超过给定长度后的数据将不被索引，无法通过term精确匹配检索返回结果。

- 数值型

  - long、Integer、short、byte、double、float、**half float**、**scaled float**

- 日期类型

  - date

- te布尔类型

  - boolean

- 二进制类型

  - binary

- 等等…

#### 3、指定字段的类型（使用PUT）

> 类似于建库（建立索引和字段对应类型），也可看做规则的建立



```
PUT /test2
{
  "mappings": {
    "properties": {
      "name": {
        "type": "text"
      },
      "age":{
        "type": "long"
      },
      "birthday":{
        "type": "date"
      }
    }
  }
}

```



![img](https://liuyou-images.oss-cn-hangzhou.aliyuncs.com/markdown/20201201144947.png)	

#### 4、获取3建立的规则

```
GET test2
```



![img](https://liuyou-images.oss-cn-hangzhou.aliyuncs.com/markdown/20201201144955.png)	



#### 5、获取默认信息

> `_doc` 默认类型（default type），type 在未来的版本中会逐渐弃用，因此产生一个默认类型进行代替

```
PUT /test3/_doc/1
{
  "name": "流柚",
  "age": 18,
  "birth": "1999-10-10"
}
GET test3
```



![img](https://liuyou-images.oss-cn-hangzhou.aliyuncs.com/markdown/20201201145002.png)	



> 如果自己的文档字段没有被指定，那么ElasticSearch就会给我们默认配置字段类型

扩展：通过`get _cat/` 可以获取ElasticSearch的当前的很多信息！

```shell
GET _cat/indices
GET _cat/aliases
GET _cat/allocation
GET _cat/count
GET _cat/fielddata
GET _cat/health
GET _cat/indices
GET _cat/master
GET _cat/nodeattrs
GET _cat/nodes
GET _cat/pending_tasks
GET _cat/plugins
GET _cat/recovery
GET _cat/repositories
GET _cat/segments
GET _cat/shards
GET _cat/snapshots
GET _cat/tasks
GET _cat/templates
GET _cat/thread_pool
```



#### 6、修改

> 两种方案

①旧的（使用put覆盖原来的值）

- 版本+1（_version）
- 但是如果漏掉某个字段没有写，那么更新是没有写的字段 ，会消失

```
PUT /test3/_doc/1
{
  "name" : "流柚是我的大哥",
  "age" : 18,
  "birth" : "1999-10-10"
}
GET /test3/_doc/1
// 修改会有字段丢失
PUT /test3/_doc/1
{
  "name" : "流柚"
}
GET /test3/_doc/1
```

![img](https://liuyou-images.oss-cn-hangzhou.aliyuncs.com/markdown/20201201145010.png)	





![img](https://liuyou-images.oss-cn-hangzhou.aliyuncs.com/markdown/20201201160911.png)	



②新的（使用post的update）

- version不会改变
- 需要注意doc
- 不会丢失字段

```shell
POST /test3/_doc/1/_update
{
  "doc":{
    "name" : "post修改，version不会加一",
    "age" : 2
  }
}
GET /test3/_doc/1
```

![img](https://liuyou-images.oss-cn-hangzhou.aliyuncs.com/markdown/20201201163030.png)	

![image-20220808191154756](C:\Users\X\AppData\Roaming\Typora\typora-user-images\image-20220808191154756.png)	

#### 7、删除

```shell
GET /test1
DELETE /test1
```



![img](https://liuyou-images.oss-cn-hangzhou.aliyuncs.com/markdown/20201203150602.png)	



![img](https://liuyou-images.oss-cn-hangzhou.aliyuncs.com/markdown/20201203150557.png)	

#### 8、查询（简单条件）

```shell
GET /test3/_doc/_search?q=name:流柚
```

![img](https://liuyou-images.oss-cn-hangzhou.aliyuncs.com/markdown/20201202204347.png)	

![image-20220808214416567](C:\Users\X\AppData\Roaming\Typora\typora-user-images\image-20220808214416567.png)	![image-20220808214540868](C:\Users\X\AppData\Roaming\Typora\typora-user-images\image-20220808214540868.png)	![image-20220808215301814](C:\Users\X\AppData\Roaming\Typora\typora-user-images\image-20220808215301814.png)	![image-20220808215450389](C:\Users\X\AppData\Roaming\Typora\typora-user-images\image-20220808215450389.png)	![image-20220808220110366](C:\Users\X\AppData\Roaming\Typora\typora-user-images\image-20220808220110366.png)	![image-20220808220429464](C:\Users\X\AppData\Roaming\Typora\typora-user-images\image-20220808220429464.png)	![image-20220808220519179](C:\Users\X\AppData\Roaming\Typora\typora-user-images\image-20220808220519179.png)	**过滤器**

![image-20220808220720605](C:\Users\X\AppData\Roaming\Typora\typora-user-images\image-20220808220720605.png)	![image-20220808221855453](C:\Users\X\AppData\Roaming\Typora\typora-user-images\image-20220808221855453.png)	![image-20220808222107076](C:\Users\X\AppData\Roaming\Typora\typora-user-images\image-20220808222107076.png)	![image-20220808222654964](C:\Users\X\AppData\Roaming\Typora\typora-user-images\image-20220808222654964.png)	![image-20220808222723264](C:\Users\X\AppData\Roaming\Typora\typora-user-images\image-20220808222723264.png)	![image-20220808222903750](C:\Users\X\AppData\Roaming\Typora\typora-user-images\image-20220808222903750.png)	![image-20220808222918751](C:\Users\X\AppData\Roaming\Typora\typora-user-images\image-20220808222918751.png)	![image-20220808223326195](C:\Users\X\AppData\Roaming\Typora\typora-user-images\image-20220808223326195.png)	![image-20220808223530950](C:\Users\X\AppData\Roaming\Typora\typora-user-images\image-20220808223530950.png)

>  自定义高亮

![image-20220808223659751](C:\Users\X\AppData\Roaming\Typora\typora-user-images\image-20220808223659751.png)









#### 9、复杂查询

> test3索引中的内容

![img](https://liuyou-images.oss-cn-hangzhou.aliyuncs.com/markdown/20201203150547.png)	

##### ①查询匹配

- `match`：匹配（会使用分词器解析（先分析文档，然后进行查询））
- `_source`：过滤字段
- `sort`：排序
- `form`、`size` 分页

```shell
  // 查询匹配
  GET /blog/user/_search
  {
    "query":{
      "match":{
        "name":"流"
      }
    }
    ,
    "_source": ["name","desc"]
    ,
    "sort": [
      {
        "age": {
          "order": "asc"
        }
      }
    ]
    ,
    "from": 0
    ,
    "size": 1
  }
```





![img](https://liuyou-images.oss-cn-hangzhou.aliyuncs.com/markdown/20201203002017.png)	



##### ②多条件查询（bool）

- `must` 相当于 `and`
- `should` 相当于 `or`
- `must_not` 相当于 `not (... and ...)`
- `filter` 过滤

```
/// bool 多条件查询
//// must <==> and
//// should <==> or
//// must_not <==> not (... and ...)
//// filter数据过滤
//// boost
//// minimum_should_match
GET /blog/user/_search
{
  "query":{
    "bool": {
      "must": [
        {
          "match":{
            "age":3
          }
        },
        {
          "match": {
            "name": "流"
          }
        }
      ],
      "filter": {
        "range": {
          "age": {
            "gte": 1,
            "lte": 3
          }
        }
      }
    }
  }
}
```

![img](https://liuyou-images.oss-cn-hangzhou.aliyuncs.com/markdown/20201203150628.png)

##### ③匹配数组

- 貌似不能与其它字段一起使用
- 可以多关键字查（空格隔开）— 匹配字段也是符合的
- `match` 会使用分词器解析（先分析文档，然后进行查询）
- 搜词

```
// 匹配数组 貌似不能与其它字段一起使用
// 可以多关键字查（空格隔开）
// match 会使用分词器解析（先分析文档，然后进行查询）
GET /blog/user/_search
{
  "query":{
    "match":{
      "desc":"年龄 牛 大"
    }
  }
}
```

![img](https://liuyou-images.oss-cn-hangzhou.aliyuncs.com/markdown/20201203150635.png)

##### ④精确查询

- `term` 直接通过 倒排索引 指定**词条**查询
- 适合查询 number、date、keyword ，不适合text

```
// 精确查询（必须全部都有，而且不可分，即按一个完整的词查询）
// term 直接通过 倒排索引 指定的词条 进行精确查找的
GET /blog/user/_search
{
  "query":{
    "term":{
      "desc":"年 "
    }
  }
}
```

![img](https://liuyou-images.oss-cn-hangzhou.aliyuncs.com/markdown/20201203150641.png)

##### ⑤text和keyword

- text：
  - **支持分词**，**全文检索**、支持模糊、精确查询,不支持聚合,排序操作;
  - text类型的最大支持的字符长度无限制,适合大字段存储；
- keyword：
  - **不进行分词**，**直接索引**、支持模糊、支持精确匹配，支持聚合、排序操作。
  - keyword类型的最大支持的长度为——32766个UTF-8类型的字符,可以通过设置ignore_above指定自持字符长度，超过给定长度后的数据将不被索引，**无法通过term精确匹配检索返回结果**。

```
// 测试keyword和text是否支持分词
// 设置索引类型
PUT /test
{
  "mappings": {
    "properties": {
      "text":{
        "type":"text"
      },
      "keyword":{
        "type":"keyword"
      }
    }
  }
}
// 设置字段数据
PUT /test/_doc/1
{
  "text":"测试keyword和text是否支持分词",
  "keyword":"测试keyword和text是否支持分词"
}
// text 支持分词
// keyword 不支持分词
GET /test/_doc/_search
{
  "query":{
   "match":{
      "text":"测试"
   }
  }
}// 查的到
GET /test/_doc/_search
{
  "query":{
   "match":{
      "keyword":"测试"
   }
  }
}// 查不到，必须是 "测试keyword和text是否支持分词" 才能查到
GET _analyze
{
  "analyzer": "keyword",
  "text": ["测试liu"]
}// 不会分词，即 测试liu
GET _analyze
{
  "analyzer": "standard",
  "text": ["测试liu"]
}// 分为 测 试 liu
GET _analyze
{
  "analyzer":"ik_max_word",
  "text": ["测试liu"]
}// 分为 测试 liu
```

##### ⑥高亮查询

```
/// 高亮查询
GET blog/user/_search
{
  "query": {
    "match": {
      "name":"流"
    }
  }
  ,
  "highlight": {
    "fields": {
      "name": {}
    }
  }
}
// 自定义前缀和后缀
GET blog/user/_search
{
  "query": {
    "match": {
      "name":"流"
    }
  }
  ,
  "highlight": {
    "pre_tags": "<p class='key' style='color:red'>",
    "post_tags": "</p>", 
    "fields": {
      "name": {}
    }
  }
} 
```

![img](https://liuyou-images.oss-cn-hangzhou.aliyuncs.com/markdown/20201203150652.png)

# 六、SpringBoot整合



> 一定要注意导入的版本和本地es的版本一致








































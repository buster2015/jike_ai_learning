# halberd [![Build Status](https://img.shields.io/badge/build-passing-green)]()

[![License](http://img.shields.io/:license-apache-blue.svg "2.0")](http://www.apache.org/licenses/LICENSE-2.0.html)
[![JDK 1.8](https://img.shields.io/badge/JDK-1.8-green.svg "JDK 1.8")]()

## halberd项目简介

- halberd是Java语言的数据处理项目。
- 使用Maven对项目进行模块化管理，提高项目的易开发性、扩展性。
- 系统构成：source、sink、transform。
- 业务相关：活动信息处理、品牌品类处理、分析结果处理、GMV处理、涨价售罄处理、好价数据处理、价格曲线处理、商品店铺映射处理、社区种子处理、微信文章处理、百科数据处理。

## 代码结构

```text
config: 项目配置
  | DBConfig: DB配置文件加载
  | XXXConfig: XXX配置文件

modules：按业务划分模块
  | systemlog：日志管理模块
  | source：读取存储组件模块
  | sink：写入存储组件模块
  | transform：逻辑处理模块

common：公共代码
  | exception：基础异常类类库、工具类及相关扩展方法（与业务无关）
  | base：基础Web、基础Mapper、基础Model
  | support：常用加密工具
  | config：配置
    | DBPasswordCallback：数据库密码加密解密配置
    | RedisCacheConfig：redis缓存配置
  | util ：
 OperateController: 项目启动类

```

## 主要功能

1.  存储组件数据读写：对rdb及nosql存储组件进行批读写。
2.	日志管理：通过注解方式进行常规日志管理，抽象日志模块，方便日志统一管理；
3.	数据标准化引擎：主要针对GMV数据进行标准化处理；对某些状态值进行重新赋值转换；
4. 规则引擎：针对数据清洗规则进行配置；

## 技术选型

1、后端

* scala library框架：scala-library 2.11.12
* spark框架：spark 2.2.0
* spark core框架：spark-core_2.11 2.2.0
* spark streaming框架：spark-streaming_2.11 2.2.0
* spark hive框架：spark-hive_2.11 2.2.0
* spark sql框架：spark-sql_2.11 2.2.0
* hadoop client框架：hadoop-client 2.7.3
* hadoop common框架：hadoop-common 2.7.3
* hbase client框架：hbase-client 1.3.1
* hbase server框架：hbase-server 1.3.1
* kafka框架：kafka_2.11 0.10.1.0
* clickhouse jdbc框架：clickhouse-jdbc 0.1.40
* spark streaming kafka框架：spark-streaming-kafka-0-10_2.11 2.2.0
* 持久层框架：MyBatis 3.2
* 数据库连接池：Alibaba Druid 1.0
* 缓存框架：Ehcache 2.6、Redis
* 日志管理：SLF4J 1.7、Log4j
* 工具类：Apache Commons、Jackson 2.2、Xstream 1.4、Dozer 5.3、POI 3.9

3、项目

* 服务器中间件：支持SprintBoot自带容器启动。
* 数据库支持：目前支持供MySql5.7、Hbase，你可以很方便的更改为其它数据库，如：Es、Mongo等。
* 开发环境：Java、IDEA xxx、Maven 3.1、Git

4、 项目组件支持情况

组件 | spark | flink | Mapreduce
---|---|---|---
kafka | 读写支持|调试中
hive |读写支持
mysql|读取支持
api|读写支持
clickhouse|
es|读取支持
hdfs|写入支持
本地多种格式文件|写入支持
控制台|写入支持

## 环境变量与配置文件
### 开发环境

* 修改application.yml文件

```
#区分开发环境配置和生产环境
spring:
  profiles:
    active: dev
```

### 测试环境

* 修改application.yml文件

```
#区分开发环境配置和生产环境
spring:
  profiles:
    active: test
```

### 线上环境

* 修改application.yml文件

```
#区分开发环境配置和生产环境
spring:
  profiles:
    active: prod
```

### 临时环境

* 修改application.yml文件

```
#区分开发环境配置和生产环境
spring:
  profiles:
    active: tmp
```

## 启动说明
    * 项目依赖mysql服务。
    * 启动方法：
    	 OperateController.java
    * 测试环境打包命令：
    	 clean package -P test
    * 生产环境打包命令：
    	 clean package -P product
    * 临时环境打包命令：
    	 clean package -P test

## 启动命令
TODO

```
249环境启动halberd-1.0.0版本，消费kafka数据到控制台
sh /data/code/waterdrop/halberd-1.0.0/bin/start-halberd-spark.sh --master yarn --deploy-mode client --config /data/code/waterdrop/halberd-1.0.0/config/halberd_spark_demo.conf
sh /data/code/waterdrop/halberd-1.0.0/bin/start-halberd-flink.sh --master yarn --deploy-mode client --config /data/code/waterdrop/halberd-1.0.0/config/halberd_flink_demo.conf

集群模式
sh /data/code/waterdrop/halberd-1.0.0/bin/start-halberd-spark.sh --master yarn --deploy-mode client --config /data/code/waterdrop/halberd-1.0.0/config/halberd_spark_demo_kafka_console.conf
sh /data/code/waterdrop/halberd-1.0.0/bin/start-halberd-spark.sh --master yarn --deploy-mode client --config /data/code/waterdrop/halberd-1.0.0/config/halberd_spark_demo_jdbc_console.conf
sh /data/code/waterdrop/halberd-1.0.0/bin/start-halberd-spark.sh --master yarn --deploy-mode client --config /data/code/waterdrop/halberd-1.0.0/config/halberd_spark_demo_es_console.conf
sh /data/code/waterdrop/halberd-1.0.0/bin/start-halberd-spark.sh --master yarn --deploy-mode client --config /data/code/waterdrop/halberd-1.0.0/config/halberd_spark_demo_jdbc_hdfs.conf

本地模式
sh /data/code/waterdrop/halberd-1.0.0/bin/start-halberd-spark.sh --master local[4] --deploy-mode client --config /data/code/waterdrop/halberd-1.0.0/config/halberd_spark_demo_kafka_console.conf
file目前只可以用local模式进行启动，写入本地目录下
sh /data/code/waterdrop/halberd-1.0.0/bin/start-halberd-spark.sh --master local[2] --deploy-mode client --config /data/code/waterdrop/halberd-1.0.0/config/halberd_spark_demo_jdbc_file.conf
```

## 项目本地调试环境配置

```
配置java环境【idea不需要配置】
配置hadoop环境【windows的话需要把winutils.exe和hadoop.dll放入bin目录；idea不需要配置】
配置spark环境【idea不需要配置】
idea本地启动需要在运行配置项中添加本地spark参数【VM Options: -Dspark.master=local】和本地运行参数【--master local --deploy-mode client --config G:\smzdm\develop\workspace_idea\halberd\config\halberd_spark_demo_jdbc_http.conf】
以上全部配置完，windows中的idea可以进行调试开发
```

## 常见问题

1.
2.
3.

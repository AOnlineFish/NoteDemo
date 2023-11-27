# KBQA FAQ

## Q1: text2vec-large-chinese not found

确认下载text2vec-large-chinese模型姿势以及路径正确

```bash
centos:yum install git-lfs
ubuntu:apt-get install git-lfs -y
macos:brew install git-lfs
```



```bash
cd models
git lfs clone https://huggingface.co/GanymedeNil/text2vec-large-chinese
```



## 怎么修改向量数据库类型

怎样在.env文件设置VECTOR_STORE_TYPE

DB-GPT当前支持向量数据库有Chroma(Default), Milvus(>2.1), Weaviate. 可以在在.env文件设置VECTOR_STORE_TYP，如果你想支持更多向量数据库， 参考[how to integrate](https://db-gpt.readthedocs.io/en/latest/modules/vector.html)

```bash
#*******************************************************************#
#**                  VECTOR STORE SETTINGS                       **#
#*******************************************************************#
VECTOR_STORE_TYPE=Chroma
#MILVUS_URL=127.0.0.1
#MILVUS_PORT=19530
#MILVUS_USERNAME
#MILVUS_PASSWORD
#MILVUS_SECURE=

#WEAVIATE_URL=https://kt-region-m8hcy0wc.weaviate.network
```



## 当使用vicuna系列模型时出现乱码。

![260623002-088d1967-88e3-4f72-9ad7-6c4307baa2f8](E:%5CDesktop%5CAPP%5CLlama2%E5%BC%80%E5%8F%91%E6%96%87%E6%A1%A3%5Cubuntu%E7%8E%AF%E5%A2%83%E9%83%A8%E7%BD%B2%5CKBAQ.assets%5C260623002-088d1967-88e3-4f72-9ad7-6c4307baa2f8.png)

通过在.env文件将KNOWLEDGE_SEARCH_TOP_SIZE设置更小点或者在文档界面点击参数设置，将topk设置更小点

## space add error (pymysql.err.OperationalError) (1054, “Unknown column ‘knowledge_space.context’ in ‘field list’”)

1.终止 dbgpt_server(ctrl c)

2.新增列 `context` for table knowledge_space

```bash
mysql -h127.0.0.1 -uroot -paa12345678
```



3.执行ddl

```mysql
mysql> use knowledge_management;
mysql> ALTER TABLE knowledge_space ADD COLUMN context TEXT COMMENT "arguments context";
```



4.重启dbgpt server

## Q5:当使用 Mysql数据库时, 使用DB-GPT怎么初始化 KBQA service database schema

构建Mysql KBQA system database schema

```bash
$ mysql -h127.0.0.1 -uroot -paa12345678 < ./assets/schema/knowledge_management.sql
```
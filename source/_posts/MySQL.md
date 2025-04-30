---
title: MySQL
date: 2025-04-27 21:59:37
categories:
  - MySQL
tags:
  - 开发组件
comments: true
---

# 架构 #

MySQL分为 **Server 层**和**存储引擎层**

**Server 层**包含连接器、查询缓存、解析器、预处理器、优化器、执行器

**存储引擎层**包含 InnoDB、MyISAM 等存储引擎以及数据



## SQL执行流程

从连接器开始，基于TCP协议确定客户端是否连接成功。连接成功后，当服务端收到客户端的 SQL 语句后，先以**Key-Value**形式到查询缓存里查看是否有这条SQL语句，如果有，则直接返回相应的 Value，即查询结果。若没有，则



































# 事务

## ACID四大特性

A：atomicity，原子性。通过 Undo log 实现

C：consistency，一致性。通过其它三个特性和应用层逻辑实现

I：isolation，隔离性。通过 MVCC 和锁实现

D：durability，持久性。通过 Redo log 实现

虽称为四大特性，但并不平级，**AID** 是为了达到 **C** 这一目标的手段



## MVCC

多版本并发控制







































# 索引

















































# 锁









































# 日志









































# 内存




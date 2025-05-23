---
title: Redis
date: 2025-04-27 21:59:44
categories: 
  - Redis
tags:
  - 开发组件
comments: true
---

# 数据类型









































# 持久化机制

## AOF日志

### 作用

宕机或服务器重启后，恢复 Redis 缓存中的数据

**如何实现？**

当 Redis 写操作执行成功后，将其追加到 AOF 日志中，该文件位于磁盘中，可以实现持久化存储

**读操作是否需要存储？**

不需要，读操作不会改变数据状态，没有记录的必要

**为什么要在写操作执行成功后才追加？**

1. 保证 AOF 中记录的语法都是正确的，避免语法检查这一不必要的开销
2. 避免当前写操作的执行被阻塞，保证 Redis 性能



### 潜在风险

1. 将 Redis 命令 追加到 AOF 这一操作，会阻塞主进程执行下一个 Redis 命令
2. 由于写操作执行成功后才做追加，如果此时服务器挂了，还是会造成数据丢失



### 写回策略

Redis 提供了三种写回硬盘的策略

1. Always，每条写操作执行成功后，立即将 AOF 日志写回硬盘
2. Everysec，写操作执行成功后，先将其加入 AOF 缓冲区，每隔一秒将缓冲区的内容写回硬盘
3. No，写操作执行成功后，将其加入 AOF 缓冲区，由操作系统决定何时写回硬盘

**这三种策略是如何实现的？**

本质上是 fsync() 函数的调用时机不同

Always 每次写入 AOF 数据后，立即调用 fsync() 函数

Everysec 创建一个异步任务来调用 fsync() 函数

No 永不调用 fsync() 函数

**如何选取？**

上述风险中提到的两个点，本质上是互斥的，无法同时兼顾

具体的写回策略须根据业务需要进行设置，给出参考

1. 追求高性能，采用 No 策略
2. 追求高可靠，采用 Always 策略，尽可能减少数据丢失
3. 追求性能且允许少量数据丢失，采取折中的 Everysec 策略



### 重写机制

随着执行的写操作命令越来越多，AOF 文件的大小越来越大

重启 Redis 后，需要读 AOF 文件恢复缓存数据，若 AOF 文件太大，恢复过程就很慢

重写机制就是为了避免这一情况的出现

**如何实现？**

Redis 以 Key-Value 形式存储数据，对于每个 Key，只保留其最新的 Value，这样即使这个键值对被修改多次，也只会保留最新状态，并在 AOF 文件中用一条命令记录它，降低 AOF 文件大小

**具体过程？**

先创建一个新的 AOF 文件，在其中进行重写，重写成功后再覆盖旧的 AOF 文件

**为什么不直接覆写在旧 AOF 文件？**

先写到新的 AOF 中，失败的话，直接删除这个文件即可，避免因重写失败污染AOF 文件



















## RDB快照

















































# 高可用

## 主从复制

## 哨兵机制

## 集群













































# 缓存

## 缓存三兄弟

### 雪崩

### 击穿

### 穿透



## 数据库与缓存的一致性问题


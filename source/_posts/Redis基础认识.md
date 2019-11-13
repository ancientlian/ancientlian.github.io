---
title: Redis基础认识
date: 2018-12-14 21:36:53
tags: redis
categories: redis
---
在学习的过程当中，我们或多或少的都会接触到redis，比如说在我们的web项目中，如果想添加历史浏览记录，那肯定要用到数据库，但如果直接上手mysql就显得的过于大材小用了，此时redis也就应运而生。不过首先需要介绍一下什么是nosql。

## NoSQL介绍

**NoSQL：一类新出现的数据库（not only sql），特点有：**

+ **不支持sql语法**
+ 存储结构和传统关系型数据库（比如mysql，oracle，sql server）中的关系表不同，nosql中存储的数据都是**K-V**形式。而Redis 就是一个高性能的key-value数据库。
+ NoSQL的世界里没有一种通用的语言，每种NoSQL数据库都有自己的api和语法，以及擅长的业务场景
+ NoSQL产品种类比如有：
+ + Mongodb
  + Redis
  + Hbase hadoop
  + Cassandra hadoop
<!--more-->
**NoSQL和SQL数据库比较：**

+ 适用场景不同：sql数据库适合用于关系特别复杂的数据查询场景，nosql相反
+ “事务”特性的支持：sql对事务的支持非常完善，而nosql基本不支持事务。（注：**“事务”**：一组sql操作，要么都成功，要么都失败）
+ 两者在不断地取长补短，呈现融合趋势

## Redis简介

+ Redis是一个开源的使用ANSI [C语言](https://baike.baidu.com/item/C%E8%AF%AD%E8%A8%80)编写、支持网络、可基于内存亦可持久化的日志型、Key-Value[数据库](https://baike.baidu.com/item/%E6%95%B0%E6%8D%AE%E5%BA%93/103728)，并提供多种语言的API。从2010年3月15日起，Redis的开发工作由VMware主持。从2013年5月开始，Redis的开发由Pivotal赞助。
+ Redis是一个key-value[存储系统](https://baike.baidu.com/item/%E5%AD%98%E5%82%A8%E7%B3%BB%E7%BB%9F)。和Memcached类似，它支持存储的value类型相对更多，包括**string(字符串)、list([链表](https://baike.baidu.com/item/%E9%93%BE%E8%A1%A8))、set(集合)、zset(sorted set --有序集合)和hash（哈希类型）**。（来源：百度词条）

## Redis特性

+ 与其他key-value缓存产品一样：支持数据的持久化，可以将内存中的数据保存在磁盘中，重启的时候可以再次加载进行使用。
+ 同时提供list,set,zset,hash等数据结构的存储。
+ 支持数据的**备份**，即master-slave模式的数据备份

## Redis 优势

+ 性能极高 - Redis能读的速度是110000次/s，写的速度为81000次/s。
+ 丰富的数据类型 - 支持**二进制案例**的 String，Lists，Hashes，Sets 及 Orderd Sets 数据类型操作。
+ 原子 - Redis的所有操作都是原子性的，同时Redis还支持对几个操作全并后的原子性执行。
+ 丰富的特性 – Redis还支持 publish/subscribe, 通知, key 过期等等特性。

## Redis应用场景

+ 用来做**缓存**（ehcache/memcached）--redis的所有数据是放在内存中的（内存数据库）
+ 可以在某些特定应用场景下替代传统数据库 -- 比如社交应用
+ 在一些大型系统，特定功能：session共享，购物车
+ 也有比如排行榜应用，取TOP N操作。
+ 按照用户投票和时间排序等等。
+ 只要你有丰富的想象力，redis有无限的惊喜

## 推荐阅读

+ [redis官方网站](https://redis.io/)
+ [redis中文官网](http://www.redis.cn/)
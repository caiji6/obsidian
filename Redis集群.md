---
title: Redis集群
categories:
  - 数据库
tags:
  - Redis
halo:
  site: http://120.76.228.140:8090
  name: 990fa7bd-8817-42bd-8663-1b401c16a763
  publish: false
---
## Redis持久化
### RDB
RDB全称Redis Database Backup file（Redis数据备份文件），也被叫做Redis数据快照。简单来说就是把内存中的所有数据都记录到磁盘中。当Redis实例故障重启后，从磁盘读取快照文件，恢复数据。
快照文件称为RDB文件，默认是保存在当前运行目录。
测试插件
台式机测试





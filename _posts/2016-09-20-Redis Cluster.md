---
published: true
title: Redis Cluster First
layout: post
author: Eli ZHANG 
category: articles
tags:
- version
- new features
- google analytics
- google search
- back to top
- read more

---

## Redis 集群搭建准备

> 下面先搭建一个最简单的 3 个节点的集群，先不为主节点添加从节点，在稍后的文章中会添加从节点。至于Redis如何安装将在后面的文章补充上.
> 
> 现在的假设我们有 3 个 A,B,C 节点，分别部署一个 Redis 节点。由于测试环境受限影响，现在把 3 个节点都部署到一台服务器上。新建 Redis-Cluster 目录，然后新建 3 个文件夹，分别是：

```
	mkdir Redis-Cluster
	mkdir 30001 30002 30003
```
>
>	然后在 30001 30002 30003 中分别生成 redis.conf 文件，文件内容分别如下。
>

```
	port 30001
	cluster-enabled yes
	cluster-config-file nodes.conf
	cluster-node-timeout 5000
	appendonly yes

	port 30002
	cluster-enabled yes
	cluster-config-file nodes.conf
	cluster-node-timeout 5000
	appendonly yes
	
	port 30003
	cluster-enabled yes
	cluster-config-file nodes.conf
	cluster-node-timeout 5000
	appendonly yes

```

> 执行如下命令来分别启动每个 Redis 实例：

```
	../src/redis-server redis.conf
```

## 创建集群

>	现在我们已经有了三个正在运行中的 Redis 实例， 接下来我们需要使用这些实例来创建集群， 并为每个节点编写配置文件。通过使用 Redis 集群命令行工具 redis-trib ， 编写节点配置文件的工作可以非常容易地完成： redis-trib 位于 Redis 源码的 >src 文件夹中， 它是一个 Ruby 程序， 这个程序通过向实例发送特殊命令来完成创建新集群， 检查集群， 或者对集群进行重新分片>（reshared）等工作。

```
./redis-trib.rb create 192.168.1.179:30001 192.168.1.179:30002 192.168.1.179:30003
```
>	执行结果如下,这表示集群中的 16384 个槽都有至少一个主节点在处理， 集群运作正常。

```
[root@192 src]# ./redis-trib.rb create 192.168.1.179:30001 192.168.1.179:30002 192.168.1.179:30003

>>> Creating cluster
>>> Performing hash slots allocation on 3 nodes...
Using 3 masters:
192.168.1.179:30001
192.168.1.179:30002
192.168.1.179:30003
M: a87f3bd894a5d58fda2c7ba0fb0926e05c8ebf96 192.168.1.179:30001
   slots:0-5460 (5461 slots) master
M: 915039a399601973b5d5915f71d8c37f6caeeb45 192.168.1.179:30002
   slots:5461-10922 (5462 slots) master
M: c4628faf0aa4cf8d083733abd9e1c10225d7a731 192.168.1.179:30003
   slots:10923-16383 (5461 slots) master
Can I set the above configuration? (type 'yes' to accept): yes
>>> Nodes configuration updated
>>> Assign a different config epoch to each node
>>> Sending CLUSTER MEET messages to join the cluster
Waiting for the cluster to join..
>>> Performing Cluster Check (using node 192.168.1.179:30001)
M: a87f3bd894a5d58fda2c7ba0fb0926e05c8ebf96 192.168.1.179:30001
   slots:0-5460 (5461 slots) master
M: 915039a399601973b5d5915f71d8c37f6caeeb45 192.168.1.179:30002
   slots:5461-10922 (5462 slots) master
M: c4628faf0aa4cf8d083733abd9e1c10225d7a731 192.168.1.179:30003
   slots:10923-16383 (5461 slots) master
[OK] All nodes agree about slots configuration.
>>> Check for open slots...
>>> Check slots coverage...
[OK] All 16384 slots covered.

```
>	到此为止，Redis 最简配置的集群就搭建完成了。







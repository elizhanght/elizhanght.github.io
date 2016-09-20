---
published: true
title: Redis Cluster 添加从节点
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

### Redis Cluster slave配置

>从上篇的文章继续，之前配置了 3 个节点，分别是 30001，30002，30003。然后执行如下命令为 30001 添加从节点。

>新建 30004 文件夹，然后新建 redis.conf文件，文件内容如下:

```
port 30004
cluster-enabled yes
cluster-config-file nodes.conf
cluster-node-timeout 5000
appendonly yes
```

> 然后执行如下命令启动 redis 实例

```
../src/redis-server redis.conf
``` 
> 执行如下命令添加从节点

```
./redis-trib.rb add-node --slave --master-id a87f3bd894a5d58fda2c7ba0fb0926e05c8ebf96 192.168.1.179:30004 192.168.1.179:30001
>>> Adding node 192.168.1.179:30004 to cluster 192.168.1.179:30001
>>> Performing Cluster Check (using node 192.168.1.179:30001)
M: a87f3bd894a5d58fda2c7ba0fb0926e05c8ebf96 192.168.1.179:30001
   slots:0-5460 (5461 slots) master
   0 additional replica(s)
M: c4628faf0aa4cf8d083733abd9e1c10225d7a731 192.168.1.179:30003
   slots:10923-16383 (5461 slots) master
   0 additional replica(s)
M: 915039a399601973b5d5915f71d8c37f6caeeb45 192.168.1.179:30002
   slots:5461-10922 (5462 slots) master
   0 additional replica(s)
[OK] All nodes agree about slots configuration.
>>> Check for open slots...
>>> Check slots coverage...
[OK] All 16384 slots covered.
>>> Send CLUSTER MEET to node 192.168.1.179:30004 to make it join the cluster.
Waiting for the cluster to join.
>>> Configure node as replica of 192.168.1.179:30001.
[OK] New node added correctly.
```

>下面对添加的 slave 节点进行测试。上面将 30004 节点添加成为 30001 的 slave 节点，接下来登录到任何一个节点（非 30004节点），然后执行下面的命令：

```
[root@192 src]# ./redis-cli -c -p 30001
127.0.0.1:30001> set a 1
-> Redirected to slot [15495] located at 192.168.1.179:30003
OK
192.168.1.179:30003> set b 2
-> Redirected to slot [3300] located at 192.168.1.179:30001
OK

```
>然后登录到 30004 节点，查看是否将数据同步到 slave 节点,如果有则说明 slave 配置成功。

```
[root@192 src]# ./redis-cli -c -p 30004
127.0.0.1:30004> get b
-> Redirected to slot [3300] located at 192.168.1.179:30001
"2"
192.168.1.179:30001> 
```


---
published: true
title: Redis Master Slave
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

## Redis 主备搭建

> 在之前的文章中已经描述怎么样来搭建 redis 的高可用服务，但是在实际使用中由于 spring 和 jedis 无法很好的支持，第三方的支持又不敢直接使用，所以在生产环境中采用了主备的架构。

> 准备 3 台服务器，分别为 192.168.100.1、192.168.100.2、192.168.100.3。Redis 部署结构如下：<br />
> 192.168.100.1 -- Master Sentinel <br />
> 192.168.100.2 -- Slave  Sentinel <br />
> 192.168.100.3 -- Slave  Sentinel <br />

> 下面分别介绍每个节点 Redis.conf 的内容,下面值列出了 Master修改的部分:

```
daemonize yes	// 后台进程运行
masterauth "123456"	// 连接密码
requirepass "123456"		// 防止主备切换后设置的密码

```

> 接下来是 slave 的 Redis.conf 设置,下面只是个性的配置:

```
daemonize yes	// 后台进程运行
masterauth "123456"
requirepass "123456"		// 防止主备切换后设置的密码
slaveof 192.168.100.33 6379		// 设置主的IP配置

```

> 通过上面的配置已经搭建起主从的配置了，首先要启动 Master ,然后启动 Slave 。然后通过 ./redis-cli 命令来查看是否成功。
> 
> 下面来配置 Redis 的监控程序，需要修改的是 sentinel.conf 配置文件。三个节点的配置都一样，如下,具体参数请查看 sentinel.conf 文档。

```
port 26380

sentinel monitor mymaster 192.168.100.1 6379 1
sentinel auth-pass mymaster 123456  // 123456 是密码
sentinel down-after-milliseconds mymaster 30000
sentinel failover-timeout mymaster 80000
sentinel parallel-syncs mymaster 1

```
> 接下来就是分别启动 sentinel ，命令如下:

```
src/redis-sentinel sentinel.conf
```



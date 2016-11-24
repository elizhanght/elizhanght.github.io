---
published: true
title: Redis-Sentinel 配置
layout: post
author: Eli ZHANG 
category: articles

---
##Redis-Sentinel配置

>Redis配置过程如下，安装过程省略。首先配置 sentinel.conf 文件，内容如下。具体环境是一主一从。主从的配置参见上一篇文章.
>
> 主 - 192.168.200.4</br>
> 从 - 192.168.200.3
> 
> 分别的两台主机上安装 Redis，具体配置文件如下所示,主从的sentinel.conf文件内容相同。

```
sentinel monitor mymaster 192.168.200.4 6379 2
sentinel auth-pass mymaster 123456  // 123456 是密码
sentinel down-after-milliseconds mymaster 60000 
sentinel failover-timeout mymaster 180000 
sentinel parallel-syncs mymaster 1
```

> 为 sentinel 配置日志及后台启动

```
daemonize yes
logfile "/var/log/sentinel/sentinel_log.log"
```
> 启动 sentinel

```
src/redis-sentinel sentinel.conf
```



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

> 准备 3 台服务器，分别为 192.168.100.1、192.168.100.2、192.168.100.3。Redis 部署结构如下：
> 192.168.100.1 -- Master Sentinel <br />
> 192.168.100.2 -- Slave  Sentinel <br />
> 192.168.100.3 -- Slave  Sentinel <br />

> 下面分别介绍每个节点 Sentinel.conf 的内容。

```


```
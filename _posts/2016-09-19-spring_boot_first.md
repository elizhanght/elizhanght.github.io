---
published: true
title: Spring Boot First
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
## 1. 基本信息

><font size='2px'>Spring Boot 是 spring 社区新增的项目，目的是加快项目的开发，下面将逐步开始介绍如何搭建一个 Spring Boot 工程。</font>

## 2. 新建项目

>首先在 Eclipse 中新建 MAVEN 工程,在 pom.xml 添加如下内容。

```
<parent>
	<groupId>org.springframework.boot</groupId>
	<artifactId>spring-boot-starter-parent</artifactId>
	<version>1.4.0.RELEASE</version>
</parent>
<dependency>
	<groupId>org.springframework.boot</groupId>
	<artifactId>spring-boot-starter-web</artifactId>
</dependency>
```
>新建工程 MyBatisController.java，代码如下：

```
package controller;

import java.util.List;
import java.util.Map;

import org.mybatis.spring.annotation.MapperScan;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.autoconfigure.EnableAutoConfiguration;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.ResponseBody;
import org.springframework.web.bind.annotation.RestController;

import com.springboot.mybatis.mapper.UserMapper;

@RestController
@EnableAutoConfiguration
public class MyBatisController {
	
	@RequestMapping("/")
	@ResponseBody
	String home(){
		return "Hello World";
	}
}
```
>新建Application.java 类，代码如下，启动工程，在浏览器地址栏输入 http://localhost:8080

```
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

import controller.MyBatisController;

@SpringBootApplication
public class Application {

	public static void main(String[] args) {
		SpringApplication.run(MyBatisController.class, args);
	}
}
```

>使用 Maven 启动工程，在指定地方输入 spring-boot:run 即可。



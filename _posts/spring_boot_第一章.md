#Spring Boot 第一章


## 1. 基本信息

>Spring Boot 是 spring 社区新增的项目，目的是加快项目的开发，下面将逐步开始介绍如何搭建一个 Spring Boot 工程。</br>

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
@MapperScan("mapper")
public class MyBatisController {

	@Autowired
	private UserMapper userMapper;
	
//	@Value("${name}")
//	private String name;
//	private StringRedisTemplate template;
//	@Autowired
//    public SampleController(StringRedisTemplate template) {
//        this.template = template;
//    } 
	
	@RequestMapping("/")
	@ResponseBody
	String home(){
		
//		System.out.println(name);
//		System.out.println(template.getClientList());
		List<Map<String,Object>> userList = userMapper.findUserList();
		System.out.println(userList);
		
		return "Hello World";
	}
}
```


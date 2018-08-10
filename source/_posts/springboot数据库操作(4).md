---
title: springboot数据库操作(4)
categories: SpringBoot
tags: SpringBoot
keywords: SpringBoot
abbrlink: e57b58e0
date: 2018-07-28 16:15:00
---
### 修改pom.xml
```
<dependency>
			<groupId>mysql</groupId>
			<artifactId>mysql-connector-java</artifactId>
			<scope>runtime</scope>
		</dependency>
		
		<dependency>
           <groupId>org.springframework.boot</groupId>
           <artifactId>spring-boot-starter-data-jpa</artifactId>
		</dependency>


```
### 修改application

```
########################################################
###datasource
########################################################
spring.datasource.url = jdbc:mysql://localhost:3306/test
spring.datasource.username = root
spring.datasource.password = abcd234
spring.datasource.driverClassName = com.mysql.jdbc.Driver
spring.datasource.max-active=20
spring.datasource.max-idle=8
spring.datasource.min-idle=8

spring.datasource.initial-size=10


########################################################
### Java Persistence Api
########################################################
# Specify the DBMS
spring.jpa.database = MYSQL
# Show or not log for each sql query
spring.jpa.show-sql = true
# Hibernate ddl auto (create, create-drop, update)
spring.jpa.hibernate.ddl-auto = update
# Naming strategy
spring.jpa.hibernate.naming-strategy = org.hibernate.cfg.ImprovedNamingStrategy
# stripped before adding them to the entity manager)
spring.jpa.properties.hibernate.dialect = org.hibernate.dialect.MySQL5Dialect

```
### 测试

新建一个类User.java;
```
package com.example.demo;

import javax.persistence.Entity;
import javax.persistence.GeneratedValue;
import javax.persistence.Id;

/**
 * Created by LM on 2017/8/6.
 */
@Entity
public class User{
    @Id
    @GeneratedValue
    private long id;
    private String name;

    public long getId() {
        return id;
    }

    public void setId(long id) {
        this.id = id;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }
}


```
重新运行，在navicat中查看发现多了一个user表

![](https://raw.githubusercontent.com/Hughendman/picture/master/springboot/22.png)

### 深入操作

#### 建表
ddl-auto:create会新建一个表如果你之前有这个表会被删掉
User.java 
```
package com.example.demo;

import javax.persistence.Entity;
import javax.persistence.GeneratedValue;
import javax.persistence.Id;

/**
 * Created by LM on 2017/8/6.
 */
@Entity
public class User{
    @Id
    @GeneratedValue
    private long id;
    private String name;
    private Integer age;

    public long getId() {
        return id;
    }

    public void setId(long id) {
        this.id = id;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public Integer getAge() {
        return age;
    }

    public void setAge(Integer age) {
        this.age = age;
    }
}


```
运行，多了age一列

ddl-auto: update 不会删掉，会保留
ddl-auto: create-drop 应用停下来删掉表
ddl-auto: none 什么都不做
validate：验证是否一致，不一致报错

### 写API操作数据库

#### get接口读取user表
新建一个类：UserController.java
```
package com.example.demo;

import java.util.List;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RestController;

@RestController
public class UserController {
	
	@Autowired
	private UserRepository userRepository;
	
	@GetMapping(value = "users")
	public List<User> userList(){
		return userRepository.findAll();
	}
}


```

新建一个Interface ：UserRepository
```
package com.example.demo;

import org.springframework.data.jpa.repository.JpaRepository;

public interface UserRepository extends JpaRepository<User, Integer>{

}


```
重启：http://127.0.0.1:8080/users

返回
```
[
    {
        "id": 1,
        "name": "yinxs",
        "age": 12
    }
]

```

#### 新增一条user信息

在UserController.java中添加

```

@PostMapping(value = "/user")
	public User userAdd(@RequestParam("name") String name,@RequestParam("age") Integer age){
		User user = new User();
		user.setName(name);
		user.setAge(age);
		
		return userRepository.save(user);
		
	}

```
重启：http://127.0.0.1:8080/users

name:yin

age:16

返回
```
{
    "id": 3,
    "name": "yin",
    "age": 16
}

```

### 错误处理

```
org.hibernate.TypeMismatchException: Provided id of the wrong type for class com.example.demo.User. Expected: class java.lang.Long, got class java.lang.Integer

```

原因：id的类型不对

```
could not execute statement; SQL [n/a]; constraint [PRIMARY]; nested excepti...

```

原因：数据库被修改

### 参考
[springboot链接数据库](https://blog.csdn.net/qq_27153901/article/details/76762264)

[两小时学会springboot](https://www.imooc.com/video/13599)

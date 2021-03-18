---
layout: post
title: "[Spring]SpringBoot war, Tomcat배포"
date: 2021-03-17 09:46:23 +0900
category: spring
---

# [Spring]SpringBoot war, Tomcat배포

세가지 과정으로 진행된다. 

- SpringBootServletInitializer 상속받는 것으로 변경

SpringBootServletInitializer를 상송받고, configure 함수를 Override한다. 

```java
package com.finger.homepage;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.boot.builder.SpringApplicationBuilder;
import org.springframework.boot.web.servlet.support.SpringBootServletInitializer;

@SpringBootApplication
public class FingerApplication extends SpringBootServletInitializer{

    public static void main(String[] args) {
        SpringApplication.run(FingerApplication.class, args);
    }

    @Override
    protected SpringApplicationBuilder configure(SpringApplicationBuilder builder){
        return builder.sources(FingerApplication.class);
    }
}
```

- Embed servlet 을 'provided' 로 변경

tomcat은 기본적으로 추가되어 있으나, scope태그를 바로잡기 위해서 적어주는듯?

```xml
<!-- marked the embedded servlet container as provided --> 
<dependency> 
    <groupId>org.springframework.boot</groupId> 
    <artifactId>spring-boot-starter-tomcat</artifactId> 
    <scope>provided</scope> 
</dependency>
```

- war로 빌드

<packaging>war</packaging> 을 pom.xml에 적어준다. 없으면 기본이 jar로 되어있어서 적어줘야된다. 

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 https://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>
    <parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>2.4.1</version>
        <relativePath/> <!-- lookup parent from repository -->
    </parent>
    <groupId>fingerHomepage</groupId>
    <artifactId>finger</artifactId>
    <version>0.0.1-SNAPSHOT</version>
    **<packaging>war</packaging>**
```

- maven clean package 명령어 입력

실행 후 target 폴더 아래에 war파일이 생성된다. 

참고 URL : 

[Spring Boot WAR 로 배포하기 (How to deploy with *.war file with Spring Boot)](https://4urdev.tistory.com/84)

[https://regyu.tistory.com/](https://regyu.tistory.com/3)3
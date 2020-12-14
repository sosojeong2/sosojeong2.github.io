---
layout: post
title: "[Board]게시판만들기-환경설정,폴더구조"
date: 2020-12-14 14:37:23 +0900
category: 게시판만들기
---

# 게시판 만들기 - 환경설정

## 개발환경
* vscode
* java 1.8
* gradle 
* mariadb 

## 라이브러리
* jpa 
* Lombok

<br />

## VSCode 에서 SpringBoot 프로젝트 생성하기
1. Ctrl + Shift + p 를 누르고 >Spring initializr: Create a Gradle Project 선택   
2. 버전 선택 (나는 v.2.3.4.RELEASE)  
3. 프로젝트 언어 선택 (Java)  
4. 프로젝트 Group Id 입력 : ex) com.example   
5. 프로젝트 Artifact Id 입력 : ex) demo  
6. 프로젝트 packaging type 선택 : war, jar  
7. java 버전 선택 : 1.8  
8. 라이브러리 선택 : Lombok, Spring Web, 등등 선택   

<br />

## 폴더 구조 
```
src
  main
    java\com\soso\test
      web
        controller
        dto
      service
      domain
      common
      config
      util
    resources
      message
      META-INF  
      domain
    webapp\WEB-INF\jsp
```

<br />

## DAO, DTO, VO, Entity Class의 차이
> DAO (Data Access Object)란?

실제로 디비에 접근하는 객체이다.     
Service와 DB를 연결한다.    
SQL를 사용하여 (개발자가 직접 코딩) DB에 접근한 후 적절한 CRUD API를 제공한다.     

<br />

> DTO (Data Transfer Object)

계층간 데이터 교환을 위한 객체이다.     
DB에서 데이터를 얻어 service나 controller로 보낸때 사용하는 객체를 말한다.     
로직을 가지고 있지 않은 순수한 데이터 객체이며, getter, setter 메서드 만을 가진다. ( DB에서 꺼낸값을 임의로 변경할 필요가 없기때문에 필요할 때만 setter를 사용하고 웬만하면 setter를 만들지 않는다.)   
Request, Response용 DTO는 view를 위한 클래스 (자주 변경이 필요한 클래스이다. )    

<br />

> VO (Value Object) 

VO는 DTO는 비슷한 개념이지만 read only 속성을 갖는다.     
VO는 특정 비즈니스 값을 담는 객체이고, DTO는 Layer간의 통신 용도로 오고가는 객체를 말한다.    

<br />

> Entity Class

실제 DB의 테이블과 매칭될 클래스.    
entity class와 dto 클래스를 분리하는 이유는 view layer와 db layer를 철저하게 분리하기 위해서이다. 

<br />

## 전체 구조(package 기준)  
Client -(dto)- controller -(dto)- service -(dto)- repository(dao) - domain(entitiy) - db




<br/>

* * *  
참고   
https://gmlwjd9405.github.io/2018/12/25/difference-dao-dto-entity.html
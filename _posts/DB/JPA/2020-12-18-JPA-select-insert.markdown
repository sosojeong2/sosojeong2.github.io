---
layout: post
title: "[MariaDB]JPA-개념,기본설정"
date: 2020-12-18 10:32:23 +0900
category: DB
---

# INSERT문에 SELECT문 사용하기

service에서 seq는 마지막 seq에서 +1로 하고, 날짜도 현재날짜 구해서 insert를 하려니깐 코드가 길어졌다.. ㅠ
 
대리님이 그냥 디비 insert 할 때 바로 하래서 찾아봤당 ㅎㅎ 

## Mariadb 문법

``` SQL

INSERT INTO tbl_temp2 (fld_id)
  SELECT tbl_temp1.fld_order_id
  FROM tbl_temp1 
  WHERE tbl_temp1.fld_order_id > 100;

```
<br />

## Springboot에서 JPA 설정

Entity class 생성
```java

public class testInfo extends BaseTimeEntity {
    @Id
    @GenerateValue(strategy = GenerationType.IDENTITY)
    @Colume(name = "TEST_INFO_ID")
    private Long id;

    @Colume(name = "ID")
    private String id;
    
    @Colume(name = "NM")
    private String nm;

    
}

```
Repository 클래스 생성
```java

public interface testRepository extends<testInfo, Long> {
    void saveJoblog(@Param("job_id") String id, @Param("nm") String nm);
}

```

application.yml 에 jpa설정하는 부분에 xml 추가를 해준다.

``` yml

  #JPA 설정
  jpa:
    open-in-view: true
    show-sql: false
    mapping-resources:
      - META-INF/ormJobInfo.xml 
    hibernate:
      ddl-auto: none
      naming:
        physical-strategy: org.hibernate.boot.model.naming.PhysicalNamingStrategyStandardImpl
    properties:
      hibernate:
        format_sql: true

```

service에서 저장하는함수 호출

``` java

  TestInfoRepository testInfoRepository;
  testInfoRepository.saveTestlog();

```

직접 쿼리를 짤 수 있다. ㅎㅎ
```xml

  <named-native-query name="TestInfo.saveTestlog">
    <query>
      <![CDATA[
        
        INSERT INTO TEST_INFO(ID,NM,SEQ,DATES)
        SELECT  ID
               , NM
               , IFNULL(MAX(SEQ), 0) +1
               , DATE_FORMAT(NOW(), '%Y%m%d')
          FROM  TEST_INFO
         WHERE  DATES=DATE_FORMAT(NOW(), '%Y%m%d')

      ]]>
    </query>
  </named-native-query>

```
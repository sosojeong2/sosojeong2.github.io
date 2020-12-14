---
layout: post
title: "[gradle]Gradle 빌드시스템 기초, 빌드자동화"
date: 2020-12-02 12:27:23 +0900
category: gradle
---

# Gradle 빌드시스템 기초, 빌드자동화

## gradle wrapper를 사용하는 목적      
 
이미 존재하는 프로젝트를 새로운 환경에 설치할 때 별도의 설치나 설치과정 없이 빌드할 수 있게 하기 위함이다.

<br/>

## gradlew 파일

유닉스용 실행 스크립트   
`gradle build` 이와같은 방법으로 실행하면 gradle이 설치되어 있어야 하고 버전이 호환되지 않을 수 있음   
`./gradlew build` wrapper를 사용하면 그런 문제가 없음   


<br/>

## gradlew.bat 파일

윈도우용 실행 배치 스크립트  

<br/>

## gradle/wrapper/gradle-wrapper.jar  

wrapper 파일
gradlew나 gradlew.bat 이 gradle task를 실행하기 때문에 로컬환경의 영향을 받지 않음

<br/>

## gradle/wrapper/gradle-wrapper.properties 파일

gradle wrapper 설정파일
이 파일의 wrapper 버전등을 변경하면 task 실행 시 자동으로 wrapper파일을 로컬 캐시에 다운받는다.  
  
나같은 경우는 폐쇄망에서 작업했기 때문에  **distributionUrl=gradle-6.4.1-bin.zip** 로 변경하였다.  

<br/>

## build.gradle 파일

의존성이나 플러그인 설정 등을 위한 스크립트 파일

<br/>

## settings.gradle 파일

프로젝트의 구성정보를 기록하는 파일

<br/>

## 의존성 관리

repositories를 사용해서 의존성을 가져올 주소를 설정
dependsOn 을 사용해서 task간의 의존성을 만들 수 있다.
  
```console
task squid {
    doLast {
        println 'Hello!! Codingsquid'
    }
}
task octopus(dependsOn: squid) {
    doLast {
        println "I'm Octopus"
    }
}
```

<br/>

## DSL 

Domain-Specific Language는 특정 도메인에 특화된 비교적 작고 간단한 프로그래밍 언어

<br/>

## sourceSet (아직 잘 모르겠음...)

자바 플러그인 안에는 Source Set이라는 개념이 들어가 있으며 이는 함께 컴파일과 실행되는 소스 파일들의 그룹을 뜻함
자바 소스 파일과 리소스 파일이 들어간다.
다른 플러그인들이 그루비나 스칼라 소스를 추가할 수 있다.
컴파일 클래스패스와 런타임 클래스 패스와 관련됨
자바 기본 source set -> main : 실제 작동소스코드. 컴파일해서 Jar파일로 들어감, test : 단위테스트 소스코드. 컴파일해서 JUnit이나 TestNG로 실행


<br/>

## 빌드 자동화란?

빌드 환경 설정
보통 코드를 작성하면 컴파일을 해서 실행파일 또는 java의 jar와 같은 라이브러리 파일을 생성한다. 
언어마다 차이가 있지만 이런 작업들을 자동화 하는게 빌드자동화 이다.


<br/>

## gradle의 장점

* 간결함  
gradle은 groovy라는 언어를 활용한 DSL을 스크립트로 사용한다.
groovy라는 언어를 몰라도 gradle스크립트에 익숙해질 수 있다.

* 문서화
공식홈페이지에 문서화가 잘 되어있다.

* 속도
성능향상을 위해 다양한 기능을 제공한다. 

* 멀티 프로젝트
gradle은 멀티 프로젝트 구성이 가능하다.
하나의 repository내에 여러개의 하위 프로젝트를 구성할 수 있다.
상위 프로젝트의 의존성 및 설정을 하위 프로젝트에서 상속받아 사용할 수 있기 때문에 중복 설정이 불필요하다.

* 유연성 + 확장성
직적 task를 구현하고, 플러그인을 만들어서 기능을 추가할 수 있다.

<br/>

* * *
참고URL

https://effectivesquid.tistory.com/entry/Gradle-%EB%B9%8C%EB%93%9C%EC%8B%9C%EC%8A%A4%ED%85%9C-%EA%B8%B0%EC%B4%88    

https://medium.com/@goinhacker/%EC%9A%B4%EC%98%81-%EC%9E%90%EB%8F%99%ED%99%94-1-%EB%B9%8C%EB%93%9C-%EC%9E%90%EB%8F%99%ED%99%94-by-gradle-7630c0993d09
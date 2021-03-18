---
layout: post
title: "[Spring]@NotNull, @NotEmpty, @NotBlank의 차이점 및 사용법"
date: 2021-03-18 09:46:23 +0900
category: spring
---

# [Spring]@NotNull, @NotEmpty, @NotBlank의 차이점 및 사용법

지금까지는 null체크(필수값체크)를 할 때 controller에서 하였다. 

이러한 조건문과 메서드 들은 변수별 특징과 조건에 관련해 공통화 하기가 쉽지 않았다.

이런경우 쉽게 사용할 수 있는 것이 @NotNull, @NotEmpty, @NotBlank 이다.

- **NotNull : 이름그대로 null만 허용하지 않는다.**

    따라서 "", " " 는 허용한다. 

DTO에서 @NotNull을 설정하고 사용하고자 하는 Controller에서 @RequestBody 앞에 @Valid를 추가해주면 설정되 Bean Validation을 사용할 수 있다. 

```java
@PostMapping("/login")
public ResponseEntity login(@Valid @RequestBody UserLoginRequestDto loginUser) {    
    UserLoginResponseDto login = userService.login(loginUser);
    return new ResponseEntity<>(new BaseResult.Normal(login), HttpStatus.OK);
}
```

- **NotEmpty : null과 "" 둘 다 허용하지 않는다.**

    즉 " "는 허용된다.

- **NotBlank : null과 "", " " 모두 허용되지 않는다.**

pom.xml에 추가

```xml
<!-- https://mvnrepository.com/artifact/javax.validation/validation-api -->
<dependency>
    <groupId>javax.validation</groupId>
    <artifactId>validation-api</artifactId>
    <version>2.0.1.Final</version>
</dependency>
```

참고 URL : 

[[Spring Boot] @NotNull, @NotEmpty, @NotBlank 의 차이점 및 사용법](https://sanghye.tistory.com/36)
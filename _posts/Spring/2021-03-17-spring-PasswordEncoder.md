---
layout: post
title: "[Spring]PasswordEncoder"
date: 2021-03-17 09:45:23 +0900
category: spring
---

# [Spring]PasswordEncoder

Spring Security 에서는 비밀번호를 안전하게 저장할 수 있도록 다양한 암호화를 지원하는 PasswordEncoder 인터페이스와 구현체들을 제공한다.

- BCryptPasswordEncoder : Bcrypt 해시함수를 사용
- Argon2PasswordEncoder : Argon2 해시함수를 사용
- Pbkdf2PasswordEncoder : Pbkdf2 해시함수를 사용
- SCryptPasswordEncoder : SCrypt 해시함수를 사용

내가 사용한 것은 **BcryptPasswordEncoder**이다.

암호화 할 때 따로 salt를 사용하지 않고도 암호화하는 과정에서 랜덤된 키 값을 이용해 암호화하여 매번 새로운 값을 만들어 준다. 

참고 URL :  

[Springframework security의 BCryptPasswordEncoder 사용하기](https://jhlblue.tistory.com/19)

[[Spring Boot] 스프링 부트에서 비밀번호 암호화하기](https://devlog-wjdrbs96.tistory.com/212)
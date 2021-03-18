---
layout: post
title: "[Spring]@Transactional"
date: 2021-03-17 09:44:23 +0900
category: spring
---

# [Spring]@Transactional

SI에서 비즈니스 로직이 Controller에서 절차지향적으로 많이 짜여져 있다. 

그래서 비즈니스 로직을 하나의 트랜잭션 단위로, service쪽으로 옮기는 작업을 하였다. 

이로 인해 객체지향적으로 코딩할 수 있게 되었고 재사용이 가능해진다. 

1. 트랜잭션의 성직
- 원자성 : 한 트랜잭션 내에서 실행한 작업들은 하나로 간주하여, 모두 성공 또는 모두 실패로 본다.
- 일관성 : 트랜잭션은 일관성 있는 데이터베이스 상태를 유지한다.
- 격리성 : 동시에 실행되는 트랜잭션들이 서로 영향을 미치면 안된다.
- 지속성 : 트랜잭션을 성공적으로 마치면 항상 저장되어야 한다.

2. 스프링에서 트랜잭션 처리 방법

스프링에서는 트랜잭션 처리를 지원하는데 그중 어노테이션 방식으로(@Transactional) 사용하는게 일반적이며, 선언적 트랜잭션이라고 부른다. 

클래스, 메서드 위에 @Transactional이라고 추가해주면 이 클래스는 트랜잭션 기능이 적용된 프록시 객체가 생성된다. 

이 프록시 객체는 @Transactional이 포함된 객체가 호출될 경우 PlatformTransactionManager를 사용하여 트랜잭션을 시작하고, 정상여부에 따라 Commit 또는 Rollback한다. 

3. 트랜잭션 롤백 예외

- 선언적 트랜잭션에서는 **런타임 예외가 발생하면 롤백**한다.
- 반면 예외가 발생하지 않거나 **체크 예외가 발생**하면 커밋한다.

    체크 예외를 커밋을 하는 이유는 체크 예외가 예외적인 상황에서 사용되기 보다는 리턴값을 대신해서 비즈니스적인 의미를 담은 결과를 돌려주는 용도로 많이 사용되기 때문이다. 

    (이것은 스프링프레임워크가 EJB에서의 관습을 따르기 때문이라고 합니다.)

- 체크 예외지만 롤백 대상으로 삼아야 하는 것이 있다면 XML의 rolback-for 애트리뷰트나 애노테이션의 rollbackFor 또는 rollbackForClassName 앨리먼트를 이용해서 예외를 지정하면 된다.

4. 사용법

@Transactional 어노테이션의 rollbackFor 옵션을 이용하면 롤백이 되는 클래스를 지정할 수 있다. 

```java
@Transactional(rollbackFor = Exception.class)

@Transactional(rollbackFor = {RuntimeException.class, Exception.class})
```

추가적으로 특정 예외가 발생하면 롤백이 되지 않도록 지정하는 방법이다. 

```java
@Transactional(noRollbackFor = {IgnoreRollbackException.class})
```

참고 URL : 

[[Spring] Transactional 정리 및 예제](https://goddaehee.tistory.com/167)

[아노테이션 드리븐 트랜잭션(@Transactional)에서 Exception을 throw할 경우 롤백(rollback)이 안됩니다.](https://offbyone.tistory.com/405)
---
layout: post
title: "[java]Optional, null 처리"
date: 2021-04-12 09:36:23 +0900
category: java
---

# [java]Optional, null 처리

## null 관련 문제점

크게 두가지로 요약된다

1. 런타임에 NPE(NullPointException)이라는 예외를 발생시킬 수 있다.
2. NPE 방어를 위해서 들어간 null 체크 로직 때문에 코드 가독성과 유지 보수성이 떨어진다.

함수형 언어들은 전혀 다른 방법으로 이 문제를 해결한다.

자바에서 존재하지 않는 값으로 null을 사용했다면, 함수형 언어들은 존재할지 안할지 모르는 값을 표현할 수 있는 별개의 타입을 가지고 있다.

java.util.Optional<T>를 새로 도입하였다.

## Optional 이란?

Optional은 "존재할 수 있지만 안 할 수도 있는 객체" 즉 null이 될 수도 있는 객체를 감싸고 있는 일종의 래퍼 클래스이다. 원소가 없거나 최대 하나밖에 없는 Collection이나 Stream으로 생각하면 좋다.

## Optional 기본 사용법

```java
Optional<T> 변수명; // T 타입의 객체를 감쌀 수 있는 Optional 타입의 변수
```

## Optional 객체 생성하기

간편하게 객체 생성을 할 수 있도록 3가지 정적 팩토리 메소드를 제공한다.

- Optional.empty()

    null을 담고 있는, 한마디로 비어있는 Optional 객체를 얻어온다.

- Optional.of(value)

    null이 아닌 객체를 담고 있는 Optional 객체를 생성한다. null이 넘어올 경우, NPE를 던지기 때문에 조심해서 사용해야한다.

- Optional.ofNullable(value)

    null인지 아닌지 확신할 수 없는 객체를 담고있는 Optional 객체를 생성한다.

## Optional이 담고 있는 객체 접근하기

Optional 클래스는 담고 있는 객체를 꺼내오기 위해서 다양한 인스턴스 메소드를 제공한다.

아래 메소드들은 모두 Optional이 담고 있는 객체가 존재할 경우 동일하게 해당 값을 반환한다.

반면 Optional이 비어있는 경우(Null을 담고 있는 경우) 다르게 작동한다.

- get()

    비어있는 Optional 객체에 대해 NoSuchElementException을 던집니다.

- orElse(T other)

    비어있는 Optional 객체에 대해 넘어온 인자를 반환한다. 

- orElseGet(Supplier<? extends T> other)

    비어있는 Optional 객체에 대해 넘어온 함수형 인자를 통해 생성된 객체를 반환한다.

- orElseThrow(Supplier<? extends X> exceptionSupplier)

    비어있는 Optional 객체에 대해서 넘어온 함수형 니자를 통해 생성된 예외를 던진다.

get() 메소드는 비어있는 Optional 객체를 대상으로 호출할 경우, 예외를 발생시키므로 객체 존재여부를 bool 타입으로 반환하는 isPresent()라는 메소드를 통해 null체크가 필요하다.

```java
String text = getText();
Optional<String> result = Optional.ofNullable(text);
if(result.isPresent()){
	// ~~~
}
```

참조 URL : 

[자바8 Optional 2부: null을 대하는 새로운 방법](https://www.daleseo.com/java8-optional-after/)
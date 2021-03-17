---
layout: post
title: "[java]Field vs Constructor vs Setter Injection 그리고 순환참조"
date: 2021-03-17 09:36:23 +0900
category: java
---

# [java]Field vs Constructor vs Setter Injection 그리고 순환참조

- Field Injection (필드 주입)

보통 우리가 많이 쓰는 의존주입은?

Field Injection이다.  → @Autowired 를 붙여 빈을 주입한다.

```java
@Component
public class ABean {
    
    @Autowired
    private BBean b;
    
    public void bMethod() {
        b.print();
    }
    
}
```

- Constructor Injection (생성자 주입)

생성자를 위한 빈 주입은 위와같이 생성자의 매개변수로 의존주입할 빈을 매개변수로 넣어준다.

스프링 4.3 버전 이후에 생상자 의존주입에 @Autowired를 넣을 필요가 없다. 

```java
@Component
public class ABean {
    
    private BBean b;
    
    public ABean(BBean b) {
        this.b=b;
    }
    
    public void bMethod() {
        b.print();
    }
    
}
```

- Setter Injection (수정자 주입)

세터를 이용한 빈주입이다. 의존 주입할 빈 객체에 대한 Setter를 만들어주고 @Autowired를 붙여준다.

```java
@Component
public class ABean {
    
    private BBean b;
    
    @Autowired
    public void setB(BBean b) {
        this.b = b;
    }
 
    public void bMethod() {
        b.print();
    }
    
}
```

참조 URL :

[[Spring] 생성자 주입 vs 필드 주입 (@Autowired)](https://jackjeong.tistory.com/41)

[Spring - Field vs Constructor vs Setter Injection 그리고 순환참조(Circular Reference)](https://coding-start.tistory.com/250)
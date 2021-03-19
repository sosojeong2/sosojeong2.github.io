---
layout: post
title: "[jsp]onclick 링크(새창/팝업/현재창/프레임)"
date: 2021-03-19 09:42:23 +0900
category: jsp
---

# [jsp]onclick 링크(새창/팝업/현재창/프레임)

- 현재페이지에서 부를 때

```java
<button onclick="location.href='address'">button</button>
```

- 새창에서 열 때

```java
<button onclick="window.open('address')">button</button>
```

- 팝업으로 열기(주소, 팝업창이름, 옵션)

```java
<button onclick="window.open('address','window_name','width=430,height=500,location=no,status=no,scrollbars=yes');">button</button>
```

- 상위 프레임에 부를때

```java
<button onclick="parent.location.href='address'">button</button>
```

- 프레임 지정 링크

```java
<button onclick="타켓명.location.href='address'">button</button>
```

참조 URL : 

[onclick 링크 (새창/팝업/현재창/프레임) - Wallel](https://wallel.com/onclick-%EB%A7%81%ED%81%AC-%EC%83%88%EC%B0%BD%ED%8C%9D%EC%97%85%ED%98%84%EC%9E%AC%EC%B0%BD%ED%94%84%EB%A0%88%EC%9E%84/)

[[JavaScript] 부모창과 자식창의 값 전달](https://all-record.tistory.com/149)
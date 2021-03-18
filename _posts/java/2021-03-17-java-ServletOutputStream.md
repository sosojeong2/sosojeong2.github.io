---
layout: post
title: "[java]ServletOutputStream"
date: 2021-03-17 09:38:23 +0900
category: java
---

# [java]ServletOutputStream

파일을 읽어올 때는 FileInputStream으로 읽어온 뒤 브라우저에 출력할 때는 ServletOutputSream를 사용한다. 

ServletOutputStream의 용도는 게시판에 파일을 올릴 때 사용한다. 

ServletOutputStream은 추상클래스이기 때문에 인스턴스를 생성할 수 없다. 

ServletResponse 클래스에 getOutputStream()이라는 함수를 통해 ServletOutputSream 인스턴스를 받아서 사용해야한다. 

```java
public void test(HttpServletResponse response){
	ServletOutputStream servletOutputStream = response.getOutputStream();
}
```
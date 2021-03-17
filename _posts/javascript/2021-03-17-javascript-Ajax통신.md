---
layout: post
title: "[javascript]Ajax통신"
date: 2021-03-17 09:32:23 +0900
category: DB
---

# [javascript]Ajax통신

Ajax (**A**synchronous **J**avascript **a**nd **X**ML )

자바스크립트를 통해서 비동기적으로 브라우저와 서버가 데이터를 주고 받는 방식을 말한다. 

DOM을 제어해서 서버로부터 리턴받은 데이터를 가지고 렌더링한다. 

- 비동기통신의 이점 :

    - 통신 후 데이터 바인딩하는 동안 사용자가 어플리케이션을 사용할 수 있다. 

    - 전체 페이지 로딩 시 모든 데이터를 서버로부터 받는것이 아니라 필요한 부분을 그때마다 일부분 렌더링 시키는 것이 가능하다. 

- Ajax 동작방식
    1. 서버로 정보 요청 : 이벤트 발생 → 생성함수 호출 → 서버요청 객체생성 및 메서드 호출
    2. 서버 내부 처리 후 응답 (json 또는 xml형태로 데이터 전달)
    3. 응답을 받으면 이벤트 발생, 이벤트의 콜백함수 호출

- 예시

```java
$.ajax({
      url : "/board/noticeWrite",
      type : "post",
      contentType : false,
      processData : false,
      async: false,
      data : formData,
      success : function (result) {
          if(result.resultCode == '0000'){
              alert("저장되었습니다.");
              location.href="noticeList";
          }else{
              alert("저장이 실패되었습니다.");
          }
      },
      error : function(){
          alert("관리자에게 문의하세요.");
      }
  });
```

참고 URL : 

[AJAX 의 클라이언트와 서버 비동기 통신](https://webdevtechblog.com/ajax-%EC%9D%98-%ED%81%B4%EB%9D%BC%EC%9D%B4%EC%96%B8%ED%8A%B8%EC%99%80-%EC%84%9C%EB%B2%84-%EB%B9%84%EB%8F%99%EA%B8%B0-%ED%86%B5%EC%8B%A0-d681c905e2a9)
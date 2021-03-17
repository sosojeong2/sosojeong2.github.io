---
layout: post
title: "[java]ClientAbortException"
date: 2021-03-17 09:32:23 +0900
category: java
---

# [java]ClientAbortException

org.apache.catalina.connector.ClientAbortException 

위 오류는 외부에 의해 톰캣에서 발생한 오류이다. 

즉, 어플리케이션에서 나는 오류가 아니다.

그래서 로그상에서 표시되는 오류 위치가 프로젝트 내부를 향하지 않는다.

- 특정 페이지를 출력하는데 데이터를 조회하는데 시간이 오래 걸리는 경우

특정 데이터를 비동기로 로딩했기 때문에 확면을 사용하는데는 문제가 없지만, 조회가 오래 걸리다 보니 조회가 완료되기 전에 다른테이지로 이동하기 때문에 발생한다.

나 같은 경우는 jsp에서 ajax로 통신하는데 통신이 끝나기도 전에 다른페이지로 이동하는 경우가 발생했고 ( BCryptPasswordEncoder ) 그럴때마다 위와같은 오류가 발생하였다.

클라이언트의 요청을 톰캣에서 받고 다시 클라이언트에게 전달해야 하는데 이때 요청한 클라이언트가 사라진 상황이었다.
---
layout: post
title: "[jsp]onsubmit"
date: 2021-03-17 09:42:23 +0900
category: jsp
---

# [jsp]onsubmit

login.jsp 에서 button을 클릭하면 login() 함수를 타게 되어있다.

login()함수에는 ajax통신을 하고 결과값으로 loginYn값이 온다. 

성공이면 로그인성공이 뜨면서 location.href= "noticeList" 로 이동한다. 

그러나 넘어가지 않고 리다이렉션 되는 현상이 발생하였다.

알고보니 form 에서 submit()이 작동되었기 때문이다. 

form 을 인위적으로 막기위해서는 onsubmit="return false" 을 사용하면 된다.
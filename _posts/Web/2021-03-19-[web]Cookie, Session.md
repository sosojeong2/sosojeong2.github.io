---
layout: post
title: "[web]Cookie, Session"
date: 2021-03-19 13:42:23 +0900
category: web
---

# [web]Cookie, Session

- Cookie 쿠키

쿠키는 클라이언트 측에서 로컬 웹 브라우저가 저장하는 데이터다.

인증을 하는 과정에서 처음에 서버는 쿠키를 생성해서 클라이언트에게 보내고 **클라이언트는 쿠키를 웹 브라우저에 key-value 형식으로 저장**하게된다.

이후 클라이언트가 데이터를 요청 시 헤더에 쿠키를 실어서 서버에 보내게 된다.

따라서 로그인 정보가 쿠키에 담겨져있다면 더이상의 인증은 필요없게된다.

- Session 세션

세션도 쿠키와 동일하게 클라이언트의 인증 상태정보를 저장하게 되는데 쿠키와 달리 **서버에 저장**된다는 차이점이 있다. 

외부에서 세션정보를 열람하여도 개인 로그인 정보와 매칭이 불가능하다

즉, 유출되어도 역으로 확인할 수 없는 정보를 담고 있어야 한다. 이를 위해서 쿠키를 활용하게 되는데 서버에서 고유 세션 id를 클라이언트에게 보내게되면 이를 클라이언트는 웹 브라우저에 저장한다. 그리고 그 다음부터 인증을 위해서는 클라이언트의 쿠키에 저장되어있는 고유세션id를 서버측에 보내게 된다. 

![alt text](/public/img/web_image_file/cookie,session1.png)

**로그인 구현하기**

우선 나는 session으로만 구현해보았다. (왜냐 자동로그인 기능이 없다.. 쿠키를 굳이 안써도 될거같았다.)

- 로그인 컨트롤러

```java
// 로그인 프로세스 
    @ResponseBody
    @RequestMapping(value="/login1", method = RequestMethod.POST)
    public Map<String, Object> goLogin1(HttpServletRequest request, 
				@RequestBody UserDTO userDTO, HttpServletResponse response) throws Exception {
        
				log.info("============= goLogin1 =============");
        log.info("============= userDTO : {}", userDTO.toString());

        Map<String, Object> paramMap = new HashMap<String, Object>();
        Map<String, Object> resultMap = new HashMap<String, Object>();
        
        Boolean loginYn = false;
        String password = "";

        // 입력받은 password를 저장 
        password = userDTO.getUserPw();

        // pw 암호화
        BCryptPasswordEncoder bcryptPasswordEncoder = new BCryptPasswordEncoder();

        // id로 user정보를 db에서 가져옴
        paramMap.put("id", userDTO.getUserId());
        userDTO = commonService.goLogin(paramMap);
        
        HttpSession session = request.getSession();

        // 디비에서 가져온 값이 없으면 회원정보가 없다. 
        if(userDTO == null){
            resultMap.put("loginYn", false);
            session.setAttribute("loginYn", loginYn); 
        }else{
            // db에서 가져온 암호화된 pwd와 입력받은 pwd가 맞는지를 확인
            loginYn = bcryptPasswordEncoder.matches(password, userDTO.getUserPw());
            resultMap.put("loginYn", loginYn);
            session.setAttribute("loginYn", loginYn); 
            session.setAttribute("id", userDTO.getUserId());

        }

        return resultMap;
    }
```

- login.jsp

```java
<%@ page language="java" contentType="text/html; charset=UTF-8" pageEncoding="UTF-8"%>
<%
    session.invalidate();
%>
```

서버상에 있는 모든 세션변수의 값을 삭제하려면 invalidate()를 이용한다. 

- list.jsp

```java
<%@ page language="java" contentType="text/html; charset=UTF-8" pageEncoding="UTF-8"%>
<% 
    String id = (String) session.getAttribute("id");
    Boolean loginYn = (Boolean) session.getAttribute("loginYn");

    try{
        if(loginYn == null || !loginYn){
            response.sendRedirect("login");
        }
    } catch(Exception e){
        
    }
%>
```

세션변수 loginYn의 값이 null이거나 false이면 로그인 페이지로 리다이렉트한다.

참고 URL : 

[로그인은 어떻게 이루어질까❓(Cookie, Session)](https://velog.io/@junhok82/%EB%A1%9C%EA%B7%B8%EC%9D%B8%EC%9D%80-%EC%96%B4%EB%96%BB%EA%B2%8C-%EC%9D%B4%EB%A3%A8%EC%96%B4%EC%A7%88%EA%B9%8CCookie-Session)

[쿠키와 세션을 이용한 자동 로그인 처리](https://rongscodinghistory.tistory.com/3)

[쿠키와 세션을 이용한 자동 로그인 처리](https://rongscodinghistory.tistory.com/3)
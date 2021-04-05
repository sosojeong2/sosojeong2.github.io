---
layout: post
title: "[java]getSession(), getSession(true), getSession(false) 차이점"
date: 2021-04-05 09:36:23 +0900
category: java
---

# [java]getSession(), getSession(true), getSession(false) 차이점

- getSession(), getSession(true)

    HttpSession이 존재하면 현재 HttpSession을 반환하고 존재하지 않으면 새로 세션을 생성합니다.

- getSession(false)

    HttpSession이 존재하면 현재 HttpSession을 반화하고 존재하지 않으면 새로 생성하지 않고 null을 반환한다.
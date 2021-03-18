---
layout: post
title: "[JQuery]img src 변경하기"
date: 2021-03-18 09:41:23 +0900
category: jqeury
---

# [JQuery]img src 변경하기

```jsx
<img id="img1">
```

위와 같은 태그의 src를 변경하려면 아래와 같이 attr을 사용하면 된다.

```jsx
$("#img1").attr("src", "이미지 경로");
```
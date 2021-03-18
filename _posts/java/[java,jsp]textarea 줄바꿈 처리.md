---
layout: post
title: "[java, jsp]textarea 줄바꿈 처리"
date: 2021-03-17 09:33:23 +0900
category: [java, jsp]
---

# [java/jsp]textarea 줄바꿈 처리

- java 단에서 처리

```java
// 예시
// 안녕 난 소정이야
// 만나서 반가워

String content = "안녕 난 소정이야\n 만나서 반가워";
content.replace("\n", "<br>");
```

- jsp 단에서 처리

```jsx
var content = result.list[0].content.replaceAll("<br>", "\n");
$('#content').val(content);
```
---
layout: post
title: "[javascript]문자열 자르기"
date: 2021-03-18 09:32:23 +0900
category: javascript
---

# [javascript]문자열 자르기

```jsx
var string = '2021-03-16'
```

- Split

```jsx
var strArray = string.split('-');
```

문자열에 있는 '-'(특정문자)를 기준으로 자를거면 split을 사용하면 된다.

- substring(시작인덱스, 종료인덱스)

문자열에서 기준없이 사용하고 싶은 문자열만 가져오고 싶으면 substring 함수를 사용한다. 

```jsx
var year = string.substring(0,4);  // 2021
var month = string.substring(5,7); // 03
var day = string.substring(8,10);  // 16
```

- substr(시작인덱스, 길이)

```jsx
var year = string.substring(0,4);  // 2021
var month = string.substring(5,2); // 03
var day = string.substring(8,2);  // 16
```
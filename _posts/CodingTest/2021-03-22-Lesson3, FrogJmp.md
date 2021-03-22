---
layout: post
title: "Lesson3, FrogJmp"
date: 2021-03-22 17:15:23 +0900
category: CodingTest
---

# Lesson3, FrogJmp

## 문제

개구리가 건너편으로 건너고 싶어한다. 

현재 위치는 X 이고 Y보다 크거나 같은 위치에 도달하고 싶어한다.

개구리는 항상 고정된 거리인 D만큼 점프할 수 있다.

세개의 정수 X, Y, D 가 주어지고 최소 점프 수를 구하여라!

ex) x = 10, y = 85, D = 30  return = 3

- X, Y 및 D는 [1 .. 1,000,000,000 ] 범위 내의 정수입니다 .
- X ≤ Y

## 풀이

개구리가 가야할 거리는 Y-X 이고, 점프 거리 D로 나눈 값을 올림하면 점프 횟수가 나온다.

```java
// you can also use imports, for example:
import java.util.*;

// you can write to stdout for debugging purposes, e.g.
// System.out.println("this is a debug message");

class Solution {
    public int solution(int X, int Y, int D) {
        // write your code in Java SE 11
        return (int) Math.ceil( ( (double) Y - (double) X) / (double) D );
    }
}
```

## 놓친점

만약 Y-X 가 0보다 작다면 return 0을 해준다.

```java
// you can also use imports, for example:
import java.util.*;

// you can write to stdout for debugging purposes, e.g.
// System.out.println("this is a debug message");

class Solution {
    public int solution(int X, int Y, int D) {
				int distance = Y - X;
				if(distance <= 0) return 0;

				double jumpCount = distance / (double) D;
        return (int) Math.ceil(jumpCount);
    }
}
```
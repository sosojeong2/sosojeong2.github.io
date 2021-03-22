---
layout: post
title: "Lesson4, MissingInteger"
date: 2021-03-22 17:15:23 +0900
category: CodingTest
---

# Lesson4, MissingInteger

## 문제

N개의 정수 배열 A가 주어지면 A에서 발생하지 않는 가장 작은 양의 정수를 반환한다. 

예를들어 A=[1,3,6,4,1,2]가 주어지면 함수는 5를 반환한다.

A=[1,2,3]이 주어지면 4를 반환해야한다.

A[-1,-3]이 주어지면 1을 반환한다.

- N은 [1 ..100,000] 범위 내의 정수 이고;
- 배열 A의 각 요소는 [−1,000,000..1,000,000] 범위 내의 정수 입니다.

## 풀이

Detected time complexity: **O(N) or O(N * log(N))**

```java
// you can also use imports, for example:
import java.util.*;

// you can write to stdout for debugging purposes, e.g.
// System.out.println("this is a debug message");

class Solution {
    public int solution(int[] A) {
        // write your code in Java SE 11
        Set set = new HashSet<>();

        for (int i : A) {
            set.add(i);
        }

        // 1부터 시작해서 없으면 리턴
        for(int i=1; i<=Integer.MAX_VALUE; i++) {
            if(!set.contains(i)) { return i; }
        }

        return -1;
    }
}
```

## 놓친점

## 다른풀이
---
layout: post
title: "Lesson5, CountDiv"
date: 2021-03-25 17:15:23 +0900
category: CodingTest
---

# Lesson5, CountDiv

## 문제

주어진 세개의 정수 A, B, K 가 주어지면 K로 나눈 수 있는 [A...B] 범위 내의 정수 수를 반환한다.

예를들어 A=6, B=11, K=2이면 2로 나눌 수 있는 숫자 6, 8, 10 이 있으므로 3을 반환한다.

- A와 B는 [ 0 .. 2,000,000,000 ] 범위 내의 정수입니다.
- K는 [ 1 .. 2,000,000,000 ] 범위 내의 정수입니다 .
- A ≤ B.

## 풀이

```java
// you can also use imports, for example:
import java.util.*;

// you can write to stdout for debugging purposes, e.g.
// System.out.println("this is a debug message");

class Solution {
    public int solution(int A, int B, int K) {
        // write your code in Java SE 11
        int count = 0;

        // 만약 B가 0면 무엇을 나누든 나머지는 0임
        if(B==0){
            return 1;
        }

        // 만약 K가 B보다 크면 0을 리턴
        if(K>B){
            return count;
        }

        if(A%K == 0 && B%K == 0){
            count++;
        }

        if(A%K == 0 && B%K != 0){
            count++;
        }

        count += B/K - A/K;

        return count;
    }
}
```

## 놓친점

왜이렇게 풀었지...ㅠㅠㅠ 

## 다른풀이

```java
class Solution {
    public int solution(int A, int B, int K) {
        if(A%K==0)
            return B/K - A/K +1;
        return B/K - A/K;
    }
}
```
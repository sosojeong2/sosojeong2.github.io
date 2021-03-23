---
layout: post
title: "Lesson4, PermCheck"
date: 2021-03-23 17:15:23 +0900
category: CodingTest
---

# Lesson4, PermCheck

## 문제

N개의 정수로 구성된 비어있지 않은 배열 A가 제공된다.

    A [0] = 4
    A [1] = 1
    A [2] = 3
    A [3] = 2

순열이지만 배열 A는 다음과 같습니다.

    A [0] = 4
    A [1] = 1
    A [2] = 3

값 2가 없기 때문에 순열이 아닙니다.

목표는 배열 A가 순열인지 확인하는 것입니다.

- N은 [1 ..100,000] 범위 내의 정수 이고;
- 배열 A의 각 요소는 [1..1,000,000,000] 범위 내의 정수 입니다.

배열 A가 주어지면 배열 A가 순열이면 1을 반환하고 그렇지 않으면 0을 반환합니다.

## 풀이

```java
// you can also use imports, for example:
import java.util.*;

// you can write to stdout for debugging purposes, e.g.
// System.out.println("this is a debug message");

class Solution {
    public int solution(int[] A) {
        // write your code in Java SE 11
        Arrays.sort(A); 

        int length = A.length;
        if(A[length-1] != length){
            return 0;
        }
        
        return 1;
    }
}
```

## 놓친점

배열 A에 중복 값이 들어온다고 생각하지 못하였다

그래서 set으로 중복값을 제거하고 다시 풀었다. 

```java
// you can also use imports, for example:
import java.util.*;

// you can write to stdout for debugging purposes, e.g.
// System.out.println("this is a debug message");

class Solution {
    public int solution(int[] A) {
        // write your code in Java SE 11
        Set set = new TreeSet(); 
        
        for (int a : A) {
            set.add(a); 
        }
        if(A.length != set.size()){
            return 0;
        }
        int N  = set.size(); 

        for(int i=1; i<N+1; i++){
            if(!set.contains(i)){
                return 0;
            }
        }
        return 1;
    }
}
```

오래걸린다...ㅠㅠ

## 다른풀이

바본가 보다... 현타온다

```java
// you can also use imports, for example:
import java.util.*;

// you can write to stdout for debugging purposes, e.g.
// System.out.println("this is a debug message");

class Solution {
    public int solution(int[] A) {
        // write your code in Java SE 11
        Arrays.sort(A); // {2}

        for(int i=1; i<A.length+1; i++){
            if(A[i-1] != i){
                return 0;
            }
        }

        return 1;
    }
}
```
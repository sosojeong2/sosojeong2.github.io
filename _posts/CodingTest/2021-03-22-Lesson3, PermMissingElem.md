---
layout: post
title: "Lesson3, PermMissingElem"
date: 2021-03-22 17:15:23 +0900
category: CodingTest
---

# Lesson3, PermMissingElem

## **문제**

N개의 다른 정수로 구성된 배열 A가 제공됩니다. 

배열에는 [1 ... (N+1)] 범위의 정수가 포함되어있고 하나의 요소가 누락되었음을 의미한다.

당신은 누락된 값을 찾아야된다.

ex)  A [0] = 2

 A [1] = 3

 A [2] = 1

 A [3] = 5

함수는 누락된 요소 4를 반환하면 된다.

- N은 [ 0 .. 100,000 ] 범위 내의 정수입니다 .
- A의 요소는 모두 구별됩니다.
- 배열 A의 각 요소는 [1 .. (N + 1)] 범위 내의 정수입니다.

## 풀이

1. 배열을 오름차순으로 정렬한다.
2. 배열 A[i]값과 i값이 다르면 리턴하고 모두 같으면 마지막 값이 빠진 것이므로 A.length+1을 리턴하여준다. 

```java
// you can also use imports, for example:
import java.util.*;

// you can write to stdout for debugging purposes, e.g.
// System.out.println("this is a debug message");

class Solution {
    public int solution(int[] A) {
        // write your code in Java SE 8
        Arrays.sort(A); 
        for(int i=1; i<A.length+1; i++){
            if(A[i-1] != i){
                return i;
            }
        }
        return A.length+1;
    }
}
```

## 놓친점

## **다른풀이**

1. 배열이 주어지고 1~N 중에 빠진 숫자를 구하는것
2. 1~10이면 합이 10*11/2 ⇒ n*(n+1)/2
3. (수식의 합 - 배열의 합)을 구하면 빠진 숫자를 확인할 수 있다.
4. 이 때 입력값이 너무 커서 long 타입으로 치환 후 int로 변경해 주어야 한다. 

```java
private int findMissingNumber3(int[] A) {
    long sum = sumStartOneTo(A.length + 1);
    for (int i = 0; i < A.length; i++) {
        sum -= A[i];
    }
    return (int) sum;
}

private long sumStartOneTo(int n) {
    return ((long) n * (n + 1)) / 2;
}
```
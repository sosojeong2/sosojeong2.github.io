---
layout: post
title: "Lesson4, MaxCounters"
date: 2021-03-22 17:15:23 +0900
category: CodingTest
---

# Lesson4, MaxCounters

## 문제

처음에는 0으로 설정된 N개의 카운터가 제공되며 두가지 기능이 가능한 작업이 있다.

- 증가 (X) -카운터 X가 1 씩 증가합니다.
- 최대 카운터 -모든 카운터는 모든 카운터의 최대 값으로 설정됩니다.

M개의 정수로 구성된 비어 있지 않은 배열 A가 제공됩니다. 이 배열은 연속 작업을 나타낸다.

- A [K] = X, 즉 1 ≤ X ≤ N이면 연산 K는 증가 (X),
- A [K] = N + 1이면 작업 K는 최대 카운터입니다.

예를들어 아래 배열 A와 N=5이면,

A [0] = 3 

(0, 0, 1, 0, 0)

A [1] = 4 

(0, 0, 1, 1, 0)

A [2] = 4

(0, 0, 1, 2, 0)

A [3] = 6

(2, 2, 2, 2, 2)

A [4] = 1

(3, 2, 2, 2, 2)

A [5] = 4

(3, 2, 2, 3, 2)

A [6] = 4

결과 (3, 2, 2, 4, 2) 가 되어야한다.

**(3, 2, 2, 4, 2)**

## 풀이

```java
// you can also use imports, for example:
import java.util.*;

// you can write to stdout for debugging purposes, e.g.
// System.out.println("this is a debug message");

class Solution {
    public int[] solution(int N, int[] A) {
        // write your code in Java SE 11
        int[] result = new int[N];
        int max = 0;
        Arrays.fill(result, 0); // 0으로 초기화

        for (int i=0; i<A.length; i++) {
            if(A[i] < N+1){
                result[A[i]-1]++;
                max = Math.max(max, result[A[i]-1]);
            }else{
                Arrays.fill(result, max);
            }
            
        }

        return result;
    }
}
```

## 놓친점

The following issues have been detected: timeout errors.

타임아웃 에러가 났다...ㅠ

보니깐 large random test, 10000 max_counter operations

불필요한 maxCount계산을 안하게 만들어야된다.

## 다른풀이

Detected time complexity: **O(N + M)**

1. N+1 이 나오기 전까지 tmp 를 이용해 일부분의 최대값을 저장한다.

2. N+1 이 나오면 tmp 값을 max (Counter의 최대값)에 더해주고 tmp를 초기화한다.

3. max 보다 작은 arr 원소가 increase 되면 값을 max + 1 로 바꿔준다. 시간 복잡도 이득을 위해 배열 값들을 모두 max로 바꾸진 않았기 때문

4. for문이 끝난 후 max 보다 작은 값들은 max로 모두 바꿔준다.

```java
// you can also use imports, for example:
import java.util.*;

// you can write to stdout for debugging purposes, e.g.
// System.out.println("this is a debug message");

class Solution {
    public int[] solution(int N, int[] A) {
        // write your code in Java SE 11
        int[] arr = new int[N];

        int idx = 0;
        int tmp = 0;
        int max = 0;
        
        for(int i = 0; i < A.length; i++){
            if(A[i] < N+1){
                idx = A[i]-1;
                
                if(arr[idx] < max){
                    arr[idx] = max;
                }
                
                arr[idx]++;
                
                if(arr[idx] > tmp + max){
                    tmp = arr[idx] - max;
                }
            }
            else{
                max += tmp;
                tmp = 0;
            }
        }
        
        for(int j = 0; j < arr.length; j++){
            if(arr[j] < max){
                arr[j] = max;
            }
        }
        
        return arr;
    }
}
```
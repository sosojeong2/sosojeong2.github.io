---
layout: post
title: "Lesson3, TapeEquilibrium"
date: 2021-03-22 17:15:23 +0900
category: CodingTest
---

# Lesson3, TapeEquilibrium

## 문제

N개의 정수로 구성된 비어있지 않은 배열 A가 제공됨

배열 A는 테이프의 숫자를 나타낸다. 

0 <P <N 인 정수 P는이 테이프를 비어 있지 않은 두 부분으로 분할합니다 : A [0], A [1], ..., A [P − 1] 및 A [P], A [ P + 1], ..., A [N-1].

두 부분 의 차이 는 | (A [0] + A [1] + ... + A [P − 1]) − (A [P] + A [P + 1] + .. . + A [N − 1]) |

즉, 첫 번째 부분의 합과 두 번째 부분의 합 사이의 절대 차이입니다.

예를 들어, 다음과 같은 배열 A를 고려하십시오.

- N은 [2 ..100,000] 범위 내의 정수 이고;
- 배열 A의 각 요소는 [−1,000..1,000] 범위 내의 정수 입니다.

ex)  A [0] = 3
       A [1] = 1
       A [2] = 2
       A [3] = 4
       A [4] = 3

- P = 1, 차이 = | 3 − 10 | = 7
- P = 2, 차이 = | 4 − 9 | = 5
- P = 3, 차이 = | 6 − 7 | = 1
- P = 4, 차이 = | 10 − 3 | = 7

여기서 최소값을 구하면 됨 return 1

## 내가 푼 풀이 - 오답

```java
// you can also use imports, for example:
import java.util.*;

// you can write to stdout for debugging purposes, e.g.
// System.out.println("this is a debug message");

class Solution {
    public int solution(int[] A) {
        // write your code in Java SE 11
        int P ;
        int N = A.length;
        int sum = 0;
        int[] result = new int[N-1];
        
        // 배열 전체 값 더하기
        for(int i =0; i<N; i++){
            sum+=A[i];
        }

        // P는 A.length만큼 돌아야한다. 1,2,3,4
        for(P = 1; P<N; P++){
            int sumFront = 0;
            int sumBack = 0;
            // P보다 작은값 (A [0] + A [1] + ... + A [P - 1])
            for(int j =0; j<P; j++){
                sumFront+=A[j];
            }

            sumBack=sum-sumFront;
            result[P-1] = Math.abs(sumFront-sumBack);
        }
        Arrays.sort(result);
        return result[0];
    }
}
```

## 놓친점

timeout error가 발생하였다. 

복잡도를 생각해야함.

시간 복잡도를 O(n^2)이 나오지 않게 하기 위해서, sum을 구하고 빼나가는 방식으로 구현했다. stream으로 sum을 구하려고 했으나 스트림이 더 성능이 안좋다는 말을 들어서 사용하지 않았다.

## 다른풀이 - 정답

```java
// you can also use imports, for example:
// import java.util.*;

// you can write to stdout for debugging purposes, e.g.
// System.out.println("this is a debug message");

class Solution {
    public int solution(int[] A) {
        int P ;
        int N = A.length;
        int sum = 0;
        int currentSum = 0;
       
        int min = 0;
        // 배열 전체 값 더하기
        for(int i =0; i<N; i++){
            sum+=A[i];
        }

        for(P = 0; P<N-1; P++){
            currentSum += A[P];
            int temp = Math.abs(currentSum-(sum-currentSum));

            // 만약 P가 0이면 
            if(P == 0) min = temp;

            min = Math.min(temp, min);
        }
        return min;
    }
}
```
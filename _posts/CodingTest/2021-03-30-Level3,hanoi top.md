---
layout: post
title: "Level3, hanoi top"
date: 2021-03-20 17:15:23 +0900
category: CodingTest
---

# Level3, hanoi top

## 문제

하노이 탑(Tower of Hanoi)은 퍼즐의 일종입니다. 세 개의 기둥과 이 기동에 꽂을 수 있는 크기가 다양한 원판들이 있고, 퍼즐을 시작하기 전에는 한 기둥에 원판들이 작은 것이 위에 있도록 순서대로 쌓여 있습니다. 게임의 목적은 다음 두 가지 조건을 만족시키면서, 한 기둥에 꽂힌 원판들을 그 순서 그대로 다른 기둥으로 옮겨서 다시 쌓는 것입니다.

1. 한 번에 하나의 원판만 옮길 수 있습니다.
2. 큰 원판이 작은 원판 위에 있어서는 안됩니다.

하노이 탑의 세 개의 기둥을 왼쪽 부터 1번, 2번, 3번이라고 하겠습니다. 1번에는 n개의 원판이 있고 이 n개의 원판을 3번 원판으로 최소 횟수로 옮기려고 합니다.

1번 기둥에 있는 원판의 개수 n이 매개변수로 주어질 때, n개의 원판을 3번 원판으로 최소로 옮기는 방법을 return하는 solution를 완성해주세요.

- n < =15 자연수이다.

**n=2**

**result = [[1,2][1,3][2,3]]**

## 풀이

못풀어... 시댕...

```java
import java.util.*;

class Solution {
        public static int[][] answer;
        public static int x = 0;
        
        // n개를 other기둥을 사용해 from -> to로 옮기기
        public static void hanoi(int n, int from, int other, int to){
            if(n == 0)
                return;
            hanoi(n-1, from, to, other);  // n-1개의 원판을 목적지가 아닌 곳(other)로 옮겨놓음.
            answer[x][0] = from;          // 마지막 원판을 목적지로 옮김.
            answer[x++][1] = to;          // 마지막 원판을 목적지로 옮김.
            hanoi(n-1, other, from, to);  // 목적지가 아닌 곳(other)에 옮겨놓았던 원판들을 목적지로 옮김
        }
            
        public int[][] solution(int n) {
            int num = (int)Math.pow(2, n) - 1;    // 하노이의 이동 횟수: 2^n - 1
            answer = new int[num][2]; // 횟수는 num만큼, 열은 3개(세 개의 기둥)
            hanoi(n, 1, 2, 3);
            return answer;
        }
    }
```

## 놓친점

재귀란 같은 형태의 보다 작은 입력을 지닌 자기 자신을 호출하는 것이고, 이렇게 재귀적인 호출을 사용하는 함수를 재귀함수라고 한다. 

hanoi(N) : N개의 원반을 옮겨라

hanoi(N-1) : (N-1)개의 원반을 옮겨라

단서는 규칙에 따라 3번째 원반을 A에서 C로 옮기려면 위의 두개의 원반은 B원반에 이미 꽂혀 있어야한다. 즉 여기서 hanoi(2) 한번, 

또 위의 2개의 원반을 B에 꽂은 후 3번째 원반을 C로 옮겼다. 이제 2개의 원반을 C로 옮겨야한다. 여기서도 hanoi(2)가 한번

**따라서 hanoi(N)은 두번의 hanoi(N-1) 재귀과정을 수반한다는 것이다.**

## 다른풀이

[https://zzang9ha.tistory.com/194](https://zzang9ha.tistory.com/194)
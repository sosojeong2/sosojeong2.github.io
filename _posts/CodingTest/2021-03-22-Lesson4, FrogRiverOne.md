---
layout: post
title: "Lesson4, FrogRiverOne"
date: 2021-03-22 17:15:23 +0900
category: CodingTest
---

# Lesson4, FrogRiverOne

## 문제

개구리가 강 건너편으로 가고 싶어한다.

개구리는 처음에 위치0 에 있으며 반대둑 위치 X+1에 도달하려고 한다.

잎은 나무에서 강 표면으로 떨어진다.

낙엽을 나타내는 N개의 정수로 구성된 배열 A가 제공된다. 

A[K]는 초 단위로 측정된 시간 K에서 한 잎이 떨어지는 위치를 나타낸다.

목표는 개구리가 강 반대편으로 점프할 수 있는 가장 빠른 시간을 나타내는 것이다.

개구리는 잎이 1에서 X까지 강 건너의 모든 위치에 나타날 때만 건널 수 있다. 

X가 5가 주어졌을때 반대 둑 위치가 X+1이기 때문에 6이고, 6에 도달하기 위해서는 1,2,3,4,5 위치 모두에 잎이 떨어져 있어야 한다. 그러니깐 결국 1~X까지의 순열이 채워지는 가장 빠른 배열의 인덱스 번호를 구하면된다. 

A [0] = 1

A [1] = 3

A [2] = 1

A [3] = 4

A [4] = 2

A [5] = 3

A [6] = 5

A [7] = 4

- N은 [ 0 .. 100,000 ] 범위 내의 정수입니다 .
- 배열 A의 각 요소는 [1 .. X] 범위 내의 정수입니다.

## 풀이

Detected time complexity: **O(N)**

```java
// you can also use imports, for example:
import java.util.*;

// you can write to stdout for debugging purposes, e.g.
// System.out.println("this is a debug message");

class Solution {
    public int solution(int X, int[] A) {
        // write your code in Java SE 11
        HashSet<Integer> set = new HashSet<>(A.length);
        int index = 0;
        for (int i : A) {

            if(i <= X){
                set.add(i);
            }

            if(set.size() == X){
                return index;
            }
            index++;
        }

        return -1;
    }
}
```

## 다른풀이

## 놓친점
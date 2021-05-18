---
layout: post
title: "[programmers] N개의 최소공배수"
subtitle : level 2 문제
tags: [algorithm]
author: Gaeun Kim
comments : True
---

<h2>문제</h2>

[programmers N개의 최소공배수 문제 보러가기](https://programmers.co.kr/learn/courses/30/lessons/12953)

<br><br>

<h2>해설</h2>

최소공배수(gcd)와 최대공약수(lcm)을 적절하게 사용해 볼 수 있는 문제로 추천한다.

<br><br>

<h2>풀이</h2>

```java
import java.util.*;

class Solution {
    public int solution(int[] arr) {
        int answer = 0;
        
        Arrays.sort(arr);
        for(int i = 0; i < arr.length-1; i++){
            int res = gcd(arr[i+1], arr[i]);
            arr[i+1] = (arr[i] * arr[i+1]) / res;
        }
        
        return arr[arr.length-1];
    }
    
    static int gcd(int a, int b){
        while(b != 0){
            int r = a % b;
            a = b;
            b = r;
        }
        
        return a;
    }
}
```


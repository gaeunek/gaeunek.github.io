---
layout: post
title: "[programmers] 입국심사"
subtitle : level 3, 이분탐색 문제
tags: [algorithm]
author: Gaeun Kim
comments : True
---

<h2>문제</h2>

[programmers 입국심사 문제 보러가기](https://programmers.co.kr/learn/courses/30/lessons/43238)

<br><br>

<h2>문제점 해결</h2>

return 타입이 long 이기 때문에 이 부분을 잘 유의해야 한다.

<br><br>

<h2>풀이</h2>

```java
class Solution {
    public long solution(int n, int[] times) {
        long answer = Long.MAX_VALUE;
        
        long st = 0, mid = 0;
        long end = (long)times[times.length - 1] * n;

        while(st != end){
            mid = (st + end) / 2;
            long sum = 0;
            
            for(int time : times) sum += mid / time;
            
            if(sum < n) st = mid + 1;
            else {
                if(answer > mid) answer = mid;
                end = mid - 1;
            }
        }
        
        return answer;
    }
}
```


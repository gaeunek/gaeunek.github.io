---
layout: post
title: "[programmers] 단속카메라"
subtitle : level 3, 탐욕법(Greedy) 문제
tags: [algorithm]
author: Gaeun Kim
comments : True
---

<h2>문제</h2>

[programmers 단속카메라 문제 보러가기](https://programmers.co.kr/learn/courses/30/lessons/42884)

<br><br>

<h2>문제점 해결</h2>

차량의 대수는 1대 이상이기에 카메라의 최소 갯수는 1개이다. 다음 차량의 진입 지점이 현재 차량의 진출지점보다 큰 경우에 카메라를 한 대 추가해주고 현재 차량의 진출지점도 바꿔준다.

```java
int now = routes[0][1];
for(int i = 0; i < routes.length; i++){
    if(now < routes[i][0]) {
        answer++;
        now = routes[i][1];
    }
}
        
return answer;
```

<br><br>

<h2>풀이</h2>

```java
import java.util.*;
class Solution {
    public int solution(int[][] routes) {
        /* 진출 지점을 기준으로 오름차순 정렬 */
        Arrays.sort(routes, new Comparator<int[]>(){
            
            @Override
            public int compare(int[] o1, int[] o2){
                if(o1[1] == o2[1]) return o1[0] - o2[0];
                return o1[1] - o2[1];
            }
        });
        
        /* 차량의 대수는 1대 이상이기에 카메라 갯수를 1개로 지정해둔다. */
        int answer = 1;
        
        /* 현재 진출 지점보다 다음 차량의 진입 지점이 크면 카메라 한 대 추가 */
        int now = routes[0][1];
        for(int i = 0; i < routes.length; i++){
            if(now < routes[i][0]) {
                answer++;
                now = routes[i][1];
            }
        }
        
        return answer;
    }
}
```


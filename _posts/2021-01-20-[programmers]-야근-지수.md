---
layout: post
title: "[programmers] 야근 지수"
subtitle : level 3 문제
tags: [algorithm]
author: Gaeun Kim
comments : True
---

<h2>문제</h2>

[programmers 야근 지수 문제 보러가기](https://programmers.co.kr/learn/courses/30/lessons/12927)

<br><br>

<h2>문제점 해결</h2>

작업량이 가장 많은 순으로 일을 끝내주기 위해 PriorityQueue(pq)를 사용해줬다. 작업량이 [4, 3, 3]이고 4시간 동안 일을 한다면 pq엔 저장된 [4, 3, 3] 중 '4'를 꺼내 한 시간 처리해준 후 다시 pq에 저장한다. 4시간 일을 모두 끝내면 pq = [2, 2, 2]가 된다.

<br><br>

<h2>풀이</h2>

```java
import java.util.*;
class Solution {
	public long solution(int n, int[] works) {
		long answer = 0; 

		PriorityQueue<Integer> pq = new PriorityQueue<>(Collections.reverseOrder());
		
		for (int i = 0; i < works.length; i++) {
			pq.add(works[i]);
		}
		
		while(n > 0) {
			int now = pq.poll();
			pq.add(now-1);
			n--;
		}
		
		while(!pq.isEmpty()) {
			int now = pq.poll();
			if(now <= 0) continue;
			answer += Math.pow(now, 2);
		}
		
        return answer;
    }
}
```


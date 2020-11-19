---
layout: post
title: "[programmers] [1차]캐시"
subtitle : level 2, 2018 KAKAO BLIND RECRUITMENT 문제
tags: [algorithm]
author: Gaeun Kim
comments : True
---

<h2>문제</h2>

[programmers [1차]캐시 문제 보러가기](https://programmers.co.kr/learn/courses/30/lessons/17680)

<br><br>

<h2>문제점 해결</h2>

캐시 교체 알고리즘 LRU에 대한 기본 지식이 요구되는 문제이다.

<br><br>

<h2>풀이</h2>

```java
import java.util.*;
class Solution {
	public int solution(int cacheSize, String[] cities) {
		int answer = 0;
		
		if(cacheSize == 0) return cities.length * 5;
		
		Queue<String> queue = new LinkedList<String>();
		
		String s;
		for (int i = 0; i < cities.length; i++) {
			String city = cities[i].toLowerCase();
			
			if(!queue.contains(city)) {
				if(queue.size() >= cacheSize) queue.poll();
				queue.add(city);
				answer += 5;
			} else {
				List<String> list = new ArrayList<>(queue);
				list.remove(list.indexOf(city));
				queue.clear();
				queue = new LinkedList<String>(list);
				queue.add(city);
				answer++;
			}
		}
		
		return answer;
	}
}
```


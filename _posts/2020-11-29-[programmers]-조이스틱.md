---
layout: post
title: "[programmers] 조이스틱"
subtitle : level 2, 탐욕법(Greedy) 문제
tags: [algorithm]
author: Gaeun Kim
comments : True
---

<h2>문제</h2>

[programmers 조이스틱 문제 보러가기](https://programmers.co.kr/learn/courses/30/lessons/42860?language=java)

<br><br>

<h2>문제점 해결</h2>

한개의 TC가 통과가 안되서 결국 구글링의 도움을 받아 푼 문제이다.

중요한 부분은 최소 이동 횟수 값을 구하는 부분이다. 'A'를 만났을 때 정방향(→)으로 이동하는 것 보다 역방향(←)으로 가는 쪽이 더 적은 횟수로 이동할 수 있는 것인지 확인하는 부분으로 i만큼 이동했고 i만큼 역방향으로 돌아가기 때문에 i * 2를 해준 뒤, 전체 길이에서 A가 연속으로 나오는 index까지의 길이를 빼면 역방향으로 이동할 때 마지막 index부터 총 몇번을 더 움직여야 하는지 계산된다.

```java
if(name.charAt(i) != 'A') {
    answer += Math.min(name.charAt(i) - 'A', ('Z' - name.charAt(i)) + 1);
				
	int next = i + 1;
				
	while(next < len && name.charAt(next) == 'A') next++;
				
	min_move = Math.min(min_move, i * 2 + len - next);
}
```

<br><br>

<h2>풀이</h2>

```java
class Solution {
	public int solution(String name) {
		int answer = 0;
		
		int len = name.length();
		int min_move = len-1;
		
		for (int i = 0; i < len; i++) {
			if(name.charAt(i) != 'A') {
				answer += Math.min(name.charAt(i) - 'A', ('Z' - name.charAt(i)) + 1);
				
				int next = i + 1;
				
				while(next < len-1 && name.charAt(next) == 'A') next++;
				
				min_move = Math.min(min_move, i * 2 + len - next);
			}
		}
		
		return answer + min_move;
	}
}
```


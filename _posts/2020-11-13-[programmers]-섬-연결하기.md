---
layout: post
title: "[programmers] 섬 연결하기"
subtitle : level 3, 탐욕법(Greedy) 문제
tags: [algorithm]
author: Gaeun Kim
comments : True
---

<h2>문제</h2>

[programmers 섬 연결하기 문제 보러가기](https://programmers.co.kr/learn/courses/30/lessons/42861?language=java)

<br><br>

<h2>문제점 해결</h2>

union-find로 해결할 수 있는 문제였으며, 배열 정렬을 꼭 잊으면 안됨을 깨달을 수 있는 문제였다.

#### 1. 실수했던 부분

```java
for (int i = 0; i < costs.length; i++) {
    if(find(costs[i][0]) != find(costs[i][1])) {
		union(costs[i][0], costs[i][1]);
		answer += costs[i][2];
	}
}
```

위의 if문을 보면 find를 사용해 값을 찾는 조건임을 알 수 있는데, 처음엔 이 부분을 parent배열의 값으로 비교를 했고, 당연히 fail의 결과가 나왔다.

실패한 이유는 n = 4이고 costs = {{0, 1, 3}, {1, 2, 2}, {1, 3, 5}, {2, 3, 1}}이란 입력이 들어왔을 때, 정렬 후 costs는 {{2, 3, 1}, {1, 2, 2}, {0, 1, 3}, {1, 3, 5}}이고 {0, 1, 3}까지 끝낸 후 parent배열은 [0, 0, 1, 2]란 값을 가지게 된다. 이 상태에서 {1, 3, 5}를 실행했을 때, **parent배열 값으로 비교하면 if문에 들어가게되지만 find를 사용한다면 결국 parent[3]값의 부모는 0이기에 if문에 들어가지 못한다.** 때문에 비교할 때, parent배열값이 아닌 find로 부모를 찾아 그 값을 비교해 주어야 함을 깨달았다.

<br><br>

<h2>풀이</h2>

```java
import java.util.*;

class Solution {
	static int[] parent;
	public int solution(int n, int[][] costs) {
		int answer = 0;
		
		Arrays.sort(costs, new Comparator<int[]>() {

			@Override
			public int compare(int[] o1, int[] o2) {
				return o1[2] - o2[2];
			}
		});
		
		parent = new int[n];
		
		for (int i = 0; i < n; i++) parent[i] = i; //부모값 본인으로 초기화
		
		for (int i = 0; i < costs.length; i++) {
			if(find(costs[i][0]) != find(costs[i][1])) {
				union(costs[i][0], costs[i][1]);
				answer += costs[i][2];
			}
		}
		
		return answer;
	}
	
	static int find(int n) {
		if(parent[n] == n) return n;
		return parent[n] = find(parent[n]);
	}
	
	static void union(int a, int b) {
		int parentA = find(a);
		int parentB = find(b);
		
		if(parentA != parentB) parent[parentB] = parentA;
	}
}
```


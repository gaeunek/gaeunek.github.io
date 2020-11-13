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


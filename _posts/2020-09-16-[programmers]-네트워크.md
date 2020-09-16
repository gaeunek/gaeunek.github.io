---
layout: post
title: "[programmers] 네트워크"
subtitle : level 3, 깊이/너비 우선 탐색(DFS/BFS) 문제
tags: [algorithm]
author: Gaeun Kim
comments : True
---

<h2>문제</h2>

[programmers 네트워크 문제 보러가기](https://programmers.co.kr/learn/courses/30/lessons/43162)

<br><br>

<h2>풀이</h2>

```java
class Solution {
	static boolean[] visited;

	public int solution(int n, int[][] computers) {
		int answer = 0;

		visited = new boolean[n];

		for (int i = 0; i < n; i++) {
			if(visited[i])
				continue;
			
			for (int j = 0; j < n; j++) {
				if (computers[i][j] == 1 && !visited[j]) {
					visited[i] = true;
					answer += dfs(i, 1, n, computers);
				}
			}
		}
		return n - answer == 0 ? 1 : answer;
	}

	static int dfs(int node, int network, int n, int[][] computers) {
		for (int i = 0; i < n; i++) {
			if (!visited[i] && computers[i][node] == computers[node][i] && computers[i][node] == 1) {
				visited[i] = true;
				dfs(i, network+1, n, computers);
			}
		}
		return network;
	}
}
```


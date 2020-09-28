---
layout: post
title: "[programmers] 가장 먼 노드"
subtitle : level 3, 그래프 문제
tags: [algorithm]
author: Gaeun Kim
comments : True
---

<h2>문제</h2>

[programmers 가장 먼 노드 문제 보러가기](https://programmers.co.kr/learn/courses/30/lessons/49189?language=java)

<br><br>

<h2>풀이</h2>

```java
import java.util.*;

class Solution {
	public int solution(int n, int[][] edge) {
		int answer = 0;

		boolean[][] visit = new boolean[n + 1][n + 1];

		for (int i = 0; i < edge.length; i++) {
			visit[edge[i][0]][edge[i][1]] = true;
			visit[edge[i][1]][edge[i][0]] = true;
		}

		Queue<Integer> queue = new LinkedList<>();
		int[] count = new int[n + 1];
		queue.add(1);

		int max = 0;
		while (!queue.isEmpty()) {
			int now = queue.poll();
			for (int i = 2; i < n + 1; i++) {
				if (count[i] == 0 && visit[now][i]) {
					count[i] = count[now] + 1;
					queue.add(i);
					max = count[i] > max ? count[i] : max;
				}
			}
		}

		for (int i = 2; i < n + 1; i++) {
			if (max == count[i])
				answer++;
		}
		return answer;
	}
}
```


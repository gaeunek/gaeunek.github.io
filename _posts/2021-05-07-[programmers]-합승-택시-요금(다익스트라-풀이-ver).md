---
layout: post
title: "[programmers] 합승 택시 요금(다익스트라 풀이 ver)"
subtitle : 2021 KAKAO BLIND RECRUITMENT 문제
tags: [algorithm]
author: Gaeun Kim
comments : True
---

<h2>문제</h2>

[programmers 합승 택시 요금 문제 보러가기](https://programmers.co.kr/learn/courses/30/lessons/72413)

<br><br>

<h2>이전 풀이(플로이드 와샬)</h2>

[플로이드 와샬 풀이 ver](https://gaeunek.github.io/2021/02/02/programmers-%ED%95%A9%EC%8A%B9-%ED%83%9D%EC%8B%9C-%EC%9A%94%EA%B8%88.html)

<br><br>

<h2>풀이(다익스트라)</h2>

효율성으로는 다익스트라로 푸는 방법이 더 빠르다.

```java
import java.util.Arrays;
import java.util.PriorityQueue;

class Solution {
	public int solution(int n, int s, int a, int b, int[][] fares) {
		int answer = Integer.MAX_VALUE;
		boolean[] visited;
		boolean[][] edge = new boolean[n + 1][n + 1];
		int[][] line = new int[n + 1][n + 1];

		for (int i = 0; i < fares.length; i++) {
			int node1 = fares[i][0];
			int node2 = fares[i][1];
			int value = fares[i][2];

			edge[node1][node2] = true;
			edge[node2][node1] = true;

			line[node1][node2] = value;
			line[node2][node1] = value;
		}

		PriorityQueue<Node> pq = new PriorityQueue<>();
		pq.add(new Node(s, 0));

		int[] start = new int[n + 1];
		Arrays.fill(start, Integer.MAX_VALUE);

		visited = new boolean[n + 1];

		// start
		while (!pq.isEmpty()) {
			Node now = pq.poll();
			int number = now.number;
			int value = now.value;
			
			if(visited[number]) continue;
			
			visited[number] = true;
			start[number] = value;

			for (int i = 1; i <= n; i++) {
				if (edge[number][i] && !visited[i] && i != number) {
					pq.add(new Node(i, value + line[number][i]));
				}
			}
		}

		int[] arrA = new int[n + 1];
		Arrays.fill(arrA, Integer.MAX_VALUE);

		Arrays.fill(visited, false);

		// arrA
		pq.add(new Node(a, 0));
		while (!pq.isEmpty()) {
			Node now = pq.poll();
			int number = now.number;
			int value = now.value;
			
			if(visited[number]) continue;
			
			visited[number] = true;
			arrA[number] = value;

			for (int i = 1; i <= n; i++) {
				if (edge[number][i] && !visited[i] && i != number) {
					pq.add(new Node(i, value + line[number][i]));
				}
			}
		}

		// arrB
		int[] arrB = new int[n + 1];
		Arrays.fill(arrB, Integer.MAX_VALUE);

		Arrays.fill(visited, false);

		pq.add(new Node(b, 0));
		while (!pq.isEmpty()) {
			Node now = pq.poll();
			int number = now.number;
			int value = now.value;
			
			if(visited[number]) continue;
			
			visited[number] = true;
			arrB[number] = value;

			for (int i = 1; i <= n; i++) {
				if (edge[number][i] && !visited[i] && i != number) {
					pq.add(new Node(i, value + line[number][i]));
				}
			}
		}

		for (int i = 1; i <= n; i++) {
			answer = Math.min(start[i] + arrA[i] + arrB[i], answer);
		}
		return answer;
	}

	static class Node implements Comparable<Node> {
		int number, value;

		public Node(int number, int value) {
			this.number = number;
			this.value = value;
		}

		@Override
		public int compareTo(Node o) {
			return this.value - o.value;
		}
	}
}
```


---
layout: post
title: "[programmers] 경주로 건설"
subtitle : level 3, 2020 카카오 인턴십, BFS 문제
tags: [algorithm]
author: Gaeun Kim
comments : True
---

<h2>문제</h2>

[programmers 경주로 건설 문제 보러가기](https://programmers.co.kr/learn/courses/30/lessons/67259)

<br><br>

<h2>풀이</h2>

<% raw %>

```java
import java.util.*;

class Solution {
	static int[][] dir = {{0, 1}, {1, 0}, {0, -1}, {-1, 0}};
	static int N;
	static int[][] arr;
	static int answer;
	
	static boolean check(int i, int j) {
		if(i >= 0 & i < N && j >= 0 && j < N) return true;
		return false;
	}
	
	public int solution(int[][] board) {
		N = board.length;
		arr = new int[N][N];
		answer = Integer.MAX_VALUE;
		
		board[0][0] = 1;
		for (int i = 0; i < N; i++) {
			Arrays.fill(arr[i], Integer.MAX_VALUE);
			continue;
		}
		arr[0][0] = 100;
		bfs(board);
		return answer - 100;
	}
	
	static void bfs(int[][] board) {
		Queue<Point> queue = new LinkedList<>();
		
		queue.add(new Point(0, 0, 0, 100));
		queue.add(new Point(0, 0, 1, 100));
		
		while(!queue.isEmpty()) {
			Point now = queue.poll();
			
			if(now.i == N-1 && now.j == N-1) {
				answer = Math.min(answer, arr[N-1][N-1]);
			};
			
			for (int d = 0; d < 4; d++) {
				int ni = dir[d][0] + now.i;
				int nj = dir[d][1] + now.j;
				
				if(check(ni, nj) && board[ni][nj] == 0) {
					int ncost = now.cost + (now.way != d ? 600 : 100);
					if(arr[ni][nj] >= ncost) {
						arr[ni][nj] = ncost;
						queue.add(new Point(ni, nj, d, ncost));
					}
				}
			}
		}
	}
	
	static class Point {
		int i, j, way, cost;
		
		public Point(int i, int j, int way, int cost) {
			this.i = i;
			this.j = j;
			this.way = way;
			this.cost = cost;
		}
	}
}
```

<% endraw %>
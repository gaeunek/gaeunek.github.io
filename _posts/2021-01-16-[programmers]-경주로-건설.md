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

<h2>문제점 해결</h2>

상,하,좌,우 모든 방향을 탐색하기 때문에 DFS로 풀이했을 때 꽤 오랜시간이 걸리고 복잡한 구현이 될 것이라 생각해 BFS로 구현하였다. 이 문제의 point는 최소한의 값을 찾는 경로이기 때문에 갈 수 있는 모든 방향을 찾으나 지금 가려는 길의 최솟값과 현재의 값을 비교해야 한다는 것이다.

#### 1. 코너인지 어떻게 확인할까?

서로 직각으로 만나는 지점을 코너라고 부르며 코너를 만들 때는 500원이 추가로 들어 600원이 비용이 된다. 나는 이 직각임을 체크하기 위해 Point 객체에 이전 방향의 값을 저장했다. 이전 방향과 값이 다르다면 ([0, 0]의 way값이 오른쪽[0]이고 이동하려는 방향(d)의 값이 아래[1]이라면) 코너로 간주하여 600원을 추가해준다.

```java
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
```

시작지점 [0, 0]을 기준으로 오른쪽과 아래로 이동할 수 있기에 이 경로를 queue에 저장해준 후 queue에서 하나씩 꺼내며 이동할 수 있는 방향이 있다면 경로를 저장해준다. 이때 왔던 방향으로 되돌아가는 처리는 따로 해주지 않았는데, 그 이유는 이동할 방향을 queue에 저장하기 전에 그 칸의 저장된 값보다 현재의 누적값이 적을 때 이동하며 기준이되는 칸의 값은 4방향 각각의 값보다 항상 작으므로 처리할 필요가 없었다. 그리고 여러 경로를 찾고 그 중 가장 적은 비용이 드는 경로를 찾아야하는 문제 특성 상 방문처리를 해줄 수 없기 때문이기도 하다.

<br><br>

<h2>풀이</h2>

```java
import java.util.*;

class Solution {
	static int[][] dir = { {0, 1}, {1, 0}, {0, -1}, {-1, 0} };
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

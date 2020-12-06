---
layout: post
title: "[programmers] [1차]프렌즈4블록"
subtitle : level 2, 2018 KAKAO BLIND RECRUITMENT 문제
tags: [algorithm]
author: Gaeun Kim
comments : True
---

<h2>문제</h2>

[programmers [1차]프렌즈4블록 문제 보러가기](https://programmers.co.kr/learn/courses/30/lessons/17679)

<br><br>

<h2>문제 해결점</h2>

먼저 2X2 형태로 4개가 붙어있는 경우를 찾는다.

```java
static int way(int i, int j) {
	int cnt = 0;
	for (int d = 0; d < 3; d++) {
		int ni = dir[d][0] + i;
		int nj = dir[d][1] + j;

		if(check(ni, nj) && arr[i][j] == arr[ni][nj]) cnt++;
	}
		
	return cnt;
}
```

4개가 붙어있는 경우를 찾으면 방문처리를 해준다.

```java
static void visit(int i, int j) {
	visited[i][j] = true;
	for (int d = 0; d < 3; d++) {
		int ni = dir[d][0] + i;
		int nj = dir[d][1] + j;
			
		visited[ni][nj] = true;
	}
}
```

**true**인 블록 갯수를 세준다.

```java
static int count() {
	int res = 0;
	for (int i = 0; i < M; i++) {
		for (int j = 0; j < N; j++) {
			if(visited[i][j]) res++;
		}
	}
    return res;
}
```

**true**인 블록을 제거 후 떨어져 있는 빈 공간을 채워준다.

```java
if(visited[i][j]) {
    arr[i][j] = '.';
	int end = i;
	while(end >= 0) {
        end--;
		if(end == -1) break; 
		if(!visited[end][j]) {
			arr[i][j] = arr[end][j];
			visited[end][j] = true;
			arr[end][j] = '.';
			break;
		}
	}
}
```

<br><br>

<h2>풀이</h2>

{% raw %}

```java
import java.util.Arrays;

class Solution {
	static int[][] dir = {{0, 1}, {1, 0}, {1, 1}};
	static char[][] arr;
	static boolean[][] visited;
	static int M, N;
	static boolean check(int i, int j) {
		if(i >= 0 && i < M && j >= 0 && j < N) return true;
		else return false;
	}
    
	public int solution(int m, int n, String[] board) {
		int answer = 0;
		M = m;
		N = n;
		arr = new char[m][n];
		visited = new boolean[m][n];
		
		for (int i = 0; i < m; i++) {
			char[] tmp = board[i].toCharArray();
			for (int j = 0; j < n; j++) {
				arr[i][j] = tmp[j];
			}
		}
		
		while(true) {
			for (int i = 0; i < m; i++) {
				for (int j = 0; j < n; j++) {
					if(arr[i][j] != '.' && way(i, j) == 3) visit(i, j);
				}
			}
			
			int front = answer;
			answer += count();
			if(answer == front) break;
			fill();
			for (int i = 0; i < m; i++) {
				Arrays.fill(visited[i], false);
			}
		}
		
		return answer;
	}
	
	/* 제거된 공백 채우기 */
	static void fill() {
		for (int j = 0; j < N; j++) {
			for (int i = M-1; i >= 0; i--) {
				if(visited[i][j]) {
					arr[i][j] = '.';
					int end = i;
					while(end >= 0) {
						end--;
						if(end == -1) break; 
						if(!visited[end][j]) {
							arr[i][j] = arr[end][j];
							visited[end][j] = true;
							arr[end][j] = '.';
							break;
						}
					}
				}
			}
		}
	}
	
	/* 제거된 블록 갯수 세기 */
	static int count() {
		int res = 0;
		for (int i = 0; i < M; i++) {
			for (int j = 0; j < N; j++) {
				if(visited[i][j]) res++;
			}
		}
		
		return res;
	}
	
	/* 4개 블록이 붙어있는지 확인 */
	static int way(int i, int j) {
		int cnt = 0;
		for (int d = 0; d < 3; d++) {
			int ni = dir[d][0] + i;
			int nj = dir[d][1] + j;
			
			if(check(ni, nj) && arr[i][j] == arr[ni][nj]) cnt++;
		}
		
		return cnt;
	}
	
	/* 방문처리해주기 */
	static void visit(int i, int j) {
		visited[i][j] = true;
		for (int d = 0; d < 3; d++) {
			int ni = dir[d][0] + i;
			int nj = dir[d][1] + j;
			
			visited[ni][nj] = true;
		}
	}
}
```


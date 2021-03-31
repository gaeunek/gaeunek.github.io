---
layout: post
title: "[programmers] 리틀 프렌즈 사천성"
subtitle : level 3, 2017 카카오코드 본선 문제
tags: [algorithm]
author: Gaeun Kim
comments : True
---

<h2>문제</h2>

[programmers 리플 프렌즈 사천성 문제 보러가기](https://programmers.co.kr/learn/courses/30/lessons/1836)

<br><br>

<h2>문제점 해결</h2>

처음 풀이했던 방법은 permutation을 사용해 가능한 순열을 모두 set에 저장한 뒤, set에서 하나씩 꺼내며 타일을 지우는 것이 가능한지 확인했다. "ADBC"의 경우를 예로 들어보면 "A"타일을 지운 뒤 "D"타일을 지울 수 없으면 바로 호출한 함수를 끝내며 시간을 줄이려 했다. 하지만 여전히 시간초과는 발생했고 알고리즘 설계부터 잘못되었다고 생각해 초기화 후 다시 풀이했다.

<br>

#### 1. 타일의 정렬

문제에 나와있듯이 모든 가능한 경우(정답) 중 알파벳 순으로 가장 먼저인 경우가 정답이기 때문에 board에 저장되어 있는 타일의 종류를 모두 ArrayList에 저장 후 오름차순 정렬하였다.

<br>

#### 2. DFS

정렬한 상태에서 tile(ArrayList)에 저장된 타일을 하나씩 꺼낸다. 이때 alpha 리스트에 저장된 타일의 위치 두개를 각각 시작점과 도착점으로 지정하여 dfs 함수를 호출한다. dfs함수에선 시작(st)지점부터 시작해 도착(end)지점 까지 가는 방법 중 직선과 한번 꺾인 경로로만 이동할 수 있는지를 확인한다. 도착점까지 무사히 간다면 map의 도착지점 값을 '.' 으로 바꿔 타일을 제거했음을 표시한다.

```java
static void dfs(boolean[][] visited, Point st, Point end, int k, boolean coner) {
    if(st.i == end.i && st.j == end.j) {
        map[end.i][end.j] = '.';
        return;
    }

    visited[st.i][st.j] = true; 

    for (int d = 0; d < 4; d++) {
        int ni = st.i + dir[d][0];
        int nj = st.j + dir[d][1];

        if(check(ni, nj) && !visited[ni][nj] && (map[ni][nj] == '.' || map[ni][nj] == map[end.i][end.j])) {
            if(k != -1 && k != d && coner) continue;

            boolean newConer = (k != -1 && k != d) || coner ? true : false;
            dfs(visited, new Point(ni, nj), end, d, newConer);
        }
    }

    visited[st.i][st.j] = false;
}
```

타일이 제거되었다면 다음 타일의 위치를 저장해 dfs를 호출하고, 제거되지 않았다면 제거 대상임을 저장하는 used 배열(boolean)의 해당 인덱스값을 false로 바꾸어 제거 대상에 다시 올려둔다.

```java
if(map[end.i][end.j] == '.') {
    map[st.i][st.j] = '.';
    find(used, result += tile.get(i));
} else used[i] = false;
```

전체적으로 시뮬레이션 느낌의 문제였으나 초반 잘못된 설계로 인해 시간이 좀 걸렸던 문제였다. 하지만 이후 비슷한 유형 문제풀이의 기초 공부가 되었다고 생각한다.

<br><br>

<h2>풀이</h2>

```java
import java.util.*;
class Solution {
	static char[][] map;
	static int[][] dir = { { 0, 1 }, { 1, 0 }, { 0, -1 }, { -1, 0 } };
	static List<Character> tile;
	static Point[][] alpha;
	static int N, M;
	static String answer = "";

	static boolean check(int i, int j) {
		if (i >= 0 && i < M && j >= 0 && j < N)
			return true;
		return false;
	}

	public String solution(int m, int n, String[] board) {
		M = m;
		N = n;
		map = new char[m][n];
		tile = new ArrayList<>();
		alpha = new Point[26][2];

		for (int i = 0; i < m; i++) {
			map[i] = board[i].toCharArray();

			for (int j = 0; j < n; j++) {
				if (map[i][j] == '.' || map[i][j] == '*')
					continue;

				if (!tile.contains(map[i][j])) {
					tile.add(map[i][j]);
					alpha[map[i][j] - 'A'][0] = new Point(i, j);
				} else
					alpha[map[i][j] - 'A'][1] = new Point(i, j);
			}
		}

		Collections.sort(tile);
		answer = "";
		find(new boolean[tile.size()], "");
		return answer == "" ? "IMPOSSIBLE" : answer;
	}

	static void find(boolean[] used, String result) {
		if (result.length() == used.length) {
			answer = result;
			return;
		}

		for (int i = 0; i < used.length; i++) {
			if (!used[i]) {
				used[i] = true;
				Point st = alpha[tile.get(i) - 'A'][0];
				Point end = alpha[tile.get(i) - 'A'][1];

				dfs(new boolean[M][N], st, end, -1, false);

				if (map[end.i][end.j] == '.') {
					map[st.i][st.j] = '.';
					find(used, result += tile.get(i));
				} else
					used[i] = false;
			}
		}
	}

	static void dfs(boolean[][] visited, Point st, Point end, int k, boolean coner) {
		if (st.i == end.i && st.j == end.j) {
			map[end.i][end.j] = '.';
			return;
		}

		visited[st.i][st.j] = true;

		for (int d = 0; d < 4; d++) {
			int ni = st.i + dir[d][0];
			int nj = st.j + dir[d][1];

			if (check(ni, nj) && !visited[ni][nj] && (map[ni][nj] == '.' || map[ni][nj] == map[end.i][end.j])) {
				if (k != -1 && k != d && coner)
					continue;

				boolean newConer = (k != -1 && k != d) || coner ? true : false;
				dfs(visited, new Point(ni, nj), end, d, newConer);
			}
		}

		visited[st.i][st.j] = false;
	}

	static class Point {
		int i, j;

		public Point(int i, int j) {
			this.i = i;
			this.j = j;
		}
	}
}
```


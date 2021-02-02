---
layout: post
title: "[programmers] 합승 택시 요금"
subtitle : level 3, 2021 KAKAO BLIND RECRUITMENT 문제
tags: [algorithm]
author: Gaeun Kim
comments : True
---

<h2>문제</h2>

[programmers 합승 택시 요금 문제 보러가기](https://programmers.co.kr/learn/courses/30/lessons/72413)

<br><br>

<h2>문제점 해결</h2>

플로이드 와샬 알고리즘을 사용해 1부터 n까지 모든 정점을 방문할 때의 최솟값을 2차원 배열 costs에 저장한다. 이때 문제 설명에 나와있는 것처럼 **이동 방향에 따라 달라지지 않기**때문에 costs\[i]\[j]에 값을 저장할 때, costs\[j]\[i]에도 같이 저장해준다.

```java
for (int k = 1; k <= n; k++) {
    for (int i = 1; i <= n; i++) {
		for (int j = 1; j <= n; j++) {
			if (i == j)
				continue;
            
			if (used[i][k] && used[k][j]) {
			if (costs[i][j] == 0)
					costs[i][j] = costs[i][k] + costs[k][j];
				else {
					costs[i][j] = Math.min(costs[i][j], costs[i][k] + costs[k][j]);
				}
				costs[j][i] = costs[i][j];
				used[i][j] = used[j][i] = true;
			}
		}
	}
}
```

#### 7줄로 인한 실패

처음엔 n+1크기의 1차원 배열 result를 만들어 result[x] 에 costs\[x][a] + costs\[x][b] (x에서 a와 b정점으로의 거리)를 저장한 뒤, 최솟값 min을 저장했고, 1~n까지의 for문을 돌려 min에 해당되는 값을 가진 result[x]에 costs\[x][s]를 저장해줬는데 100점만점에 63점으로 실패하였다. 아무리 a정점과 b정점으로 가는 최솟값이라도 s정점에서 x정점으로 가는 값이 더 작아 총 비용이 더 적어질 수 있기 때문에 최솟값으로 필터링 하는 부분을 지우니 바로 통과되었다.

<br>

#### 실패 코드

```java
int min = Integer.MAX_VALUE;
int[] result = new int[n+1];
for (int i = 1; i <= n; i++) {
	result[i] = costs[i][a] + costs[i][b];
	if(result[i] == 0) continue;
	min = Math.min(min, result[i]);
}

for (int i = 1; i <= n; i++) {
	int sum = 0;
	if (result[i] == min) {
		sum += costs[s][i] + result[i];
		answer = Math.min(answer, sum);
	}
}
```

#### 통과 코드

```java
int min = 0;
for (int i = 1; i <= n; i++) {
	min = costs[i][a] + costs[i][b] + costs[s][i];
	if(min == 0) continue;
	answer = Math.min(min, answer);
}
```

<br><br>

<h2>풀이</h2>

```java
class Solution {
	public int solution(int n, int s, int a, int b, int[][] fares) {
		int answer = Integer.MAX_VALUE;

		int[][] costs = new int[n + 1][n + 1];
		boolean[][] used = new boolean[n + 1][n + 1];

		for (int i = 0; i < fares.length; i++) {
			costs[fares[i][0]][fares[i][1]] = fares[i][2];
			costs[fares[i][1]][fares[i][0]] = fares[i][2];
			used[fares[i][0]][fares[i][1]] = used[fares[i][1]][fares[i][0]] = true;
		}

		for (int k = 1; k <= n; k++) {
			for (int i = 1; i <= n; i++) {
				for (int j = 1; j <= n; j++) {
					if (i == j)
						continue;

					if (used[i][k] && used[k][j]) {
						if (costs[i][j] == 0)
							costs[i][j] = costs[i][k] + costs[k][j];
						else {
							costs[i][j] = Math.min(costs[i][j], costs[i][k] + costs[k][j]);
						}
						costs[j][i] = costs[i][j];
						used[i][j] = used[j][i] = true;
					}
				}
			}
		}
		
		int min = 0;
		for (int i = 1; i <= n; i++) {
			min = costs[i][a] + costs[i][b] + costs[s][i];
			if(min == 0) continue;
			answer = Math.min(min, answer);
		}

		return answer;
	}
}
```


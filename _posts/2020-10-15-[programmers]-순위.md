---
layout: post
title: "[programmers] 순위"
subtitle : level 3, 그래프 문제
tags: [algorithm]
author: Gaeun Kim
comments : True
---

<h2>문제</h2>

[programmers 순위 문제 보러가기](https://programmers.co.kr/learn/courses/30/lessons/49191?language=java)

<br><br>

<h2>문제점 해결</h2>

| n    | results                                  | return |
| ---- | ---------------------------------------- | ------ |
| 5    | [[4, 3], [4, 2], [3, 2], [1, 2], [2, 5]] | 2      |

위의 입출력을 예로 들자면, 최초 접근 방식은 ArrayList배열 두 가지(win, lose)를 생성하여 각 승패를 추가해 ArrayList의 크기가 n-1이면 answer를 증가시키는 방법이었다.

2가 5를 이기면서 5에 2가 패한 숫자들 1,3,4가 add되어 5는 총 1,2,3,4 4명에게 패한 결과가 나오며 answer값을 1 증가시키게 된다. 그러나 테스트케이스 10개 중 7개를 통과하지 못하였고, 그 원인은 숫자가 이겼을 경우에 대한 처리를 안했기 때문이라 생각된다.

결국 문제를 해결한 방법은 **플로이드 와샬 알고리즘**이다.

<br>

#### 1. 플로이드 와샬 알고리즘

플로이드 와샬 알고리즘은 모든 정점에서 모든 정점까지의 최단 거리를 구하는 알고리즘이다.

플로이드 와샬은 다이나믹 프로그래밍 기반으로 사용시 가장 중요한 부분은 3개 for문의 사용방법이다. 제일 **바깥쪽** 반복문은 **거쳐가는 정점**이고, **두 번째**는 **출발**, **마지막**은 **도착** 정점이다.

```java
for (int i = 1; i <= n; i++) {
    boolean flag = true;
    for (int j = 1; j <= n; j++) {
        if (i != j && !(game[i][j] || game[j][i])) {
            flag = false;
            break;
        }
    }
   
	if (flag)
        answer++;
}
```

위의 코드 내부 for문에 `!(game[i][j] || game[j][i])`부분은 승 혹은 패의 결과가 없다면 바로 for문을 끝내버려 시간을 단축시킬 수 있는 부분이다. game배열엔 승리한 결과 true값만 저장되어있기 때문에 패배한 결과는 알 수 없다. 만약 A가 B에게 승리한 값 [i,j]가 true라면 반대로 B는 A에게 패배했기 때문에 [j,i] 또한 값이 존재한다 여겨지며, 즉 한 쪽에만 결과값이 들어있어도 반대쪽 결과값도 누적될 수 있도록 하여 결과값이 존재하지 않으면 flag를 걸어 answer에 누적시키지 않도록 코딩하였다.

<br><br>

<h2>풀이</h2>

```java
class Solution {
	public int solution(int n, int[][] results) {
		int answer = 0;

		boolean[][] game = new boolean[n + 1][n + 1];

		for (int i = 0; i < results.length; i++) {
			game[results[i][0]][results[i][1]] = true;
		}

		for (int k = 1; k <= n; k++) {
			for (int i = 1; i <= n; i++) {
				for (int j = 1; j <= n; j++) {
					if (i != j && game[i][k] && game[k][j])
						game[i][j] = true;
				}
			}
		}

		for (int i = 1; i <= n; i++) {
			boolean pass = true;
			for (int j = 1; j <= n; j++) {
				if (i != j && !(game[i][j] || game[j][i])) {
					pass = false;
					break;
				}
			}

			if (pass)
				answer++;
		}
		return answer;
	}
}
```


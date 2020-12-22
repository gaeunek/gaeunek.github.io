---
layout: post
title: "[programmers] 기둥과 보 설치"
subtitle : level 3, 2018 KAKAO BLIND RECRUITMENT 문제
tags: [algorithm]
author: Gaeun Kim
comments : True
---

<h2>문제</h2>

[programmers 기둥과 보 설치 문제 보러가기](https://programmers.co.kr/learn/courses/30/lessons/60061)

<br><br>

<h2>문제점 해결</h2>

시간이 오래 걸린 문제였는데 복잡하게 생각하지 않고 단순하게 생각하면 풀 수 있는 문제였다.

#### 1. 기둥과 보의 분리

구조물의 상태를 return 하기 위해 기둥과 보를 두개의 2차원 배열로 분리하여 설치 및 제거한다.

<br>

#### 2. 제거 후 전체 검토

구조물을 제거할 수 있는지에 대한 검사를 하는것이 번거롭기 때문에 제거 후 전체 검토를 해준다. 검토 결과 조건에 부합하지 않는 기둥이나 보가 존재하면 제거한 구조물을 다시 설치한다.

#### 3. 2차원 배열의 활용

아래 그림은 기둥 설치·제거를 처리하는 배열이다. 한칸을 1 × 1라 하면 행(row)을 기준으로 기둥은 선분에 설치해준다. 기둥하나를 설치하면 열(col)을 기준으로 한칸을 차지하게 된다.

![pillar.jpg](/assets/img/20201223_60061_pillar.jpg)

아래 그림은 보 설치·제거를 처리하는 배열로 기둥 설치와는 다르게 보 설치 시 행(row)의 한 칸을 차지하되, 열(col)은 선분에 설치된다고 생각하면 된다.

![pillar.jpg](/assets/img/20201223_60061_beams.jpg)

즉, 문제에 나와있는 그림 그대로 배열로 옮긴다고 생각하면 된다!

<br><br>

<h2>풀이</h2>

```java
import java.util.*;

class Solution {
	static boolean[][] pillar, beams;
	static int N;
	public int[][] solution(int n, int[][] build_frame) {
		N = n;
		pillar = new boolean[n+1][n+1];
		beams = new boolean[n+1][n+1];
		
		for (int i = 0; i < build_frame.length; i++) {
			int x = n-build_frame[i][1]; //행
			int y = build_frame[i][0]; //열
			int structure = build_frame[i][2];
			int build = build_frame[i][3];
			
			if(structure == 0 && build == 1 && isPillar(x, y)) pillar[x][y] = true;
			else if(structure == 0 && build == 0) {
				pillar[x][y] = false;
				if(!check()) pillar[x][y] = true;
			}
			
			if(structure == 1 && build == 1 && isBeams(x, y)) beams[x][y] = true;
			else if(structure == 1 && build == 0) {
				beams[x][y] = false;
				if(!check()) beams[x][y] = true;
			}
		}

		List<int[]> result = new ArrayList<int[]>();
		for (int i = N; i >= 0; i--) {
			for (int j = 0; j <= N; j++) {
				if(pillar[i][j]) result.add(new int[] {j, N-i, 0});
				
				if(beams[i][j]) result.add(new int[] {j, N-i, 1});
			}
		}
		
		int[][] answer = new int[result.size()][3];
		for (int i = 0; i < answer.length; i++) {
			answer[i] = result.get(i);
		}
		
		Arrays.sort(answer, new Comparator<int[]>() {

			@Override
			public int compare(int[] o1, int[] o2) {
				if(o1[0] == o2[0]) {
					if(o1[1] == o2[1]) return o1[2] - o2[2];
					return o1[1] - o2[1];
				}
				return o1[0] - o2[0];
			}
		});
		
		return answer;
	}
	
	static boolean check() {
		for (int i = N; i >= 0; i--) {
			for (int j = 0; j <= N; j++) {
				if(pillar[i][j] && !isPillar(i, j)) return false;
			}
		}
		
		for (int i = N; i >= 0; i--) {
			for (int j = 0; j <= N; j++) {
				if(beams[i][j] && !isBeams(i, j)) return false;
			}
		}
		
		return true;
	}
	
	static boolean isPillar(int x, int y) {
		if(x == N || beams[x][y] || (y-1 >= 0 && beams[x][y-1]) || pillar[x+1][y]) return true;
		return false;
	}
	
	static boolean isBeams(int x, int y) {
		if(pillar[x+1][y] || pillar[x+1][y+1] || (y-1 >= 0 && y+1 <= N && beams[x][y-1] && beams[x][y+1])) return true;
		return false;
	}
}
```


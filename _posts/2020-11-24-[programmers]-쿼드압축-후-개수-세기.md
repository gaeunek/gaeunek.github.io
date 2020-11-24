---
layout: post
title: "[programmers] 쿼드압축 후 개수 세기"
subtitle : level 2, 월간 코드 챌린지 시즌1 문제
tags: [algorithm]
author: Gaeun Kim
comments : True
---

<h2>문제</h2>

[programmers 쿼드압축 후 개수 세기 문제 보러가기](https://programmers.co.kr/learn/courses/30/lessons/68936)

<br><br>

<h2>문제점 해결</h2>

재귀함수를 돌려 2차원 배열 arr을 분할시켜 풀 수 있는 문제였다. 재귀 함수를 돌리는 만큼 분할 시키는 arr배열의 범위를 정하는 부분이 가장 중요했다.

<br><br>

<h2>풀이</h2>

```java
class Solution {
	static int[] answer;
	public int[] solution(int[][] arr) {
		answer = new int[2];
		division(arr, 0, 0, arr.length);
		return answer;
	}
	
	static void division(int[][] arr, int row, int col, int mid) {
		if(mid == 0) return;
		int zero = 0, one = 0;
		for (int i = row; i < row+mid; i++) {
			for (int j = col; j < col+mid; j++) {
				if(arr[i][j] == 1) one++;
				else zero++;
			}
		}
		
		int area = mid * mid;
		if(one == area) answer[1]++;
		else if(zero == area) answer[0]++;
		else {
			mid /= 2;
			division(arr, row, col, mid);
			division(arr, row, col+mid, mid);
			division(arr, row+mid, col, mid); 
			division(arr, row+mid, col+mid, mid);
		}
	}
}
```


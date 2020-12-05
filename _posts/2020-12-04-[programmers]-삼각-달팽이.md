---
layout: post
title: "[programmers] 삼각 달팽이"
subtitle : level 2, 월간 코드 챌린지 시즌1 문제
tags: [algorithm]
author: Gaeun Kim
comments : True
---

<h2>문제</h2>

[programmers 삼각 달팽이 문제 보러가기](https://programmers.co.kr/learn/courses/30/lessons/68645?language=java)

<br><br>

<h2>문제점 해결</h2>

먼저, 2차원 배열을 만들어 아래와 같이 배열을 왼쪽으로 밀어준다.

```java
[1]
[2, 9]
[3, 10, 8]
[4, 5, 6, 7]
```

다음은 문제 이름처럼 달팽이를 그리듯 ↓, →, ↖ 이 세가지 방향을 반복시켜준다.

<br><br>

<h2>풀이</h2>

```java
import java.util.*;
class Solution {
	public int[] solution(int n) {
		int[][] arr = new int[n][];

		int sum = 0;
		for (int i = 0; i < arr.length; i++) {
			arr[i] = new int[i + 1];
			sum += i + 1;
		}
		
		int[] answer = new int[sum];

		int value = 1;
		int i = 0, j = 0;
		arr[i][j] = value;
		while (value < sum) {
			while(i < n-1 && arr[i+1][j] == 0) {
				arr[++i][j] = ++value;
			}

			while(j < n-1 && arr[i][j+1] == 0) {
				arr[i][++j] = ++value;
			}
			
			while(i > 0 && j > 0 && arr[i-1][j-1] == 0) {
				arr[--i][--j] = ++value;
			}
		}
		
		int index = 0;
		for (i = 0; i < arr.length; i++) {
			for (j = 0; j < arr[i].length; j++) {
				answer[index++] = arr[i][j]; 
			}
		}

		return answer;
	}
}
```


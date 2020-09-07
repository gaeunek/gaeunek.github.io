---
layout: post
title: "[programmers] 타겟 넘버"
subtitle : level 2, 깊이/너비 우선 탐색(DFS/BFS) 문제
tags: [algorithm]
author: Gaeun Kim
comments : True
---

<h2>문제</h2>

[programmers 타겟 넘버 문제 보러가기](https://programmers.co.kr/learn/courses/30/lessons/43165)

<br><br>

<h2>풀이</h2>

```java
class Solution {
	static boolean[] used;
	static int[] nums;
	static int answer;

	public int solution(int[] numbers, int target) {
		answer = 0;
		nums = numbers;
		used = new boolean[numbers.length];

		dfs(0, 0, target);

		return answer;
	}

	static void dfs(int idx, int sum, int target) {
		if (idx == nums.length) {
			if (sum == target)
				answer++;

			return;
		}

		dfs(idx + 1, sum + nums[idx], target);
		dfs(idx + 1, sum - nums[idx], target);
	}
}
```

<br><br>

<h2>문제점 해결</h2>

주어진 숫자들(numbers)를 사용해 target을 만드는 방법의 수를 구하는 문제로 DFS를 공부할 수 있는 기본적인 문제였다고 생각한다.
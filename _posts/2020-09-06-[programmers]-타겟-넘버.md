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




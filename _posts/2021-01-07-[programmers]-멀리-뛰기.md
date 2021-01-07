---
layout: post
title: "[programmers] 멀리 뛰기"
subtitle : level 3 문제
tags: [algorithm]
author: Gaeun Kim
comments : True
---

<h2>문제</h2>

[programmers 멀리 뛰기 문제 보러가기](https://programmers.co.kr/learn/courses/30/lessons/12914?language=java)

<br><br>

<h2>문제점 해결</h2>

먼저 n이 1일때, 방법은 1가지이며, n이 2일때는 `(1칸, 1칸), (2칸)` 2가지이다. 마찬가지로 n이 3일때는 `(1칸, 1칸, 1칸), (1칸, 2칸), (2칸, 1칸)`으로 3가지이다. n이 4일때는 예제에 나와있듯이 5가지이다.

n=1, 1가지

n=2, 2가지

n=3, 3가지

n=4, 4가지

피보나치 수열임을 발견할 수 있다. memorization을 사용해 시간초과를 없앴다.

<br><br>

<h2>풀이</h2>

```java
class Solution {
	static long[] memo;
	public long solution(int n) {
		memo = new long[n+1];
		memo[0] = 1;
		return func(n) % 1234567;
	}
	
	static long func(int n) {
		if(n <= 1) return n;
		memo[n-1] = func(n-1);
		return memo[n-1] % 1234567 + memo[n-2] % 1234567;
	}
}
```


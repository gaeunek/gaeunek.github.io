---
layout: post
title: "[programmers] 주식가격"
subtitle : level 2, 스택/큐 문제
tags: [algorithm]
author: Gaeun Kim
comments : True
---

<h2>문제</h2>

[programmers 주식가격 문제 보러가기](https://programmers.co.kr/learn/courses/30/lessons/42584)

<br><br>

<h2>풀이</h2>

```java
class Solution {
	public int[] solution(int[] prices) {
		int[] answer = new int[prices.length];

		for (int i = 0; i < prices.length; i++) {
			int price = prices[i];
			for (int j = i + 1; j < prices.length; j++) {
				if (price > prices[j] || j == answer.length - 1) {
					answer[i] = j-i;
					break;
				}
			}
		}
		return answer;
	}
}
```

<br><br>

<h2>문제점 해결</h2>

스택/큐를 사용해서 푸는 문제라고 생각했지만 이중 for문으로 해결 할 수 있는 문제였다.
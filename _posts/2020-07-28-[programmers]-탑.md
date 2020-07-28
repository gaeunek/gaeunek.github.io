---
layout: post
title: "[programmers] 탑"
subtitle : level 2, 스택/큐문제
tags: [algorithm]
author: Gaeun Kim
comments : True
---

<h2>문제</h2>

[programmers 탑 문제 보러가기](https://programmers.co.kr/learn/courses/30/lessons/42588)

<br><br>

<h2>풀이</h2>

```java
import java.util.*;

public class Solution {
	public int[] solution(int[] heights) {
		int[] answer = new int[heights.length];

		Stack<Height> stack = new Stack<>();
		Stack<Height> hold = new Stack<>();

		for (int i = 0; i < answer.length; i++) {
			stack.push(new Height(heights[i], i));
		}

		while (!stack.isEmpty()) {
			Height now = stack.pop();

			if (stack.isEmpty()) {
				answer[now.idx] = 0;
				break;
			}

			if (stack.peek().h > now.h) {
				int size = hold.size();
				for (int s = 0; s < size; s++) {
					Height hold_now = hold.pop();

					if (stack.peek().h > hold_now.h) {
						answer[hold_now.idx] = stack.size();
					} else if (stack.peek().h < hold_now.h) {
						answer[hold_now.idx] = 0;
					} else {
						hold.add(hold_now);
					}
				}
				answer[now.idx] = stack.size();
			} else {
				hold.push(now);
			}
		}

		return answer;
	}

	static class Height {
		int h, idx;

		public Height(int h, int idx) {
			this.h = h;
			this.idx = idx;
		}
	}
}
```

<br><br>
---
layout: post
title: "[programmers] 기능개발"
subtitle : level 2, 스택/큐문제
tags: [algorithm]
author: Gaeun Kim
comments : True
---

<h2>문제</h2>

[programmers 기능개발 문제 보러가기](https://programmers.co.kr/learn/courses/30/lessons/42586)

<br><br>

<h2>풀이</h2>

```java
import java.util.*;

public class Solution {
	public int[] solution(int[] progresses, int[] speeds) {
		Stack<Task> stack = new Stack<>();

		for (int i = 0; i < progresses.length; i++) {
			int progresse = progresses[i];
			int speed = speeds[i];
			int cnt = 0;

			while (progresse < 100) {
				cnt++;
				progresse += speed;
			}

			if (stack.isEmpty() || stack.peek().cnt < cnt) {
				stack.push(new Task(cnt, 1));
			} else {
				Task now = stack.pop();
				now.day++;
				stack.push(now);
			}
		}
		int[] answer = new int[stack.size()];
		for (int i = answer.length - 1; i >= 0; i--) {
			answer[i] = stack.pop().day;
		}
		return answer;
	}

	static class Task {
		int cnt, day;

		public Task(int cnt, int day) {
			this.cnt = cnt;
			this.day = day;
		}
	}
}
```

<br><br>
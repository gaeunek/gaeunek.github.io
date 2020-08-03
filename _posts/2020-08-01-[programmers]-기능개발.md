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
import java.util.Stack;

public class Solution {
	public int[] solution(int[] progresses, int[] speeds) {
		Stack<Task> stack = new Stack<>();

		for (int i = 0; i < progresses.length; i++) {
			int progresse = progresses[i];
			int speed = speeds[i];
			int day = 0;

			while (progresse < 100) {
				day++;
				progresse += speed;
			}

			if (stack.isEmpty() || stack.peek().day < day) {
				stack.push(new Task(day, 1));
			} else {
				Task now = stack.pop();
				now.cnt++;
				stack.push(now);
			}
		}
		int[] answer = new int[stack.size()];
		for (int i = answer.length - 1; i >= 0; i--) {
			answer[i] = stack.pop().cnt;
		}
		return answer;
	}

	static class Task {
		int cnt, day;

		public Task(int day, int cnt) {
			this.cnt = cnt;
			this.day = day;
		}
	}
}
```

<br><br>

<h2>문제점 해결</h2>

먼저 100%까지 만들기 위해 `while`문을 돌린다.

```java
if (stack.isEmpty() || stack.peek().day < day) {
    stack.push(new Task(day, 1));
} else {
    Task now = stack.pop();
	now.cnt++;
	stack.push(now);
}
```

`stack`이 비어있거나 `stack.peek()`보다 개발 일수가 오래 걸린다면 `stack`에 `day(개발 일수), cnt(기능 수)`객체를 만들어 `push`해준다.

만약 현재의 기능을 개발하는데 걸리는 시간보다 이미 `stack`에 저장되어 있는 바로 앞의 기능 개발 일수가 더 길다면 `cnt`를 증가시켜준다.
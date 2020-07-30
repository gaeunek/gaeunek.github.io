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
		Height[] arr = new Height[heights.length];

		Stack<Height> stack = new Stack<>();

		for (int i = 0; i < answer.length; i++) {
			arr[i] = new Height(heights[i], i);
		}

		for (int i = arr.length - 1; i >= 0; i--) {
			if (!stack.isEmpty() && arr[i].h > stack.peek().h) {
				int size = stack.size();

				for (int s = 0; s < size; s++) {
					Height now = stack.pop();

					if (arr[i].h > now.h) {
						answer[now.idx] = arr[i].idx + 1;
					} else {
						stack.add(now);
					}
				}
			}
			stack.push(arr[i]);
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

<h2>문제점 해결</h2>

`Height`라는 Class를 만든 이유

`{ 5, 3, 1, 2, 3 }`와 같은 입력이 들어왔을 때, `stack`에 `{3, 2, 1}`이 쌓이게 되고,  1번 인덱스의 `3`과 비교해서 `{2, 1}`로 부터 수신이 가능하기 때문에 answer배열에 저장해야하는데 이때, 저장해야하는 값들의 인덱스를 알아야하기 때문에 `Height` Class를 만들었다.
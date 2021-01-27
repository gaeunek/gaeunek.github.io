---
layout: post
title: "[programmers] 하노이의 탑"
subtitle : level 3 문제
tags: [algorithm]
author: Gaeun Kim
comments : True
---

<h2>문제</h2>

[programmers 하노이의 탑 문제 보러가기](https://programmers.co.kr/learn/courses/30/lessons/12946)

<br><br>

<h2>문제점 해결</h2>

해당 문제는 재귀를 사용한 문제로, 탑이 2, 3, 4개 일때의 경로를 직접 써본 후 규칙을 찾아 코딩해봤지만 시간초과가 발생했다. 도저히 풀이방법이 떠오르지 않아서 다른 분의 블로그에 올라온 풀이 방법을 참고해 풀 수 있었다.

[참고 블로그](https://velog.io/@hyeon930/%ED%94%84%EB%A1%9C%EA%B7%B8%EB%9E%98%EB%A8%B8%EC%8A%A4-%ED%95%98%EB%85%B8%EC%9D%B4%EC%9D%98-%ED%83%91-Java)

<br><br>

<h2>풀이</h2>

```java
import java.util.*;
class Solution {
	static List<int[]> result;
	public int[][] solution(int n) {
		int[][] answer = {};
		
		result = new ArrayList<>();
		
		hanoi(n, 1, 3, 2);
		
		for (int i = 0; i < result.size(); i++) {
			int[] arr = result.get(i);
			answer[i][0] = arr[0];
			answer[i][1] = arr[1];
		}
		
		return answer;
	}
	
	static void hanoi(int n, int st, int end, int mid) {
		int[] move = {st, end};
		
		if(n == 1) {
			result.add(move);
		} else {
			hanoi(n - 1, st, mid, end);
			result.add(move);
			hanoi(n-1, mid, end, st);
		}
	}
}
```


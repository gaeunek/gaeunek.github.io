---
layout: post
title: "[programmers] 숫자 게임"
subtitle : level 3, Summer/Winter Coding(~2018) 문제
tags: [algorithm]
author: Gaeun Kim
comments : True
---

<h2>문제</h2>

[programmers 숫자 게임 문제 보러가기](https://programmers.co.kr/learn/courses/30/lessons/12987)

<br><br>

<h2>풀이</h2>

```java
import java.util.*;

public class Solution {
	public int solution(int[] A, int[] B) {
		int answer = 0;
		Arrays.sort(A);
		Arrays.sort(B);

		int index = 0;
		for (int i = 0; i < B.length; i++) {
			if(B[i] <= A[index]) {
				continue;
			} else {
				answer++;
				index++;
			}
		}
		
		return answer;
	}
}
```


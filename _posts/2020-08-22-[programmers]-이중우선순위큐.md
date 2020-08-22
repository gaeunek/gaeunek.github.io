---
layout: post
title: "[programmers] 이중우선순위큐"
subtitle : level 3, 힙(heap)문제
tags: [algorithm]
author: Gaeun Kim
comments : True
---

<h2>문제</h2>

[programmers 이중우선순위큐 문제 보러가기](https://programmers.co.kr/learn/courses/30/lessons/42628)

<br><br>

<h2>풀이</h2>

```java
import java.util.*;

public class Solution {
	public int[] solution(String[] operations) {
		int[] answer = new int[2];
		ArrayList<Integer> list = new ArrayList<Integer>();

		for (int i = 0; i < operations.length; i++) {
			String[] tmp = operations[i].split(" ");
			if (tmp[0].equals("I"))
				list.add(Integer.parseInt(tmp[1]));
			else if (!list.isEmpty() && tmp[0].equals("D")) {
				if (Integer.parseInt(tmp[1]) >= 0)
					list.remove(Collections.max(list));
				else
					list.remove(Collections.min(list));
			}
		}

		if (list.isEmpty())
			answer[0] = answer[1] = 0;
		else {
			answer[0] = Collections.max(list);
			answer[1] = Collections.min(list);
		}

		return answer;
	}
}

```

<br><br>

<h2>문제점 해결</h2>

`ArrayList`를 사용해서 `min`값과 `max`값을 찾아내는 부분이 핵심이라고 생각한다.
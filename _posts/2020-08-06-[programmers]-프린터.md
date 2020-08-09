---
layout: post
title: "[programmers] 프린터"
subtitle : level 2, 스택/큐문제
tags: [algorithm]
author: Gaeun Kim
comments : True
---

<h2>문제</h2>

[programmers 프린터 문제 보러가기](https://programmers.co.kr/learn/courses/30/lessons/42587)

<br><br>

<h2>풀이</h2>

```java
import java.util.*;

public class Solution {
	public int solution(int[] priorities, int location) {
		int answer = 0;

		Queue<Print> queue = new LinkedList<>();
		List<Integer> list = new ArrayList<Integer>();

		for (int i = 0; i < priorities.length; i++) {
			queue.add(new Print(priorities[i], i));
			list.add(priorities[i]);
		}

		while (true) {
			Integer max = Collections.max(list);
			int index = 0;

			while (true) {
				Print now = queue.poll();
				if (now.priority == max) {
					index = now.location;
					list.remove(max);
					break;
				} else {
					queue.add(now);
				}
            }
			answer++;
			if (index == location)
				return answer;
		}
	}

	static class Print {
		int priority, location;

		public Print(int priority, int location) {
			this.location = location;
			this.priority = priority;
		}
	}
}
```

<br><br>

<h2>문제점 해결</h2>

대기목록에서 우선순위가 가장 높은 순으로 프린트 되어야하기 때문에 List에 대기목록을 저장한 뒤 `Collections`를 사용해 `max`값을 찾았다.
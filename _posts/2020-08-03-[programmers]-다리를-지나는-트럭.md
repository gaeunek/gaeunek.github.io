---
layout: post
title: "[programmers] 다리를 지나는 트럭"
subtitle : level 2, 스택/큐문제
tags: [algorithm]
author: Gaeun Kim
comments : True
---

<h2>문제</h2>

[programmers 다리를 지나는 트럭 문제 보러가기](https://programmers.co.kr/learn/courses/30/lessons/42586)

<br><br>

<h2>풀이</h2>

```java
import java.util.*;

public class Solution {
	public int solution(int bridge_length, int weight, int[] truck_weights) {
		Queue<Integer> queue = new LinkedList<Integer>();
		Queue<Integer> pass = new LinkedList<Integer>();

		int i = 0, t_weight = 0, cnt = 0;
		do {
			if (i < truck_weights.length && t_weight + truck_weights[i] <= weight) {
				queue.add(truck_weights[i]);
				t_weight += truck_weights[i];
				i++;
				cnt++;
			} else {
				queue.add(0);
				cnt++;
			}

			if (cnt >= bridge_length && !queue.isEmpty()) {
				int tmp = queue.poll();
				
				if(tmp != 0) {
					t_weight -= tmp;
					pass.add(tmp);
				}
			}
		} while (pass.size() < truck_weights.length);

		return cnt+1;
	}
}
```

<br><br>

<h2>문제점 해결</h2>

두개의 Queue를 사용해서 트럭이 모두 다리를 지날 때 까지 While문을 돌려 총 몇초가 걸렸는지 계산하였다.
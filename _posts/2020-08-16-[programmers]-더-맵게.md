---
layout: post
title: "[programmers] 더 맵게"
subtitle : level 2, 힙(Heap)문제
tags: [algorithm]
author: Gaeun Kim
comments : True
---

<h2>문제</h2>

[programmers 더 맵게 문제 보러가기](https://programmers.co.kr/learn/courses/30/lessons/42626)

<br><br>

<h2>풀이</h2>

```java
import java.util.*;

public class Solution {
	public int solution(int[] scoville, int K) {
		int answer = 0;

		PriorityQueue<Integer> pq = new PriorityQueue<Integer>();

		for (int i : scoville)
			pq.add(i);

		while (!pq.isEmpty()) {
			try {
				int first = pq.poll();
				if (first >= K)
					return answer;

				int second = pq.poll();

				int cal = first + (second * 2);
				answer++;

				pq.add(cal);
			} catch (Exception e) {
				return -1;
			}
		}
		return answer;
	}
}
```

<br><br>

<h2>문제점 해결</h2>

처음 실행했을 때 6, 7, 10, 13 번 테스트 케이스가 실패로 나왔는데, 이건 처음`pq`에 `scoville`배열 값을 저장할 때와 `While`문 안에서 `pq`에 `cal`을 저장할 때 K미만 값들만 저장했던것이 문제가 되었다. 만약 `scoville = {1,2,3}, K = 4`이라는 입력이 들어온다면 1회전 했을 때 `pq`엔 `{3,5}`가 저장되어 `return`값은 `2`가 나와야하는정답인데 `While`문 안의 조건 `if(cal < K)`때문에 `pq`엔 `{3}`이 저장되어 `return`값이 `-1`이 된다.


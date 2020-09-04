---
layout: post
title: "[programmers] 디스크 컨트롤러"
subtitle : level 3, 힙(heap) 문제
tags: [algorithm]
author: Gaeun Kim
comments : True
---

<h2>문제</h2>

[programmers 디스크 컨트롤러 문제 보러가기](https://programmers.co.kr/learn/courses/30/lessons/42627)

<br><br>

<h2>풀이</h2>

```java
import java.util.*;

public class Solution {
	public int solution(int[][] jobs) {
		int answer = 0;
		PriorityQueue<int[]> pq = new PriorityQueue<>((o1, o2) -> o1[1] - o2[1]);

		Arrays.sort(jobs, (o1, o2) -> o1[0] - o2[0]);

		int time = 0;
		int idx = 0;
		int len = jobs.length;

		while (idx < len || !pq.isEmpty()) {
			while (idx < len && jobs[idx][0] <= time)
				pq.offer(jobs[idx++]);

			if (pq.isEmpty())
				time = jobs[idx][0];
			else {
				int[] job = pq.poll();
				answer += time - job[0] + job[1];
				time += job[1];
			}
		}

		return answer / len;
	}
}

```

<br><br>

<h2>문제점 해결</h2>

요청부터 종료까지 걸린 시간의 평균을 가장 줄이는 방법을 return 하는 문제로 `PriorityQueue<>`를 사용해 해결했다.

```java
PriorityQueue<int[]> pq = new PriorityQueue<>((o1, o2) -> o1[1] - o2[1]);

Arrays.sort(jobs, (o1, o2) -> o1[0] - o2[0]);
```

`PriorityQueue<>`는 작업의 소요시간을 기준으로 오름차순 정렬이며, `jabs`배열은 작업의 요청시간을 기준으로 오름차순 정렬하였다.

```java
while (idx < len && jobs[idx][0] <= time)
    pq.offer(jobs[idx++]);
```
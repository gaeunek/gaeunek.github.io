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

먼저 `time(현재 시간)`을 0으로 초기화 한 후 작업 요청 시간이 `time`과 같은 작업들을 모두 `pq`에 저장한다. 이때, `jobs`배열 값을 저장하기 때문에 저장한 후엔 `idx`를 증가시켜준다.현재 시간안에 모든 요청들이 들어왔다면 이젠 작업을 처리해야한다.

```java
if (pq.isEmpty())
    time = jobs[idx][0];
else {
	int[] job = pq.poll();
	answer += time - job[0] + job[1];
	time += job[1];
}
```

만약 `pq`가 비어있다면 0으로 초기화 되어있는 `time` 즉, 0초엔 작업 요청이 없다는 의미로 `time`값을 최초 작업 요청 시간으로 바꿔준다.`time`시간내에 요청된 작업들을 처리하기 위해 `pq`에서 하나씩 작업을 `poll`시킨다.입력이 `jobs = {{0,3}, {1,9}, {2,6}}`으로 주어졌을 때

1. `pq`에 `job = {0,3}`이 저장된다.
2. answer = (0 + 0 + 3) = 3
3. time = 0 (현재 작업 요청 시점) + 3 (소요 작업 시간 값)
4. `pq`에 `job = {1.9}`와 `job = {2,6}`이 저장된다.
5. `pq`는 작업 소요시간 기준 오름차순 정렬로 설정해뒀기 때문에 `pq = {2,6}, {1,9}`로 저장된다.
6. answer = 3 (현재 작업 요청되는 시점) - 2 (처리 작업이 요청된 시간) + 6 (작업 소요시간) = 7
7. time = 3 + 6 (처리된 작업 소요시간)

이러한 순서대로 작업이 처리된다.
`answer`의 계산 방법은 이렇다.

- 현작업이 처리되기 전, 앞의 요청이 얼마나 걸렸는지의 값 - 현작업의 요청시간 = 현작업을 처리하기 위해 대기한 시간 `time - job[0] = A`
- 현작업을 처리하기 위해 대기한 시간 + 현작업의 소요시간 = 현작업의 대기부터 처리까지 걸린 시간 `A + job[1] = B`
---
layout: post
title: "[programmers] 디스크 컨트롤러(2번째 풀이)"
subtitle : level 3, 힙(heap) 문제
tags: [algorithm]
author: Gaeun Kim
comments : True
---

<h2>문제</h2>

[programmers 디스크 컨트롤러 문제 보러가기](https://programmers.co.kr/learn/courses/30/lessons/42627)

<br><br>

<h2>문제점 해결</h2>

나는 현재 도움을 받아 푼 문제는 주기적으로 다시 풀어보려 노력중이며, 디스크 컨트롤러 또한 그 문제에 해당하였다.

지난 8월에 푼 이후 다시 풀어봤으나 역시나 시간이 좀 걸렸고, 당시엔 이해한 후 풀었다고 생각했으나 억지로 문제풀이를 외운거나 다름없었던지라 처음 본 문제와도 같았다.

다음번엔 이와 같은 상황을 만들지 않기 위해 한줄한줄 왜 이렇게 코딩했는지 생각하며 풀었고, 나만의 방법, 나만의 알고리즘으로 풀 수 있었다.

#### 1. 입력 데이터 정렬

가장 먼저 한 일은 입력으로 들어온 2차원 배열 jobs의 정렬이었다. jobs는 [요청시점, 작업시간]으로 이루어져있고 요청시점을 기준으로 오름차순 정렬하였다.

<br>

#### 2. PriorityQueue

다음으론 작업시간이 짧은 Task부터 처리할 수 있도록 PriorityQueue를 생성하였다.

<br>

#### 3. 조건

두개의 while문을 사용해 Task를 PriorityQueue(pq)에 넣고 빼는 작업을 했는데, 이 부분에서 가장 중요한것은 while문을 돌릴 조건을 정하는 것이었다.

```java
while (!pq.isEmpty() || index < jobs.length) {
    …
        
    while ((index < jobs.length && sum >= jobs[index][0]) || pq.isEmpty()) {
			pq.add(jobs[index++]);
	}
			
    …
}
```

먼저 pq에 Task를 저장하는 조건은 두 가지인데 먼저 전자는 현재시점(time) 까지의 요청을 저장하는 것이며, 후자는 [[0, 1], [50, 6]]과 같이 Task 사이에 텀이 존재하는 입력이 들어오는 경우를 위해 존재하는 조건이다. 만약 작업을 수행하고 있지 않을 때에는 먼저 요청이 들어온 작업부터 처리해야하기 때문에 후자와 같은 조건을 설정하였다.

<br>

#### 4.  각 작업의 요청부터 종료까지 걸린 시간

만약 현재 처리한 작업(now)가 현재시점(time) 이전에 요청이 들어왔으면 **현재시점 - 요청시점 + 작업시간**이 걸린 시간이 되며, 현재시점 이후에 요청이 들어오면 요청시점부터 작업시간까지 즉, 작업시간만큼만 걸린 시간이 된다.

<br>

#### 5. 현재시점(now)

다음 작업을 처리하기 위해 현재시점의 변동은 중요하다. 현재시점은 간단하게 처리한 작업시간을 더해주기만 하면 되는데 다음 작업과의 사이에 텀이 존재하면 요청시점 + 작업시간을 현재시점으로 바꿔준다.

<br>

#### 주의!!!

처음 jobs배열을 정렬하는 부분에서 요청시점이 같은 경우엔 작업시간을 기준으로 정렬하는데 이건 [[0, 1], [50, 7], [50, 2]]와 같은 입력이 들어왔을 때를 생각하여 작성한 조건이다. 만약 요청시점만으로 정렬하면 [50, 7]가 1번 인덱스가 될 수 있으며, 이 경우엔 작업을 처리하는 이중 while문 중 안쪽 while문의 pq.isEmpty()조건에 해당되고, [50, 2]가 아닌 [50, 7]가 먼저 pq에 저장되어 잘못된 결과가 나올 수 있다.

```java
Arrays.sort(jobs, new Comparator<int[]>() {
    @Override
	public int compare(int[] o1, int[] o2) {
		if(o1[0] == o2[0]) return o1[1] - o2[1];
			return o1[0] - o2[0];
		}
});
```

<br><br>

<h2>풀이</h2>

```java
import java.util.*;
class Solution {
	public int solution(int[][] jobs) {
		int answer = 0;

		Arrays.sort(jobs, new Comparator<int[]>() {
			@Override
			public int compare(int[] o1, int[] o2) {
				if(o1[0] == o2[0]) return o1[1] - o2[1];
				return o1[0] - o2[0];
			}
		});

		PriorityQueue<int[]> pq = new PriorityQueue<>((o1, o2) -> o1[1] - o2[1]);
		int index = 0, cnt = 0, time = 0;
		
		while (!pq.isEmpty() || index < jobs.length) {
			if(cnt == jobs.length) break;
			
			while ((index < jobs.length && time >= jobs[index][0]) || pq.isEmpty()) {
				pq.add(jobs[index++]);
			}
			
			int[] now = pq.poll();
			cnt++;
			int req = now[0];
			int pross = now[1];

			if(time >= req) {
				answer += time - req + pross;
				time += pross;
			} else {
				answer += pross;
				time = req + pross;
			}
		}
		
		return answer / jobs.length;
    }
}
```


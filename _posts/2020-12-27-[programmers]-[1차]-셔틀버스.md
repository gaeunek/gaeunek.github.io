---
layout: post
title: "[programmers] [1차] 셔틀버스"
subtitle : level 3, 2018 KAKAO BLIND RECRUITMENT 문제
tags: [algorithm]
author: Gaeun Kim
comments : True
---

<h2>문제</h2>

[programmers [1차] 셔틀버스 문제 보러가기](https://programmers.co.kr/learn/courses/30/lessons/17678)

<br><br>

<h2>문제점 해결</h2>

1. timetable 배열에 저장된 시간을 모두 minute(분)으로 변환하여 1차원 int 배열에 저장한다.
2. 배열을 오름차순 정렬 후, Queue에 저장한다.
3. n회 for문을 돌려 09:00시(540분)을 기준으로 t씩 증가시켜가며 Queue에 저장된 크루원도착시간과 비교해 크루원이 셔틀버스를 탈 수 있는지 확인한다. 단, 최대인원 m명 이내로만 태운다.

#### 1. 정답을 구하는 4가지 조건

1. 셔틀버스 시간이 막차시간이며 최대인원을 모두 태운 상태라면 마지막으로 태운 크루원 보다 1분 일찍 와야한다는 답이 나온다. → Why? **콘은 게으르기 때문에 항상 대기열 마지막에 있기 때문이다.**
2. 막차시간이지만 남아있는 승객이 없다면 막차시간이 정답이 된다. → 콘이 가장 늦게 사무실에 갈 수 있는 시간을 구하기 때문이다.
3. 남은 승객이 없다면(Queue가 빈 상태라면) 막차시간이 정답이 된다.
4. 막차까지 모두 지나갔는데 승객이 남아있는 경우에도 막차시간이 정답이 된다.

<br><br>

<h2>풀이</h2>

```java
import java.util.*;

class Solution {
	public String solution(int n, int t, int m, String[] timetable) {
        int[] times = new int[timetable.length];
        for (int i = 0; i < timetable.length; i++) {
        	String[] tmp = timetable[i].split(":");
        	times[i] = Integer.parseInt(tmp[0]) * 60 + Integer.parseInt(tmp[1]);
		}
        
        Arrays.sort(times);
        Queue<Integer> queue = new LinkedList<Integer>();
        for (int time : times) {
			queue.add(time);
		}
        
        int answer = 0;
        int max = m;
        int arrived = 540;
        for (int i = 0; i < n; i++, arrived+=t, max = m) {
        	if(queue.isEmpty()) {
        		answer = arrived;
        		continue;
        	}
        	
			while(queue.peek() <= arrived) {
				int last = queue.poll();
				max--;
				
				/* 막차시간이고 막차가 꽉찼다. */
				if(i == n-1 && max == 0) {
					answer = last - 1;
					break;
				}
				
				/* 막차시간이고 남은 승객이 없다. */
				if(queue.isEmpty() && max != 0 && i == n-1) {
					answer = arrived;
					break;
				}
				
				/* 남은 승객이 없다. */
				if(max == 0) break;
			}
		}
        
        /* 승객이 남아있는데 막차는 끝났다. */
        if(answer == 0 && !queue.isEmpty()) answer = arrived - t;
        
        int hour = answer / 60;
        int minute = answer % 60;
        
        return (hour < 10 ? "0" + hour : hour) + ":" + (minute < 10 ? "0" + minute : minute);
    }
}
```


---
layout: post
title: "[programmers] [1차]추석 트래픽"
subtitle : level 3, 2018 KAKAO BLIND RECRUITMENT 문제
tags: [algorithm]
author: Gaeun Kim
comments : True
---

<h2>문제</h2>

[programmers [1차]추석 트래픽 문제 보러가기](https://programmers.co.kr/learn/courses/30/lessons/17676?language=java)

<br><br>

<h2>문제점 해결</h2>

해당 문제에서 주의할 부분은 **처리시간에 시작시간과 끝시간이 포함되어있다**는 것이다.

<br>

#### 1. 시작시간과 끝시간 계산

입력으로 들어오는 시간은 끝시간의 정보이며 처리시간 T를 빼주면 시작시간을 구할 수 있다.

응답완료시간 S가 2016년 9월 15일만 포함하기 때문에 년월을 제외한 일(day)부터 초(s)까지 모두 밀리세컨드(ms)로 변환해준다.

```java
static int[] transTime(String s) {
		int[] res = new int[2];

		String[] times = s.split(" ");
		String[] cal = times[0].split("-");
		String[] hm = times[1].split(":");

		int day = Integer.parseInt(cal[2]) * 24 * 3600000;
		int hour = Integer.parseInt(hm[0]) * 3600000;
		int minute = Integer.parseInt(hm[1]) * 60000;
		int second = (int) (Double.parseDouble(hm[2]) * 1000);
		
		...
	}
```

시작시간과 끝시간이 포함이기때문에 1ms를 빼준다. (처리시간(T)가 소숫점 포함으로 주어질 수 있기 때문에 Double type으로 변환해준 뒤 1000을 곱해준다.)

```java
int pros = (int)(Double.parseDouble(times[2].substring(0, times[2].length() - 1)) * 1000) - 1;
```

계산된 시간의 끝시간 기준부터 1초(=1000ms이지만 실제론 999ms를 더해준다.)까지 4가지 조건에 부합하는 경우가 있다면 count를 증가시켜준다.

```java
for (int i = 0; i < list.size(); i++) {
    int st = list.get(i).end;
	int end = st + 999;
			
	int cnt = 1;
	for (int j = 0; j < list.size(); j++) {
		Point line = list.get(j);
		if(i == j) continue;
		
		if(st <= line.st && line.st <= end) cnt++;
		else if(st <= line.end && line.end <= end) cnt++;
		else if(st >= line.st && line.end >= end) cnt++;
		else if(st <= line.st && line.end <= end) cnt++;
	}
			
	answer = Math.max(answer, cnt);
}
```

![1_second.jpg](/assets/img/20201221_17676.jpg)

<br><br>

<h2>풀이</h2>

```java
import java.util.*;

class Solution {
	public int solution(String[] lines) {
		int answer = 0;
		
		if(lines.length == 1) return 1;
		
		List<Point> list = new ArrayList<>();

		for (int i = 0; i < lines.length; i++) {
			int[] time = transTime(lines[i]);
			list.add(new Point(time[0], time[1]));
		}
		
		Collections.sort(list);

		for (int i = 0; i < list.size(); i++) {
			int st = list.get(i).end;
			int end = st + 999;
			
			int cnt = 1;
			for (int j = 0; j < list.size(); j++) {
				Point line = list.get(j);
				if(i == j) continue;
				
				if(st <= line.st && line.st <= end) cnt++;
				else if(st <= line.end && line.end <= end) cnt++;
				else if(st >= line.st && line.end >= end) cnt++;
				else if(st <= line.st && line.end <= end) cnt++;
			}
			
			answer = Math.max(answer, cnt);
		}

		return answer;
	}

	static int[] transTime(String s) {
		int[] res = new int[2];

		String[] times = s.split(" ");
		String[] cal = times[0].split("-");
		String[] hm = times[1].split(":");
		int pros = (int)(Double.parseDouble(times[2].substring(0, times[2].length() - 1)) * 1000) - 1;
		
		/* 끝시간 */
		int day = Integer.parseInt(cal[2]) * 24 * 3600000;
		int hour = Integer.parseInt(hm[0]) * 3600000;
		int minute = Integer.parseInt(hm[1]) * 60000;
		int second = (int) (Double.parseDouble(hm[2]) * 1000);
		
		res[1] += hour + minute + second + day;
		
		/* 시작시간 */
		res[0] = res[1] - pros;

		return res;
	}
	
	static class Point implements Comparable<Point>{
		int st, end;
		
		public Point(int st, int end) {
			this.st = st;
			this.end = end;
		}

		@Override
		public int compareTo(Point o) {
			return this.st - o.st;
		}
	}
}
```




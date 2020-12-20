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
		
		if(hour + minute + second == 0) {
			day = day - (24 * 3600000);
			res[1] += 24 * 3600000;
		} else res[1] += hour + minute + second;
		res[1] += day;
		
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




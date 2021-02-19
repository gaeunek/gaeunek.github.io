---
layout: post
title: "[programmers] 광고 삽입"
subtitle : level 3, 2021 KAKAO BLIND RECRUITMENT 문제
tags: [algorithm]
author: Gaeun Kim
comments : True
---

<h2>문제</h2>

[programmers 광고 삽입 문제 보러가기](https://programmers.co.kr/learn/courses/30/lessons/72414)

<br><br>

<h2>풀이</h2>

```java
class Solution {
	public String solution(String play_time, String adv_time, String[] logs) {
		String[] tmp, tmp1, tmp2;

		tmp = play_time.split(":");
		int play_time_sec = Integer.parseInt(tmp[0]) * 3600 + Integer.parseInt(tmp[1]) * 60 + Integer.parseInt(tmp[2]);
		tmp = adv_time.split(":");
		int adv_time_sec = Integer.parseInt(tmp[0]) * 3600 + Integer.parseInt(tmp[1]) * 60 + Integer.parseInt(tmp[2]);

		if (play_time == adv_time)
			return "00:00:00";

		long[] total_time = new long[360000];

		for (int i = 0; i < logs.length; i++) {
			tmp = logs[i].split("-");
			tmp1 = tmp[0].split(":");
			tmp2 = tmp[1].split(":");

			int start = Integer.parseInt(tmp1[0]) * 3600 + Integer.parseInt(tmp1[1]) * 60 + Integer.parseInt(tmp1[2]);
			int end = Integer.parseInt(tmp2[0]) * 3600 + Integer.parseInt(tmp2[1]) * 60 + Integer.parseInt(tmp2[2]);

			total_time[start]++;
			total_time[end]--;
		}

		for (int i = 1; i < play_time_sec; i++) {
			total_time[i] += total_time[i - 1];
		}

		for (int i = 1; i < play_time_sec; i++) {
			total_time[i] += total_time[i - 1];
		}

		long max = 0, result = 0;
		int max_start_sec = 0;

		for (int end = adv_time_sec - 1; end < play_time_sec; end++) {
			int st = end - adv_time_sec;
			if (st < 0)
				result = total_time[end];
			else
				result = total_time[end] - total_time[st];

			if (result > max) {
				max = result;
				max_start_sec = st + 1;
			}
		}

		return trans(max_start_sec);
	}

	static String trans(int max_start_sec) {
		int hour = max_start_sec / 3600;
		int minute = (max_start_sec % 3600) / 60;
		int second = max_start_sec % 3600 % 60;
		return (hour < 10 ? "0" + hour : hour) + ":" + (minute < 10 ? "0" + minute : minute) + ":"
				+ (second < 10 ? "0" + second : second);
	}
}
```


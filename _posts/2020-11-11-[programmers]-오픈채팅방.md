---
layout: post
title: "[programmers] 오픈채팅방"
subtitle : level 2, 2019 KAKAO BLIND RECRUITMENT 문제
tags: [algorithm]
author: Gaeun Kim
comments : True
---

<h2>문제</h2>

[programmers 오픈채팅방 문제 보러가기](https://programmers.co.kr/learn/courses/30/lessons/42888)

<br><br>

<h2>문제점 해결</h2>

입장 또는 닉네임 변경 활동이 일어나면 map에 ID(key)와 닉네임(value)를 저장하고 list엔 record의 입장, 나가기, 닉네임 변경 등의 모든 기록을 저장한다.

```java
for (int i = 0; i < record.length; i++) {
    String[] input = record[i].split(" "); //활동, 아이디, 닉네임
	if(!input[0].equals("Leave")) {
        map.put(input[1], input[2]);
		if(input[0].equals("Change")) continue; 
	}
	list.add(new Records(input[1], input[0]));
}
```

<br><br>

<h2>풀이</h2>

```java
import java.util.*;
class Solution {
	public String[] solution(String[] record) {
		Map<String, String> map = new HashMap<>(); //key:ID, value:닉네임
		List<Records> list = new ArrayList<>();
		
		for (int i = 0; i < record.length; i++) {
			String[] input = record[i].split(" "); //활동, 아이디, 닉네임
			if(!input[0].equals("Leave")) {
				map.put(input[1], input[2]);
				if(input[0].equals("Change")) continue; 
			}
			
			list.add(new Records(input[1], input[0]));
		}

		String[] answer = new String[list.size()];
		for (int i = 0; i < list.size(); i++) {
			Records now = list.get(i);
			answer[i] = map.get(now.id);
			if(now.active.equals("Enter")) answer[i] += "님이 들어왔습니다.";
			else answer[i] += "님이 나갔습니다.";
		}
		
		return answer;
	}
	
	static class Records {
		String id, active;
		
		public Records(String id, String active) {
			this.id = id;
			this.active = active;
		}
	}
}
```


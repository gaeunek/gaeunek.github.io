---
layout: post
title: "[programmers] [3차]압축"
subtitle : level 2, 2018 KAKAO BLIND RECRUITMENT 문제
tags: [algorithm]
author: Gaeun Kim
comments : True
---

<h2>문제</h2>

[programmers [3차] 압축 문제 보러가기](https://programmers.co.kr/learn/courses/30/lessons/17684)

<br><br>

<h2>문제점 해결</h2>

**HashMap을 사용**하여 알파벳 A~Z까지를 key값으로, 1~26까지를 value값으로 map에 저장하였고, 입력으로 들어오는 단어를 key값으로 사용하여 색인번호(value)를 출력하였다.

<br><br>

<h2>풀이</h2>

```java
import java.util.*;

class Solution {
	public int[] solution(String msg) {
		Map<String, Integer> map = new HashMap<>();
		for (char i = 'A', num = '1'; i <= 'Z'; i++, num++)
			map.put(String.valueOf(i), num - '0');

		char[] tmp = msg.toCharArray();
		List<Integer> result = new ArrayList<Integer>();
		String now = "", next = "";
		for (int i = 0; i < tmp.length; i++) {
			now += tmp[i];
			
			if(i == tmp.length-1) {
				result.add(map.get(now));
				break;
			}
			
			for (int j = i+1; j < tmp.length; j++) {
				next = String.valueOf(tmp[j]);
				
				if(map.get(now+next) == null) {
					map.put(now+next, map.size()+1);
					result.add(map.get(now));
					now = "";
					break;
				}else break;
			}
		}
		
		int[] answer = new int[result.size()];
		for (int i = 0; i < result.size(); i++) answer[i] = result.get(i);
		return answer;
	}
}

```


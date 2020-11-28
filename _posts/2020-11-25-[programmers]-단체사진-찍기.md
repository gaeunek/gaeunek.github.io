---
layout: post
title: "[programmers] 단체사진 찍기"
subtitle : level 2, 2017 카카오코드 본선 문제
tags: [algorithm]
author: Gaeun Kim
comments : True
---

<h2>문제</h2>

[programmers 단체사진 찍기 문제 보러가기](https://programmers.co.kr/learn/courses/30/lessons/1835)

<br><br>

<h2>문제점 해결</h2>

permutation을 사용해 풀 수 있는 문제였다.

먼저 set에 8! 가지의 경우를 모두 저장한 뒤, 하나씩 꺼내어 data에 나와있는 조건에 모두 부합하는지 확인하여 answer값을 증가시킨다.

<br><br>

<h2>풀이</h2>

```java
import java.util.*;
class Solution {
	static Set<String> set;
	static boolean[] used;
	static List<String> list;
	static String[] friends = {"A", "C", "F", "J", "M", "N", "R", "T"};
	public int solution(int n, String[] data) {
		int answer = 0;
		set = new HashSet<>();
		used = new boolean[friends.length];
		list = new ArrayList<>();
		permutation();
		Iterator<String> it = set.iterator();
		int c1, c2, res, value;
		
		while(it.hasNext()) {
			String str = it.next();
			boolean flag = false;
			
			for (int i = 0; i < n; i++) {
				flag = false;
				String d = data[i];
				c1 = str.indexOf(d.charAt(0));
				c2 = str.indexOf(d.charAt(2));
				value = d.charAt(d.length()-1) - '0';
				res = Math.abs(c1-c2) - 1;
				if(d.contains("=") && res == value) {
					flag = true;
				}else if(d.contains(">") && res > value) {
					flag = true;
				}else if(d.contains("<") && res < value) {
					flag = true;
				}
				
				if(!flag) break;
			}
			if(flag) answer++; 
		}
		
		return answer;
	}
	
	static void permutation() {
		if(list.size() == friends.length) {
			StringBuilder sb = new StringBuilder();
			for (String str : list) sb.append(str);
			set.add(new String(sb));
			return;
		}
		
		for (int i = 0; i < friends.length; i++) {
			if(!used[i]) {
				list.add(friends[i]);
				used[i] = true;
				permutation();
				used[i] = false;
				list.remove(friends[i]);
			}
		}
	}
}
```
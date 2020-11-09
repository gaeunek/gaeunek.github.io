---
layout: post
title: "[programmers] 수식 최대화"
subtitle : level 2, 2020 카카오 인턴십 문제
tags: [algorithm]
author: Gaeun Kim
comments : True<
---

<h2>문제</h2>

[programmers 수식 최대화 문제 보러가기](https://programmers.co.kr/learn/courses/30/lessons/67257)

<br><br>

<h2>문제점 해결</h2>

연산자 1개 일 땐 1! = 1가지, 2! = 2가지, 3! = 6가지의 조합을 만들어 최댓값을 return하도록 코딩하였다.

<br><br>

<h2>풀이</h2>

```java
import java.util.*;

class Solution {
	static List<String> exp, oper, result;
	static boolean[] used;
	static long answer;
	public long solution(String expression) {
		exp = new ArrayList<>();
		oper = new ArrayList<>();
		
		char[] tmp = expression.toCharArray();
		String str = "", s;
		for (int i = 0; i < tmp.length; i++) {
			if(tmp[i] == '-' || tmp[i] == '+' || tmp[i] == '*') {
				s = String.valueOf(tmp[i]);
				exp.add(str);
				exp.add(s);
				if(!oper.contains(s)) oper.add(s);
				str = "";
				continue;
			}
			
			str += tmp[i];
			if(i == tmp.length-1) exp.add(str);
		}
		
		result = new ArrayList<>();
		used = new boolean[oper.size()];
		priorityOper();
		
		return answer;
	}
	
	static void priorityOper(){
		if(result.size() == oper.size()) {
			long res = calculation();
			answer = res > answer ? res : answer;
			return;
		}
		
		for (int i = 0; i < oper.size(); i++) {
			if(!used[i]) {
				used[i] = true;
				result.add(oper.get(i));
				priorityOper();
				result.remove(oper.get(i));
				used[i] = false;
			}
		}
	}
	
	static long calculation() {
		List<String> temp = new ArrayList<>();
		for (String str : exp) temp.add(str);
		
		for (int i = 0; i < result.size(); i++) {
			String op = result.get(i);
			for (int j = 0; j < temp.size(); j++) {
				if(temp.get(j).equals(op)) {
					long a = Long.parseLong(temp.get(j-1));
					long b = Long.parseLong(temp.get(j+1));
					a = op(a, b, op);
					temp.add(j-1, String.valueOf(a));
					for (int k = 0; k < 3; k++) temp.remove(j);
					j = 0;
				}
			}
		}
		
		return Math.abs(Long.parseLong(temp.get(0)));
	}
	
	static long op(long a, long b, String op) {
		switch (op) {
		case "+":
			return a += b;
		case "-":
			return a -= b;
		default:
			return a *= b;
		}
	}
}
```


---
layout: post
title: "[programmers] 괄호 변환"
subtitle : 2020 KAKAO BLIND RECRUITMENT 문제
tags: [algorithm]
author: Gaeun Kim
comments : True
---

<h2>문제</h2>

[programmers 2020 KAKAO BLIND RECRUITMENT 괄호 변환 문제 보러가기](https://programmers.co.kr/learn/courses/30/lessons/60058)

<br><br>

<h2>풀이 및 해설</h2>

```java
import java.util.*;

class Solution {
	static Stack<Character> stack;
	static String answer;

	public String solution(String p) {
		// 1.이미 "올바른 괄호 문자열"이거나 빈 문자열인 경우 그대로 반환
		if (check(p.toCharArray()) || p.equals(""))
			return p;

		stack = new Stack<Character>();
		answer = "";
		return recursion(p);
	}

	static String recursion(String p) {
		String[] substr = substr(p);
		String u = "";

		// 3.u가 "올바른 괄호 문자열"이라면 v에 대해 1단계부터 다시 수행하여 u에 이어 붙인다.
		if (check(substr[0].toCharArray())) { 
			u = substr[0] + resultV(substr[1]);
		} else {
			// 4.문자열 u가 "올바른 괄호 문자열"이 아닌경우
			
			// 4-1.빈 문자열에 첫 번째 문자로 '('를 붙인다.
			// 4-2. 문자열 v에 대해 1단계부터 재귀적으로 수행한 결과를 이어 붙인다.
			// 4-3. ')'를 다시 붙인다.
			u += "(" + resultV(substr[1]) + ")";

			// 4-4. u의 첫 번째와 마지막 문자를 제거하고, 나머지 문자열의 괄호 방향을 뒤집어서 뒤에 붙여준다.
			if (substr[0].length() > 2) {
				String u_substr = substr[0].substring(1, substr[0].length() - 1);
				char[] tmp = u_substr.toCharArray();
				for (int i = 0; i < tmp.length; i++) {
					if (tmp[i] == '(')
						u += ")";
					else
						u += "(";
				}
			}
		}

		return u;
	}

	static String resultV(String v) {
		if (check(v.toCharArray()))
			return v;
		else
			return recursion(v); // step1
	}

	// 2.두 "균형잡힌 괄호 문자열" u,v로 분리한다.
	static String[] substr(String p) {
		int idx = 0;
		int value = 0;
		char[] p_to_char = p.toCharArray();

		do {
			char c = p_to_char[idx++];
			if (c == '(')
				value++;
			else
				value--;

			if (value == 0)
				break;
		} while (idx < p.length());

		String[] tmp = new String[2];

		tmp[0] = p.substring(0, idx);
		tmp[1] = p.substring(idx);

		return tmp;
	}

	// "올바른 괄호 문자열"인지 확인
	static boolean check(char[] c) {
		if (c.length == 0) // v는 빈 문자열 일 수 있다.
			return true;

		for (char cc : c) {
			try {
				if (cc == '(')
					stack.push(cc);
				else
					stack.pop();
			} catch (Exception e) {
				return false;
			}
		}

		if (stack.isEmpty())
			return true;
		else
			return false;
	}
}
```


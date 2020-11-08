---
layout: post
title: "[programmers] [3차]n진수 게임"
subtitle : level 2, 2018 KAKAO BLIND RECRUITMENT 문제
tags: [algorithm]
author: Gaeun Kim
comments : True
---

<h2>문제</h2>

[programmers [3차]n진수 게임 문제 보러가기](https://programmers.co.kr/learn/courses/30/lessons/17687)

<br><br>

<h2>문제점 해결</h2>

최대로 주어지는 n(진수)가 16이기 때문에 values의 값을 0부터 F까지 작성된 String타입으로 저장하였다. 진수로 변환된 값을 저장할 배열 game의 크기는 튜브의 순서(p)가 가장 마지막 순서일때를 고려해서 m×t로 설정했다. 처음엔 진법변환 알고리즘을 몰라서 코드가 지저분했지만 구글링을 참고해 나름 깨끗하게 코딩하였다고 생각한다!

<br><br>

<h2>풀이</h2>

```java
import java.util.*;

class Solution {
	static Stack<String> stack;
	static String values = "0123456789ABCDEF";
	public String solution(int n, int t, int m, int p) {
		String answer = "";
		
		String[] game = new String[t*m];
		stack = new Stack<>();
		int num = 0, index = 0;
		while(index < t*m) {
			String res = cal(num++, n);
			if(res.length() == 1) {
				game[index++] = res;
				continue;
			}
			
			for (int i = 0; i < res.length(); i++) {
				if(index >= t*m) break;
				game[index++] = String.valueOf(res.charAt(i));
			}
		}
		
		for (int i = p-1, cnt = 0; i < t*m && cnt < t; i+=m, cnt++) answer += game[i];
		return answer;
	}
	
	static String cal(int num, int n) {
		int quotient = num / n;
		int remainder = num % n;
		if(quotient == 0) return String.valueOf(values.charAt(remainder));
		else if(quotient == 1) return String.valueOf(quotient) + values.charAt(remainder);
		return cal(num / n, n) + values.charAt(remainder);
	}
}
```


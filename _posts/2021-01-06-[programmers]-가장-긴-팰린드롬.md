---
layout: post
title: "[programmers] 가장 긴 팰린드롬"
subtitle : level 3 문제
tags: [algorithm]
author: Gaeun Kim
comments : True
---

<h2>문제</h2>

[programmers 가장 긴 팰린드롬 문제 보러가기](https://programmers.co.kr/learn/courses/30/lessons/12904)

<br><br>

<h2>문제점 해결</h2>

앞뒤를 뒤집어도 똑같은 문자열을 팰린드롬(palindrome)이라고 한다. 예제에는 나와있지 않지만 "abccba"와 같은 짝수 길이의 입력 또한 고려해서 처리 방법을 홀수, 짝수 두 가지로 구현하였다.

먼저, 홀수 처리 방법이다. 홀수는 기본적으로 중심이 되는 알파벳이 있다 가정한 뒤 중심값 앞(left), 뒤(right) 값을 비교하여 가장 큰 값을 찾아낸다.

```java
static int odd(String s) {
    int res = 0, w = 1;
	char left, right;
		
	for (int i = 1; i < N-1; i++) {
		left = s.charAt(i-w);
		right = s.charAt(i+w);
			
		if(left == right) {
			res = Math.max(res, w);
			if(i-w == 0 || i+w == N-1) {
				w = 1;
				continue;
			}
			w++;
			i--;
		} else w = 1;
	}
	return res;
}
```

다음으로 짝수 처리 방법이다. 짝수는 딱 반으로 접으면 거울에 비춘 모양처럼 동일한 모습이 나와야 한다. ex) abccba

```java
static int even(String s) {
	int res = 0, w = 0;
	char left, right;
		
	for (int i = 0; i < N-1; i++) {
		left = s.charAt(i-w);
		right = s.charAt(i+w+1);
			
		if(left == right) {
			res = Math.max(res, w+1);
				
			if(i-w == 0 || i+w+1 == N-1) {
				w = 0;
				continue;
			}	
			w++;
			i--;
		} else w = 0;
	}
	return res;
}
```

두 가지 함수 모두 팰린드롬이 몇개인지(쌍 ex) aa, bb)를 찾는 함수로 총 팰린드롬의 길이가 아니기 때문에 홀수는 기준이 되는 중심 알파벳 1개를 더해 res * 2 + 1개가 되며, 짝수는 res * 2개가 된다.

#### 1. odd 함수와 even 함수 return 값이 동일하다면?

예를 들어 "ccccccc"와 같은 입력이 들어온다면 정답은 7이 된다. 이때 odd와 even함수의 return 값은 3으로 동일하며 가장 긴 경우는 중심값 1을 더해주는 odd함수를 호출한 경우이기 때문에 다음과 같은 조건이 필요하다.

```java
if(odd + even == 0) return 1;
else if(odd >= even) return odd * 2 + 1;
else return even * 2;
```

<br><br>

<h2>풀이</h2>

```java
class Solution{
	static int N;
	public int solution(String s) {
		N = s.length();
		
		int odd = odd(s);
		int even = even(s);
		if(odd + even == 0) return 1;
		else if(odd >= even) return odd * 2 + 1;
		else return even * 2;
	}
	
	static int odd(String s) {
		int res = 0, w = 1;
		char left, right;
		
		for (int i = 1; i < N-1; i++) {
			left = s.charAt(i-w);
			right = s.charAt(i+w);
			
			if(left == right) {
				res = Math.max(res, w);
				if(i-w == 0 || i+w == N-1) {
					w = 1;
					continue;
				}
				w++;
				i--;
			} else w = 1;
		}
		
		return res;
	}
	
	static int even(String s) {
		int res = 0, w = 0;
		char left, right;
		
		for (int i = 0; i < N-1; i++) {
			left = s.charAt(i-w);
			right = s.charAt(i+w+1);
			
			if(left == right) {
				res = Math.max(res, w+1);
				
				if(i-w == 0 || i+w+1 == N-1) {
					w = 0;
					continue;
				}
				
				w++;
				i--;
			} else w = 0;
		}
		
		return res;
	}
}
```




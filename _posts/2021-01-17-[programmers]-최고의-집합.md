---
layout: post
title: "[programmers] 최고의 집합"
subtitle : level 3 문제
tags: [algorithm]
author: Gaeun Kim
comments : True
---

<h2>문제</h2>

[programmers 최고의 집합 문제 보러가기](https://programmers.co.kr/learn/courses/30/lessons/12938)

<br><br>

<h2>문제점 해결</h2>

n = 3, s = 8 일 때의 result = [2, 3, 3] 이다. 예제문제와 직접 경우의 수를 작성해보며 알게된 사실은 표준편차가 가장 작을 때의 값이 정답이라는 것이다.

1. s를 n으로 나눈 몫을 n개 만큼 저장한다. ex) n = 3, s = 8, s/n = 2 result = [2, 2, 2]
2. s를 n으로 나눈 나머지 수 만큼 result에 +1을 해준다. ex) s%n = 2, result = [2, 3, 3]

이 문제를 풀며 느낀 점은 정말 이런 저런 문제를 많이 풀어봐야 한다는 것이었다.

<br><br>

<h2>풀이</h2>

```java
class Solution {
	public int[] solution(int n, int s) {
        int[] answer = {};
        
        int value = s / n;
        if(value == 0) return new int[] {-1};
        int remain = s % n;
        
        answer = new int[n];
        for (int i = 0; i < n; i++) {
			answer[i] = value;
		}

        for (int i = n-1, j = remain; i >= 0 && j > 0; i--, j--) {
        	answer[i] += 1;
		}
        
        return answer;
    }
}
```


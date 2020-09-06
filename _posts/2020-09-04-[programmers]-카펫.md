---
layout: post
title: "[programmers] 카펫"
subtitle : level 2, 완전탐색 문제
tags: [algorithm]
author: Gaeun Kim
comments : True
---

<h2>문제</h2>

[programmers 카펫 문제 보러가기](https://programmers.co.kr/learn/courses/30/lessons/42842)

<br><br>

<h2>풀이</h2>

```java
import java.util.*;

class Solution {
	public int[] solution(int brown, int yellow) {
		int[] answer = new int[2];

		ArrayList<int[]> yellow_list = new ArrayList<>();

		for (int i = yellow; i >= 1; i--) {
			if(yellow % i == 0) {
				int[] tmp = {i, yellow / i};
				yellow_list.add(tmp);
			}
		}
		
		int size = brown + yellow;

		for (int i = size; i >= 3; i--) {
			boolean flag = false;
			if (size % i == 0) {
				int width = i;
				int height = size / i;

				for (int j = 0; j < yellow_list.size(); j++) {
					int[] yellows = yellow_list.get(j);
					if (width - 2 >= yellows[0] && height - 2 >= yellows[1] && width >= height) {
						answer[0] = width;
						answer[1] = height;
						flag = true;
					}
				}
			}
			
			if(flag) break;
		}

		return answer;
	}
}
```

<br><br>

<h2>문제점 해결</h2>

먼저 int배열 타입의 `ArrayList<>`를 하나 생성하여 `yellow`의 약수를 구해 `yellow_list`에 저장한다.

```java
ArrayList<int[]> yellow_list = new ArrayList<>();

for (int i = yellow; i >= 1; i--) {
	if(yellow % i == 0) {
		int[] tmp = {i, yellow / i};
			yellow_list.add(tmp);
	}
}
```

<br>

카펫의 형태를 보면 노란색 카펫이 갈색 카펫에 둘러쌓여 있기 때문에 노란색 까펫이 둘러쌓이기 위해선 갈색 카펫이 노란색 카펫의 가로, 세로보다 크기가 2이상씩 커야한다. 또한 가로길이가 세로길이보다 크거나 같다라는 조건도 있기 때문에 이 조건에 일치하는 결과값을 도출해내야한다.

```java
int size = brown + yellow;

for (int i = size; i >= 3; i--) {
    boolean flag = false;
    if (size % i == 0) {
        int width = i;
        int height = size / i;

		for (int j = 0; j < yellow_list.size(); j++) {
			int[] yellows = yellow_list.get(j);
			if (width - 2 >= yellows[0] && height - 2 >= yellows[1] && width >= height) {
				answer[0] = width;
				answer[1] = height;
				flag = true;
			}
		}
	}
	if(flag) break;
}
```

일치하는 값을 찾아내면 `flag`값을 `true`로 바꿔 `for`문을 중지시킨다.
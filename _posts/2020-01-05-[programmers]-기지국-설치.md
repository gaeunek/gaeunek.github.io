---
layout: post
title: "[programmers] 기지국 설치"
subtitle : level 3, Summer/Winter Coding(~2018) 문제
tags: [algorithm]
author: Gaeun Kim
comments : True
---

<h2>문제</h2>

[programmser 기지국 설치 문제 보러가기](https://programmers.co.kr/learn/courses/30/lessons/12979)

<br><br>

<h2>문제점 해결</h2>

전파 전달 지점을 모두 계산하는 방법으로 접근하였다. `빈 공간/전파거리`값이 0이 아니라면 몫 + 1을 0이라면 몫을 answer에 더해줬다. 접근 방법은 좋았으나 세세한 부분에서 실수가 있어 오래 걸렸던 문제다.

#### 1. 전파가 겹치는 경우

`{5, 10, 15, 20}`와 같은 입력이 주어지면 전파들이 겹칠 수 있고, 아래 코드에서 if문을 처리해주지 않으면 -값이 존재하기 때문에 의도치 않은 값들을 도출한다. 처음엔 이 부분을 고려하지 않고 코딩하여 테스트케이스는 통과했지만 정확성에서 실패하였다.

```java
for (int i = 0; i < stations.length; i++) {
	left = stations[i] - w < 1 ? 1 : stations[i] - w;
	right = stations[i] + w > n ? n : stations[i] + w;
			
	num = left - st;
	
    // - 값이 나올 수 있기 때문에 꼭 존재해야 한다.
	if(num <= 0) {
		st = right + 1;
		continue;
	}
    
	answer += num / distance;
	if(num % distance != 0) answer++;
	st = right + 1;
}
```

<br>

#### 2. 설정 값을 확인하자!!

st는 이전 전파의 가장 끝 지점 값의 다음 지점을 저장하므로 즉, st는 전파가 닿지 않은 부분이다. 때문에 for문이 끝난 후 아래 if문을 실행할 때 크기 구분을 잘 해야한다. 처음엔 st가 n보다 작을 때만 if문을 실행했는데 위에서 말했듯이 st는 전파가 닿지 않은 부분이기 때문에 st값이 n이어도 n에 전파가 닿지 않았기 때문에 이 또한 포함시켜 st <= n 으로 조건을 바꾸니 정확성과 효율성을 통과할 수 있었다.

```java
if(st <= n) {
	num = n - st + 1;
	answer += num / distance;
	if(num % distance != 0) answer++;
}
```

<br>

#### 3. 반례

전파가 다닥다닥 붙어있는 경우 `(13, new int[] {5, 8, 12}, 1)`와 전파가 겹치는 경우를 잘 고려하자!

<br><br>

<h2>풀이</h2>

```java
class Solution {
	public int solution(int n, int[] stations, int w) {
		int answer = 0;
		int distance = w * 2 + 1;
		
		int st = 1, left = 1, right = 1, num = 0;
		for (int i = 0; i < stations.length; i++) {
			left = stations[i] - w < 1 ? 1 : stations[i] - w;
			right = stations[i] + w > n ? n : stations[i] + w;
			
			num = left - st;
			if(num <= 0) {
				st = right + 1;
				continue;
			}
			answer += num / distance;
			if(num % distance != 0) answer++;
			st = right + 1;
		}
		
		if(st <= n) {
			num = n - st + 1;
			answer += num / distance;
			if(num % distance != 0) answer++;
		}
		
		return answer;
	}
}
```




---
layout: post
title: "[programmers] 징검다리 건너기"
subtitle : level 3, 2019 카카오 개발자 겨울 인턴십 문제
tags: [algorithm]
author: Gaeun Kim
comments : True
---

<h2>문제</h2>

[programmers 징검다리 건너기 문제 보러가기](https://programmers.co.kr/learn/courses/30/lessons/64062)

<br><br>

<h2>문제점 해결</h2>

이중 for문을 사용하게 되면 stones 배열의 최대 크기가 200,000일 경우 시간초과가 발생할 수 있다. 접근 방법은 이분탐색이었으며 연속된 0값의 디딤돌이 k개가 되었을 때 지금 건너는 사람까지가 최대 건널 수 있는 인원임을 answer에 저장한 후 최댓값(max)을 mid-1로 갱신시킨다. 그 다음 min부터 max까지의 범위 내에서 현재 answer값이 정말 최대 인원이 맞는지 찾는 과정을 반복한다.

<br><br>

<h2>풀이</h2>

```java
class Solution {
	public int solution(int[] stones, int k) {
		int answer = 0;
		int max = 0, min = Integer.MAX_VALUE;
		for (int i = 0; i < stones.length; i++) {
			max = Math.max(stones[i], max);
			min = Math.min(stones[i], min);
		}
		
		while(min <= max) {
			int mid = (min + max) / 2;
			int cnt = 0;
			boolean flag = false;
			
			for (int i = 0; i < stones.length; i++) {
				if(stones[i] - mid <= 0) cnt++;
				else cnt = 0;
				
				if(cnt == k) {
					flag = true;
					break;
				}
			}
			
			if(flag) {
				answer = mid;
				max = mid - 1;
			} else {
				min = mid + 1;
			}
		}
		
		return answer;
	}
}
```




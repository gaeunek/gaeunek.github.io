---
layout: post
title: "[programmers] 소수 만들기"
subtitle : level 2, Summer/Winter Coding(~2018) 문제
tags: [algorithm]
author: Gaeun Kim
comments : True
---

<h2>문제</h2>

[programmers 소수 만들기 문제 보러가기](https://programmers.co.kr/learn/courses/30/lessons/12977?language=java)

<br><br>

<h2>문제점 해결</h2>

시간 복잡도에 대한 이해가 필요했던 문제라고 생각한다.

**재귀함수를 돌려 조합을 얻는 것 > 4중 for문**

<br><br>

<h2>풀이</h2>

```java
class Solution {
	static int answer;

	public int solution(int[] nums) {
		answer = 0;
		for (int i = 0; i < nums.length; i++) {
			for (int j = i+1; j < nums.length; j++) {
				for (int k = j+1; k < nums.length; k++) {
					boolean flag = false;
					int num = nums[i] + nums[j] + nums[k];
					for (int l = 2; l < num; l++) {
						if(num % l == 0) {
							flag = true;
							break;
						}
					}
					if(!flag) answer++;
				}
			}
		}
		return answer;
	}
}
```


---
layout: post
title: "[programmers] 스타 수열"
subtitle : level 3, 월간 코드 챌린지 시즌1 문제
tags: [algorithm]
author: Gaeun Kim
comments : True
---

<h2>문제</h2>

[programmers 스타 수열 문제 보러가기](https://programmers.co.kr/learn/courses/30/lessons/70130)

<br><br>

<h2>문제점 해결</h2>

완전 탐색을 해야하는 문제라고 생각했지만 처음엔 시간초과가 발생했다. Map에 숫자의 등장 횟수를 저장하니 이걸 응용해서 **등장 횟수 × 2**가 현재 answer 값보다 작은 경우엔 for문을 돌릴 필요가 없으므로 시간을 줄일 수 있다고 생각한다.

그리고 처음엔 key값만 찾아서 좌, 우로 확인을 한 뒤, boolean배열로 체크했는데 이 부분을 모두 지우고 두개씩 확인하면서 조건과 부합한 경우에만 count를 증가시켰다.

<br><br>

<h2>풀이</h2>

```java
import java.util.*;

class Solution {
	public int solution(int[] a) {
		int answer = 0;
		
		Map<Integer, Integer> map = new HashMap<>();
		
		for(int i : a) map.put(i, map.getOrDefault(i, 0)+1);
		
		for (int key : map.keySet()) {
			if(map.get(key) * 2 <= answer) continue;
			
			int count = 0;
			for (int i = 0; i < a.length-1; i++) {
				if((a[i] == key || a[i+1] == key) && (a[i] != a[i+1])) {
					count += 2;
					i++;
				}
			}
			
			answer = Math.max(answer, count);
		}
			
		return answer;
	}
}
```


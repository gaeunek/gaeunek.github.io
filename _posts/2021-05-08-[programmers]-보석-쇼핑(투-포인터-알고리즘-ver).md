---
layout: post
title: "[programmers] 보석 쇼핑(투 포인터 알고리즘 ver)"
subtitle : level 3, 2020 카카오 인턴십 문제
tags: [algorithm]
author: Gaeun Kim
comments : True
---

<h2>문제</h2>

[programmers 보석 쇼핑 문제 보러가기](https://programmers.co.kr/learn/courses/30/lessons/67258?language=java)

<br><br>

<h2>이전 풀이법</h2>

[이전 풀이 방법](https://gaeunek.github.io/2020/12/28/programmers-%EB%B3%B4%EC%84%9D-%EC%87%BC%ED%95%91.html)

이전보다 더 빠른 방법인 투포인터 알고리즘을 사용하여 풀었다. 정확도는 이전과 비슷했으나 효율성에선 시간이 비교불가할 정도로 빨라졌다.

<br><br>

<h2>풀이</h2>

```java
import java.util.*;

class Solution {
	public int[] solution(String[] gems) {
		int[] answer = new int[2];

		int st = 0, end = 0;
		Set<String> set = new HashSet<>();
		for (int i = 0; i < gems.length; i++) {
			set.add(gems[i]);
		}

		Map<String, Integer> map = new HashMap<>();

		int min = Integer.MAX_VALUE;

		while (st <= end) {

			if (map.size() == set.size()) {
				if (min > end - st) {
					answer[0] = st + 1;
					answer[1] = end;
					min = end - st;
				}

				map.replace(gems[st], map.get(gems[st]) - 1);
				if (map.get(gems[st]) == 0)
					map.remove(gems[st]);
				st++;
			} else if (end == gems.length) {
				break;
			} else if (map.size() < set.size()) {
				map.put(gems[end], map.getOrDefault(gems[end], 0) + 1);
				end++;
			}
		}

		return answer;
	}
}
```


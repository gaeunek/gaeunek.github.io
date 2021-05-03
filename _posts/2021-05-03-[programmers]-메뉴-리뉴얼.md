---
layout: post
title: "[programmers] 메뉴 리뉴얼"
subtitle : 2021 KAKAO BLIND RECRUITMENT 문제
tags: [algorithm]
author: Gaeun Kim
comments : True
---

<h2>문제</h2>

[programmers 메뉴 리뉴얼 문제 보러가기](https://programmers.co.kr/learn/courses/30/lessons/72411)

<br><br>

<h2>풀이법</h2>

두번째 풀이여서 이전과는 다른 방법으로 풀어보겠다고 교집합을 찾아서 그 안에서 조합을 찾았는데 결과는 30점도 넘기지 못했다. 각 손님이 주문한 메뉴의 여러 조합을 찾아서 Map에 저장한 후 2번 이상 주문되었으며 가장 많이 주문된 메뉴 조합만 출력할 수 있도록 코딩하였다.

<br><br>

<h2>풀이</h2>

```java
import java.util.*;
import java.util.Map.Entry;

class Solution {
	static Map<String, Integer> map;

	public String[] solution(String[] orders, int[] course) {
		StringBuilder sb;
		map = new HashMap<>();
		// 입력값 정렬
		for (int i = 0; i < orders.length; i++) {
			char[] order = orders[i].toCharArray();
			Arrays.sort(order);

			comb(new boolean[order.length], order, "", 0);
		}

		int[] count = new int[11]; // course 배열의 각 원소는 2 이상 10이하인 자연수 이므로
		ArrayList<Entry<String, Integer>> list = new ArrayList<>(map.entrySet());

		for (Entry<String, Integer> e : list) {
			int len = e.getKey().length();
			count[len] = Math.max(count[len], e.getValue());
		}

		boolean[] check = new boolean[11];
		for (int i = 0; i < course.length; i++) {
			check[course[i]] = true;
		}

		ArrayList<String> result = new ArrayList<>();
		for (Entry<String, Integer> e : list) {
			int len = e.getKey().length();
			if (check[len] && count[len] >= 2 && count[len] == e.getValue())
				result.add(e.getKey());
		}

		String[] answer = new String[result.size()];
		for (int i = 0; i < answer.length; i++) {
			answer[i] = result.get(i);
		}
		
		Arrays.sort(answer);
		return answer;
	}

	public static void comb(boolean[] used, char[] order, String result, int index) {
		if (result.length() >= 2) {
			map.put(result, map.getOrDefault(result, 0) + 1);

			if (index == used.length)
				return;
		}

		for (int i = index; i < used.length; i++) {
			if (!used[i]) {
				used[i] = true;
				comb(used, order, result + order[i], i + 1);
				used[i] = false;
			}
		}
	}
}
```


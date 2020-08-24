---
layout: post
title: "[programmers] 소수 찾기"
subtitle : level 2, 완전탐색 문제
tags: [algorithm]
author: Gaeun Kim
comments : True
---

<h2>문제</h2>

[programmers 소수 찾기 문제 보러가기](https://programmers.co.kr/learn/courses/30/lessons/42839)

<br><br>

<h2>풀이</h2>

```java
import java.util.*;

public class Solution {
	static HashMap<Integer, Integer> map;
	static boolean[] used;
	static int[] arr;

	public int solution(String numbers) {
		arr = new int[numbers.length()];
		used = new boolean[numbers.length()];
		map = new HashMap<Integer, Integer>();

		for (int i = 0; i < arr.length; i++)
			arr[i] = numbers.charAt(i) - '0';

		combination("");
		System.out.println(map);
		return map.size();
	}

	static void combination(String num) {
		if (!num.equals("") || !num.isBlank()) {
			int tmp = Integer.parseInt(num);
			boolean flag = false;

			if (tmp != 0 && tmp != 1) {
				for (int i = 2; i < tmp - 1; i++) {
					if (tmp % i == 0) {
						flag = true;
						break;
					}
				}

				if (!flag)
					map.put(tmp, map.getOrDefault(tmp, 0) + 1);
			}
		}

		for (int i = 0; i < used.length; i++) {
			if (!used[i]) {
				used[i] = true;
				combination(num + arr[i]);
				used[i] = false;
			}
		}
	}
}
```

<br><br>

<h2>문제점 해결</h2>

시간초과 나는 테스트 케이스가 있었는데 한 줄 추가로 이를 해결했다.

```java
for (int i = 2; i < tmp - 1; i++) {
    if (tmp % i == 0) {
        flag = true;
    }
}
```

기존의 코드인데 `flag`는 소수인지 아닌지를 판단하기 위한 `boolean`변수로 이미 소수가 아님을 확인했는데 `break`문이 없기 때문에 계속 반복 작업을 하게되고, 큰 숫자일때 시간초과가 발생한것이라 예상했다.

```java
for (int i = 2; i < tmp - 1; i++) {
    if (tmp % i == 0) {
        flag = true;
        break;
    }
}
```

위처럼 `break`문 하나만 추가하니 바로 통과되었다.
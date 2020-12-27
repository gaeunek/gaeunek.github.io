---
layout: post
title: "[programmers] 불량 사용자"
subtitle : level 3, 2018 KAKAO BLIND RECRUITMENT 문제
tags: [algorithm]
author: Gaeun Kim
comments : True
---

<h2>문제</h2>

[programmers 불량 사용자 문제 보러가기](https://programmers.co.kr/learn/courses/30/lessons/64064)

<br><br>

<h2>문제점 해결</h2>

해당 문제는 경우의 수를 구하는 문제이기 때문에 제재 당하는 사용자의 아이디가 중복되면 안된다는 부분을 주의해야 한다.

첫번째 제재 아이디부터 일치하는 사용자 아이디가 있는지 확인하였고, 있다면 다음 제재 아이디를 확인하는 방식으로 DFS를 사용해 풀었고, 제재 아이디 조합을 구했으면 이를 List에 담아 정렬하여 중복 제거가 되는 Set에 저장하였다. 처음엔 DFS만을 사용해서 조합을 구하면서 answer값을 증가시켰는데, `{frodo, crodo, abc123, frodoc}`와 `{frodo, crodo, frodoc, abc123}`에 속한 원소값은 같으나 순서가 다르기에 서로 다른 값으로 여겨 원하는 answer값이 나오지 않는 오류가 있어 Set을 사용하게 되었다.

<br><br>

<h2>풀이</h2>

```java
import java.util.*;

class Solution {
	static boolean[] used;
	static Set<List<String>> set;

	public int solution(String[] user_id, String[] banned_id) {
		used = new boolean[user_id.length];
		set = new HashSet<>();
		dfs(user_id, banned_id, 0);
		System.out.println(set);
		return set.size();
	}

	static void dfs(String[] user_id, String[] banned_id, int idx) {
		if (idx == banned_id.length) {
			List<String> result = new ArrayList<>();
			for(int i = 0; i < used.length; i++) {
				if(used[i]) result.add(user_id[i]);
			}
			Collections.sort(result);
			set.add(result);
			return;
		}

		for (int i = 0; i < user_id.length; i++) {
			if (used[i])
				continue;

			String u_id = user_id[i];
			String b_id = banned_id[idx];

			if (b_id.length() != u_id.length())
				continue;

			int same = 0;
			for (int j = 0; j < b_id.length(); j++) {
				if (u_id.charAt(j) == b_id.charAt(j) || b_id.charAt(j) == '*')
					same++;
			}

			if (same == b_id.length()) {
				used[i] = true;
				dfs(user_id, banned_id, idx+1);
				used[i] = false;
			}
		}
	}
}
```


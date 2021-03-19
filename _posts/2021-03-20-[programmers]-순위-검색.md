---
layout: post
title: "[programmers] 순위 검색"
subtitle : level 2, 2021 KAKAO BLIND RECRUITMENT 문제
tags: [algorithm]
author: Gaeun Kim
comments : True
---

<h2>문제</h2>

[programmers 순위 검색 문제 보러가기](https://programmers.co.kr/learn/courses/30/lessons/72412)

<br><br>

<h2>문제점 해결</h2>

카카오 문제는 언제 풀어도 어렵다 ..ㅠㅠ 하지만 포기하자니 아쉽고 결국 문제 해설을 참고하여 풀었다.

[2021 카카오 신입공채 1차 온라인 코딩 테스트 for Tech developers 문제해설](https://tech.kakao.com/2021/01/25/2021-kakao-recruitment-round-1/)

<br><br>

<h2>풀이</h2>

```java
import java.util.*;

class Solution {
	static Map<ArrayList<String>, ArrayList<Integer>> map;

	public int[] solution(String[] info, String[] query) {
		int[] answer = new int[query.length];

		map = new HashMap<>();
		String[][] row = new String[info.length][5];

		for (int i = 0; i < info.length; i++) {
			row[i] = info[i].split(" ");
		}

		Arrays.sort(row, new Comparator<String[]>() {

			@Override
			public int compare(String[] o1, String[] o2) {
				return Integer.parseInt(o1[4]) - Integer.parseInt(o2[4]);
			}
		});

		for (int i = 0; i < info.length; i++) {
			boolean[] used = new boolean[5];
			comb(row[i], used, 0);
		}

		ArrayList<String> tmp = new ArrayList<>();

		for (int i = 0; i < query.length; i++) {
			String[] q = query[i].split(" ");

			for (int j = 0; j < q.length; j += 2) {
				tmp.add(q[j]);
			}

			int num = Integer.parseInt(q[7]);

			ArrayList<Integer> result = map.get(tmp);

			if (result != null) {
				int st = 0, end = result.size() - 1;

				while (st <= end) {
					int mid = (st + end) / 2;

					if (result.get(mid) < num) {
						st = mid + 1;
					} else {
						end = mid - 1;
					}
				}

				answer[i] = result.size() - st;
			}

			tmp.clear();
		}

		return answer;
	}

	static void comb(String[] row, boolean[] used, int index) {
		ArrayList<String> list = new ArrayList<>();

		for (int i = 0; i < used.length - 1; i++) {
			if (used[i]) {
				list.add(row[i]);
			} else {
				list.add("-");
			}
		}

		map.computeIfAbsent(list, key -> new ArrayList<>()).add(Integer.parseInt(row[4]));

		if (index == used.length)
			return;

		for (int i = index; i < used.length - 1; i++) {
			if (!used[i]) {
				used[i] = true;
				comb(row, used, i + 1);
				used[i] = false;
			}
		}
	}
}
```


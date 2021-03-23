---
layout: post
title: "[programmers] 외벽 점검"
subtitle : level 3, 2020 KAKAO BLIND RECRUITMENT 문제
tags: [algorithm]
author: Gaeun Kim
comments : True
---

<h2>문제</h2>

[programmers 외벽 점검 문제 보러가기](https://programmers.co.kr/learn/courses/30/lessons/60062)

<br><br>

<h2>문제점 해결</h2>

해당 문제는 몇시간을 생각해도 좋은 아이디어가 떠오르지 않아 블로그를 참고하여 순열과 조합으로 풀 수 있었다. ㅠㅠ 카카오 코딩테스트 문제는 언제 풀어도 참 오랜시간이 걸리고 많은 생각이 필요한 문제라고 생각한다.

<br><br>

<h2>풀이</h2>

```java
import java.util.*;

class Solution {
	static Set<ArrayList<Integer>> set;
	static ArrayList<String> list;

	public int solution(int n, int[] weak, int[] dist) {
		int answer = Integer.MAX_VALUE;

		int[] circleWeak = new int[weak.length * 2 - 1];

		for (int i = 0; i < weak.length; i++) {
			circleWeak[i] = weak[i];
		}

		int index = 0;
		int a = weak[weak.length - 1], b = weak[index];

		for (int i = weak.length; i < circleWeak.length; i++) {
			int c = b - a < 0 ? n + b - a : b - a;
			circleWeak[i] = circleWeak[i - 1] + c;
			a = b;
			b = weak[++index];
		}

		ArrayList<int[]> circle = new ArrayList<>();
		for (int i = 0; i < weak.length; i++) {
			index = 0;
			int[] temp = new int[weak.length];
			for (int j = i; j < i + weak.length; j++) {
				temp[index++] = circleWeak[j];
			}
			
			circle.add(temp);
		}
		
		set = new HashSet<>();
		list = new ArrayList<>();
		boolean[] used = new boolean[dist.length];
		comb(dist, used);
		
		ArrayList<ArrayList<Integer>> set_to_list = new ArrayList<>(set);
		
		for (int i = 0; i < circle.size(); i++) {
			for (int j = 0; j < set_to_list.size(); j++) {
				int result = check(set_to_list.get(j), circle.get(i));
				
				if(result != -1) {
					answer = Math.min(answer, result);
				}
			}
		}

		return answer == Integer.MAX_VALUE ? -1 : answer;
	}
	
	static public int check(ArrayList<Integer> dist_list, int[] circle) {
		int index = 0, point = 0, suc = 0; // 보낸 친구 값, 취약 지점 점검 후의 지점
		
		for (int i = 0; i < circle.length; i++) {
			point = circle[i];
			if((suc == 0 || point > suc) && index < dist_list.size()) {
				suc = point + dist_list.get(index++);
			}
		}
		
		return point <= suc ? index : -1;
	}

	static public void comb(int[] dist, boolean[] used) {
		if (list.size() != 0) {
			ArrayList<Integer> tmp = new ArrayList<>();
			for (String s : list) {
				int num = Integer.parseInt(s);
				tmp.add(num);
			}

			set.add(tmp);
		}

		for (int i = 0; i < dist.length; i++) {
			if (!used[i]) {
				String s = String.valueOf(dist[i]);
				used[i] = true;
				list.add(s);
				comb(dist, used);
				list.remove(s);
				used[i] = false;
			}
		}
	}
}
```


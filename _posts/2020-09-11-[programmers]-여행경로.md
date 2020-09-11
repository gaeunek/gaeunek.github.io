---
layout: post
title: "[programmers] 여행경로"
subtitle : level 3, 깊이/너비 우선 탐색(DFS/BFS) 문제
tags: [algorithm]
author: Gaeun Kim
comments : True
---

<h2>문제</h2>

[programmers 여행경로 문제 보러가기](https://programmers.co.kr/learn/courses/30/lessons/43164?language=java)

<br><br>

<h2>풀이</h2>

```java
import java.util.*;

class Solution {
	static boolean[] used;
	static ArrayList<String> result;
	static ArrayList<String> before;
	
	public String[] solution(String[][] tickets) {
		Arrays.sort(tickets, new Comparator<String[]>() {

			public int compare(String[] o1, String[] o2) {
				if (o1[0].equals("ICN") && o2[0].equals("ICN"))
					return o1[1].compareTo(o2[1]);
				else if (o1[0].equals("ICN"))
					return -1;
				else if (o2[0].equals("ICN"))
					return 1;
				else
					return o1[0].compareTo(o2[0]);
			}
		});
		result = new ArrayList<String>();
		before = new ArrayList<String>();
		used = new boolean[tickets.length];
		result.add("ICN");
		dfs("ICN", tickets);
		
		String[] answer = new String[before.size()];
		
		for (int i = 0; i < answer.length; i++)
			answer[i] = before.get(i);
		return answer;
	}

	static void dfs(String ticket, String[][] tickets) {
		if (result.size() > tickets.length) {
			if(before.size() == 0) {
				for (int i = 0; i < result.size(); i++)
					before.add(result.get(i));
			} else {
				for (int i = 0; i < result.size(); i++) {
					if(result.get(i).compareTo(before.get(i)) == -1) {
						before.clear();
						for (int j = 0; j < result.size(); j++)
							before.add(result.get(j));
					}
				}
			}
			
			return;
		}

		for (int i = 0; i < tickets.length; i++) {
			if (!used[i] && tickets[i][0].equals(ticket)) {
				used[i] = true;
				result.add(tickets[i][1]);
				dfs(tickets[i][1], tickets);
				used[i] = false;
				result.remove(tickets[i][1]);
			}
		}
	}
}
```

100점 만점에 75점으로 다시 풀이해봐야겠다 ㅠㅠ
---
layout: post
title: "[programmers] 여행경로(풀이 완료)"
subtitle : level 3, 깊이/너비 우선 탐색(DFS/BFS) 문제
tags: [algorithm]
author: Gaeun Kim
comments : True
---

<h2>문제</h2>

[programmers 여행경로 문제 보러가기](https://programmers.co.kr/learn/courses/30/lessons/43164?language=java)

<br><br>

<h2>문제점 해결</h2>

9월에 이 문제를 풀었을 땐 100점 만점에 75점으로 패스하지 못했는데 이번엔 통과했다.

1번 테스트케이스가 통과되지 못한 이유는 List에 경로를 저장하고 제거하는 과정에서 발생했다. 같은 이름을 가진 티켓으로 인해 원하는 티켓을 지우지 못하고 엉뚱한 티켓을 지우다 보니 "ICN"부터 시작해야하는 결과가 이상한 티켓부터 시작해 발견할 수 있는 오류였다.

질문하기 목록에 나와있는 반례란 반례는 모두 시도하던 중 발견한 이 반례 덕분에 오류를 발견할 수 있었다. (반례 작성해주신분 정말 감사합니다. ㅠㅠ)

[참고 반례](https://programmers.co.kr/questions/10332)`{{"ICN","BOO" }, { "ICN", "COO" }, { "COO", "DOO" }, {"DOO", "COO"}, { "BOO", "DOO"} ,{"DOO", "BOO"}, {"BOO", "ICN" }, {"COO", "BOO"}}`

<br><br>

<h2>풀이</h2>

```java
import java.util.*;

class Solution {
	static boolean[] used;
	static List<String> result, list;
	static int N;
	
	public String[] solution(String[][] tickets) {
        N = tickets.length;
        used = new boolean[N];
        list = new ArrayList<>();
        result = new ArrayList<>();
        
        dfs(tickets, "ICN");
        
        String[] answer = new String[N+1];
        for (int i = 0; i <= N; i++) {
        	answer[i] = result.get(i);
		}
        
        return answer;
    }
	
	static void dfs(String[][] tickets, String now) {
		if(list.size() == N) {
			List<String> tmp = new ArrayList<>();
			for (int i = 0; i < N; i++) {
				int idx = Integer.parseInt(list.get(i));
				tmp.add(tickets[idx][0]);
				
				if(i == N-1) tmp.add(tickets[idx][1]);
			}
			
			if(result.size() == 0) result = tmp;
			else {
				boolean flag = false;
				for (int i = 1; i <= N; i++) {
					int v = result.get(i).compareTo(tmp.get(i));
					
					if(v > 0) {
						flag = true;
						break;
					} else if(v < 0) break;
				}
				
				if(flag) result = tmp;
			}
			
			return;
		}
		
		for (int i = 0; i < N; i++) {
			String next = tickets[i][1];
			
			if(!used[i] && tickets[i][0].equals(now)) {
				String value = String.valueOf(i);
				list.add(value);
				used[i] = true;
				dfs(tickets, next);
				used[i] = false;
				list.remove(value);
			}
		}
	}
}
```




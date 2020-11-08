---
layout: post
title: "[programmers] [3차]파일명 정렬"
subtitle : level 2, 2018 KAKAO BLIND RECRUITMENT 문제
tags: [algorithm]
author: Gaeun Kim
comments : True
---

<h2>문제</h2>

[programmers [3차]파일명 정렬 문제 보러가기](https://programmers.co.kr/learn/courses/30/lessons/17686)

<br><br>

<h2>문제점 해결</h2>

HEAD와 NUMBER부분을 분리해 File이란 객체로 만든 후 HEAD > NUMBER  > INDEX 우선수위를 정해 정렬시켰다.

<br><br>

<h2>풀이</h2>

```java
import java.util.*;
class Solution {
	public String[] solution(String[] files) {
		String[] answer = new String[files.length];
		File[] result = new File[files.length];

		String s = "";
		for (int i = 0; i < files.length; i++) {
			String tmp = files[i];
			boolean flag = false, numCheck = false;
			String num = "0";

			for (int j = 0; j < tmp.length(); j++) {
				char c = tmp.charAt(j);
				if (c == '0' && !flag) {
					numCheck = true;
					continue;
				} else if (c >= '0' && c <= '9') {
					flag = true;
					num += c;
					continue;
				}

				if ((j != 0 && flag) || numCheck)
					break;
				if (!flag)
					s += c;
			}

			result[i] = new File(i, s.toLowerCase(), Integer.parseInt(num));
			s = "";
		}
		
		Arrays.sort(result, new Comparator<File>() {
			
			@Override
			public int compare(File o1, File o2) {
				if(o1.head.compareTo(o2.head) == 0) {
					if(o1.num == o2.num) return o1.index - o2.index;
					return o1.num - o2.num;
				}
				return o1.head.compareTo(o2.head);
			}
		});

		for (int i = 0; i < answer.length; i++) {
			answer[i] = files[result[i].index];
		}

		return answer;
	}

	static class File {
		String head;
		int index, num;

		public File(int index, String head, int num) {
			this.index = index;
			this.head = head;
			this.num = num;
		}
	}
}
```




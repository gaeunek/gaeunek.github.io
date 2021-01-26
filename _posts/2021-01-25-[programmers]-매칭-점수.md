---
layout: post
title: "[programmers] 매칭 점수"
subtitle : level 3, 2019 KAKAO BLIND RECRUITMENT 문제
tags: [algorithm]
author: Gaeun Kim
comments : True
---

<h2>문제</h2>

[programmers 매칭 점수 문제 보러가기](https://programmers.co.kr/learn/courses/30/lessons/42893)

<br><br>

<h2>문제점 해결</h2>

#### 1. 웹페이지의 url 추출

각 웹페이지마다 url이 있고, 다른 웹페이지에서 외부링크로 참조할 수 있다. 링크 점수를 계산하기 위해선 본인이 어느 웹페이지의 외부링크로 들어가 있는지 알 수 있어야 하기 때문에 url과 index번호를 Map에 저장한 후 참조하고 있는 웹페이지(Page class)의 connect_link_list에 index번호를 저장해준다.

```java
for (int i = 0; i < len; i++) {
    pages[i] = pages[i].toLowerCase();
	meta_filter = pages[i].split("<meta property=\"og:url\" ");
	url_filter = meta_filter[1].split("content=\"");
	url_filter[1] = url_filter[1].replaceAll("\n", "");

	String url = url_filter[1];
	int j = 0;
	for (j = 1; j < url.length(); j++) {
		if (url.charAt(j) == '\"')
			break;
	}

	url_list.put(url.substring(0, j), i);
	result[i] = new Page(0, 0, 0);
}
```

여기서 **주의할 점**은 url을 추출하는 부분이다. 처음에 난 "\</head\>"를 기준으로 1차 split 한 뒤, "content="를 기준으로 2차 split을 하여 url을 추출하였는데, 9번 테스트케이스가 통과가 안되서 위와 같이 바꿨더니 통과되었다. "<meta property=\"og:url\""가 핵심이었던 것같다.

#### 2. 외부링크 추출

각 웹페이지에서 참고하는 외부링크는 <a href= ... \</a\>와 같은 형태로 존재하기에 처음엔 공백을 기준으로 split한 후 "href="를 기준으로 split 했는데, 10번 테스트케이스가 통과가 안되서 아래와 같이 수정 한 후 통과되었다.

```java
String[] tmp = words.split("<a href=\"");
for (int k = 0; k < tmp.length; k++) {
    if (tmp[k].contains("</a>")) {
		link_count++;
		String s = tmp[k];

		int c = 0;
		for (c = 0; c < s.length(); c++) {
			if (s.charAt(c) == '\"')
				break;
		}

		String key = s.substring(0, c);
						
		if (url_list.get(key) != null)
			result[url_list.get(key)].connect_link_list.add(i);
	}
}
```

<br><br>

<h2>풀이</h2>

```java
import java.util.*;

class Solution {
	public int solution(String word, String[] pages) {
		int len = pages.length;
		word = word.toLowerCase();

		Map<String, Integer> url_list = new HashMap<>();
		Page[] result = new Page[len];

		String[] meta_filter, url_filter, body_filter, end_body_filter, words_list, find_word;
		String match = "[^a-z]";

		/* url과 index를 저장해준다. */
		for (int i = 0; i < len; i++) {
			pages[i] = pages[i].toLowerCase();
			meta_filter = pages[i].split("<meta property=\"og:url\" ");
			url_filter = meta_filter[1].split("content=\"");
			url_filter[1] = url_filter[1].replaceAll("\n", "");

			String url = url_filter[1];
			int j = 0;
			for (j = 1; j < url.length(); j++) {
				if (url.charAt(j) == '\"')
					break;
			}

			url_list.put(url.substring(0, j), i);
			result[i] = new Page(0, 0, 0);
		}

		for (int i = 0; i < len; i++) {
			body_filter = pages[i].split("<body>");
			end_body_filter = body_filter[1].split("</body>");
			words_list = end_body_filter[0].split("\n");

			int link_count = 0, word_count = 0;
			for (int j = 0; j < words_list.length; j++) {
				String words = words_list[j];

				/* 외부 링크 연결 */
				String[] tmp = words.split("<a href=\"");
				for (int k = 0; k < tmp.length; k++) {
					if (tmp[k].contains("</a>")) {
						link_count++;
						String s = tmp[k];

						int c = 0;
						for (c = 0; c < s.length(); c++) {
							if (s.charAt(c) == '\"')
								break;
						}

						String key = s.substring(0, c);
						
						if (url_list.get(key) != null)
							result[url_list.get(key)].connect_link_list.add(i);
					}
				}

				/* word 카운트 */
				words = words.replaceAll(match, " ");
				if (words.contains(word)) {
					find_word = words.split(" ");

					for (int k = 0; k < find_word.length; k++) {
						if (find_word[k].equals(word))
							word_count++;
					}
				}
			}

			result[i].basic_score = word_count;
			result[i].link_count = link_count;
			result[i].link_score = (double) word_count / link_count;
		}

		double max = 0;
		int max_index = 0;
		for (int i = 0; i < len; i++) {
			Page p = result[i];

			double sum = p.basic_score;
			for (int j = 0; j < p.connect_link_list.size(); j++) {
				int index = p.connect_link_list.get(j);
				sum += result[index].link_score;
			}

			if (max < sum) {
				max = sum;
				max_index = i;
			}
		}

		return max_index;
	}

	static class Page {
		int basic_score, link_count;
		double link_score;
		List<Integer> connect_link_list;

		public Page(int b, int lc, double ls) {
			this.basic_score = b;
			this.link_count = lc;
			this.link_score = ls;
			this.connect_link_list = new ArrayList<>();
		}
	}
}
```


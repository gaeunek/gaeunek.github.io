---
layout: post
title: "[programmers] 완주하지 못한 선수"
subtitle : level 1
tags: [algorithm]
author: Gaeun Kim
comments : True
---

<br>

### 문제

[programmers 완주하지 못한 선수 문제 보러가기](https://programmers.co.kr/learn/courses/30/lessons/42576)

<br><br>

### 풀이

```java
import java.util.Arrays;

public class Solution {

	public String solution(String[] participant, String[] completion) {
		Arrays.sort(participant);
		Arrays.sort(completion);

		int i;
		for (i = 0; i < completion.length; i++) {
			if (!participant[i].equals(completion[i]))
				return participant[i];
		}
		return participant[i];
	}
}

```

<br>

<br>

### 문제점 해결

처음엔 이중for문을 사용했지만 효율성검사에서 모두 탈락했고, 질문하기에 올라온 글 들을 통해 Hash만이 답이라고 생각했지만 결국 sort 하나로 풀었다.

문제를 푼 다음엔 다른 사람들의 풀이를 볼 수 있는데 Hash로 푼 방법을 참고해서 공부를 해야겠다.
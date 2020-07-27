---
layout: post
title: "[programmers] 가장 큰 수"
subtitle : level 2, 정렬문제
tags: [algorithm]
author: Gaeun Kim
comments : True
---

<h2>문제</h2>

[programmers 가장 큰 수 문제 보러가기](https://programmers.co.kr/learn/courses/30/lessons/42746)

<br><br>

<h2>풀이</h2>

```java
import java.util.*;

public class Solution {
	public String solution(int[] numbers) {
		String answer = "";
		ArrayList<Integer> list = new ArrayList<Integer>();

		for (int i : numbers)
			list.add(i);

		Collections.sort(list, new Comparator<Integer>() {
			@Override
			public int compare(Integer o1, Integer o2) {
				String tmp1 = String.valueOf(o1), tmp2 = String.valueOf(o2);

				if (Integer.parseInt(tmp1 + tmp2) > Integer.parseInt(tmp2 + tmp1))
					return -1;
				else if (Integer.parseInt(tmp1 + tmp2) < Integer.parseInt(tmp2 + tmp1))
					return 1;
				else
					return 0;
			}
		});

		int num = 0;
		for (int i : list) {
			num += i;
			answer += i;
		}
		
		if (num == 0)
			return "0";
		
		return answer;
	}
}

```

<br><br>

<h2>문제점 해결</h2>

combination알고리즘을 이용해 풀었지만 TC모두 실패 또는 시간초과라는 결과가 나왔고 문제에 나와있듯이 정렬을 사용해 풀어야하는 문제였다.

```java
String tmp1 = String.valueOf(o1), tmp2 = String.valueOf(o2);

if (Integer.parseInt(tmp1 + tmp2) > Integer.parseInt(tmp2 + tmp1))
    return -1;
else if (Integer.parseInt(tmp1 + tmp2) < Integer.parseInt(tmp2 + tmp1))
    return 1;
else
	return 0;
```

`{6, 10, 2}`라는 입력이 주어졌을 때, `Comparator<>`를 사용해서 `{6, 10}`을 비교한다. `610`이라는 숫자가 더 큰지, `106`이라는 숫자가 더 큰지 비교한다.

#### compare() 메서드

- 1번 parameter(o1) < 2번 parameter(o2) : return -1
- o1 == o2 : return 0
- o1 > o2 : return 1

위의 코드에서 봤을 때, `610`이 `106`보다 큰 숫자이다. `6`은 1번 parameter이고 `10`은 2번 이므로 o1 < o2 에 성립하기 때문에 -1을 return 해준다.

**이 문제의 함정**은 return 타입이 String이라는 부분이다. 만약 `{0, 0, 0}`이라는 입력이 들어오면  String 타입의 answer에 결과값을 하나 씩 붙여 `000`이라는 문자열이 나온다. 올바른 답은 `0`이기 때문에 한 번 더 int형으로 변환하여 값이 `0`인지 확인해주는 if문을 추가했다.
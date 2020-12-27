---
layout: post
title: "[programmers] 보석 쇼핑"
subtitle : level 3, 2020 카카오 인턴십 문제
tags: [algorithm]
author: Gaeun Kim
comments : True
---

<h2>문제</h2>

[programmers 보석 쇼핑 문제 보러가기](https://programmers.co.kr/learn/courses/30/lessons/67258?language=java)

<br><br>

<h2>문제점 해결</h2>

모든 보석을 하나 이상 포함하는 가장 짧은 구간을 찾는 문제로, 가장 짧은 구간이 여러 개라면 "시작 진열대 번호"가 가장 작은 구간을 return 한다.

<br>

#### 1. Set을 사용해 보석 종류를 찾는다.

Set은 중복된 값은 1개만 저장하는 특징이 있기 때문에 gems 배열을 set에 저장해 보석의 종류가 몇가지인지 찾는다.

```java
Set<String> set = new HashSet<>();
for (String str : gems) {
	set.add(str);
}
```

<br>

#### 2. Set의 크기가 1 이다. (= 보석의 종류가 1개 이다.)

보석의 종류가 하나일 때, {1, 1}을 return 해준다.(시작 진열대 번호가 가장 작은 구간을 return 해야하기 때문이다.)

<br>

#### 3. Map과 List를 사용해 구간을 찾는다.

처음엔 Map을 사용해 이미 Map에 저장된 key라면 value값을 갱신하여 map의 크기가 set의 크기(모든 보석의 종류)와 같아지면 map의 value값을 기준으로 정렬해 가장 작은값을 시작 진열대로, 큰값을 끝 진열대로 return 하였다. 결과는 FAIL. 그 이유는 `{"DIA", "EM", "EM", "RUB", "DIA"}`과 같은 입력이 들어올 때, output이 [1, 4]가 되지만 올바른 답은 [3, 5]이다.

유의해야하는 부분은 **gems 배열의 모든 값을 확인해야 하며, 그 중 가장 짧은 구간을 찾는 것**이다.

<br>

map에 보석과 진열대 번호를 저장할 때, list에도 진열대 번호를 저장해준다. 만약, map에 이미 보석이 저장되어 있다면 기존에 저장된 진열대 값은 지우고 현재의 새로운 진열대 값을 저장해준 후 map의 값도 갱신해준다.

```java
if(!map.containsKey(gems[i])) {
    list.add(String.valueOf(i+1));
} else {
	String str = String.valueOf(map.get(gems[i]));
	list.remove(str);
	list.add(String.valueOf(i+1));
}

map.put(gems[i], i+1);
```

map의 크기가 set의 크기(모든 보석의 종류)와 같아지면 list에서 0번째 값(st)과 마지막 값(end)을 찾는다. answer값으로 저장된 구간의 크기(size)보다 현재 구간이 더 짧으면서 모든 보석을 구매할 수 있는 구간이라면 answer값과 size를 갱신해준다.

```java
if(map.size() == set.size()) {
	int st = Integer.parseInt(list.get(0));
	int end = Integer.parseInt(list.get(list.size()-1));
				
	if(size == 0 || size > end - st) {
			size = end - st;
			answer[0] = st;
			answer[1] = end;
	}
}
```

<br><br>

<h2>풀이</h2>

```java
import java.util.*;

class Solution {
	public int[] solution(String[] gems) {
		int[] answer = new int[2];
		
		Set<String> set = new HashSet<>();
		for (String str : gems) {
			set.add(str);
		}
		
		if(set.size() == 1) return new int[] {1, 1};
		
		Map<String, Integer> map = new HashMap<>();
		List<String> list = new ArrayList<>();
		int size = 0;
		
		for (int i = 0; i < gems.length; i++) {
			if(!map.containsKey(gems[i])) {
				list.add(String.valueOf(i+1));
			} else {
				String str = String.valueOf(map.get(gems[i]));
				list.remove(str);
				list.add(String.valueOf(i+1));
			}
			
			map.put(gems[i], i+1);
			
			if(map.size() == set.size()) {
				int st = Integer.parseInt(list.get(0));
				int end = Integer.parseInt(list.get(list.size()-1));
				
				if(size == 0 || size > end - st) {
					size = end - st;
					answer[0] = st;
					answer[1] = end;
				}
			}
		}
		
		return answer;
	}
}
```


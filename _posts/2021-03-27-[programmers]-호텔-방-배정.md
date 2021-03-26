---
layout: post
title: "[programmers] 호텔 방 배정"
subtitle : level 4, 2019 카카오 개발자 겨울 인턴십 문제
tags: [algorithm]
author: Gaeun Kim
comments : True
---

<h2>문제</h2>

[programmers 호텔 방 배정 문제 보러가기](https://programmers.co.kr/learn/courses/30/lessons/64063)

<br><br>

<h2>문제점 해결</h2>

처음엔 Map을 사용해 key는 배정 방번호로 value에는 인덱스 값을 저장해 value로 정렬 후 key값을 출력했는데 시간초과가 발생했다. 여기에 이분탐색을 추가했지만 결과는 실패 ㅠㅠ 결국 서치를 통해 풀이법을 확인하였고 Map을 이용해 Memoization으로 풀어야 하는 문제임을 깨달았다. 이때 Map에 저장하는 값은 key가 배정된 방번호 이고 value가 다음 방 번호이다.

> [참고 블로그](https://bcp0109.tistory.com/entry/Kakao-2019-Internship-Winter-%ED%98%B8%ED%85%94-%EB%B0%A9-%EB%B0%B0%EC%A0%95-Java)

<br><br>

<h2>풀이</h2>

```java
import java.util.*;

class Solution{
	static Map<Long, Long> map;
	public long[] solution(long k, long[] room_number) {
		long[] answer = new long[room_number.length];

		map = new HashMap<>();
		
		for (int i = 0; i < room_number.length; i++) {
			answer[i] = findRoomNumber(room_number[i]);
		}
		
		return answer;
	}
	
	static long findRoomNumber(long number) {
		if(!map.containsKey(number)) {
			map.put(number, number + 1);
			return number;
		}
		
		long nextRoom = map.get(number);
		long newRoom = findRoomNumber(nextRoom);
		map.put(number, newRoom);
		return newRoom;
	}
}
```


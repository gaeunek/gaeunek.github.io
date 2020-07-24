---
layout: post
title: "[programmers] 전화번호 목록"
subtitle : level 2, 해시문제
tags: [algorithm]
author: Gaeun Kim
comments : True
---

<h2>문제</h2>

[programmers 전화번호 목록 문제 보러가기](https://programmers.co.kr/learn/courses/30/lessons/42577)

<br>

<br>

<h2>풀이</h2>

```java
public boolean solution(String[] phone_book) {
		boolean answer = true;
		Arrays.sort(phone_book);
		for (int i = 0; i < phone_book.length-1; i++) {
			if(phone_book[i+1].startsWith(phone_book[i]))
				return false;
		}
		return answer;
	}
```

<br>

<br>

<h2>문제점 해결</h2>

항상 느끼는거지만 **문제를 잘 읽는 습관**을 가져야겠다.

문제의 핵심은 바로 **접두어**인데 접두어란 단어의 앞 부분에만 붙기 때문에 앞에서 일치하는 숫자가 나오면 뒤에나오는 숫자는 볼 필요가 없다는 뜻이다.

테스트 케이스(TC)로 주어진 `{"119", "97674223", "1195524421"}`를 알 수 있듯이 앞의 119가 다른 문자열 앞부분에 붙어있다면 `false`가 정답이 되는것이다.

<br>

#### 비교 기준이 되는 문자열은 단 하나?

문제를 잘 읽어야겠다라고 느낀건 바로 이 문제점에 직면한 상황에서였다.

문제에서 `한 번호가 다른 번호의 접두어`인 경우 `false`를 출력하라고 나와있는데 나는 무조건 맨 앞에 있는 문자열이 기준이 된다고 생각했다. 하지만 잘 생각해보면 0번 인덱스가 1번 인덱스의 접두어는 아니지만 1번이 2번의 접두어가 될 수 있다. 이건 무조건 **0번 인덱스의 값이 기준**이 아님을 보여주고 있다.

<br>

#### 정렬

위에서 이야기했듯이 0번 인덱스가 1번 인덱스의 접두어가 될 순 없지만 0번이 3번 인덱스의 접두어인 경우가 있다면? 이런 문제를 해결하기 위해 입력 배열 `phone_book`을 정렬 시키는 방법을 선택했다.

<br>

<h3>startWith()</h3>

문자열 함수 중에 이런 함수는 처음 사용해보았는데 해당 문제를 풀 때 정말 큰 역할을 했다.

`subString()`함수만 쓸 줄 알았는데 이런 함수가 있었다니..정말 편리함을 느꼈다!

`startsWith()`는 이름 그대로 비교 대상 문자 또는 문자열로 시작하는지 확인해서 `true`, `false`로 출력해주는 함수이다.

```java
		String str1 = "apple";
		String str2 = "112";
		String str3 = "사과바나나";
		String str4 = "사과 바나나";
		
		System.out.println(str1.startsWith("ap")); //true
		System.out.println(str2.startsWith("1")); //true
		System.err.println(str3.startsWith("사과바나")); //true
		System.out.println(str4.startsWith("바나나")); //false
```


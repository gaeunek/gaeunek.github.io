---
layout: post
title: "[programmers] 베스트앨범"
subtitle : level 2, 해시문제
tags: [algorithm]
author: Gaeun Kim
comments : True
---

<h2>문제</h2>

[programmers 베스트앨범 문제 보러가기](https://programmers.co.kr/learn/courses/30/lessons/42579)

<br><br>

<h2>풀이</h2>

```java
import java.util.*;
import java.util.Map.Entry;

class Solution {
   public int[] solution(String[] genres, int[] plays) {
		ArrayList<Music> list = new ArrayList<>();

		for (int i = 0; i < plays.length; i++) {
			list.add(new Music(genres[i], plays[i], i));
		}

		Collections.sort(list);
		
		// 장르 재생 수 계산
		HashMap<String, Integer> map = new HashMap<String, Integer>();
		for (int i = 0; i < genres.length; i++) {
			map.put(genres[i], map.getOrDefault(genres[i], 0) + plays[i]);
		}

		// 내림차순 정렬
		List<Entry<String, Integer>> list_entry = new ArrayList<Map.Entry<String, Integer>>(map.entrySet());
		Collections.sort(list_entry, new Comparator<Entry<String, Integer>>() {

			@Override
			public int compare(Entry<String, Integer> o1, Entry<String, Integer> o2) {
				return o2.getValue().compareTo(o1.getValue());
			}
		});

		ArrayList<Integer> result = new ArrayList<Integer>();
		
		for (int i = 0; i < list_entry.size(); i++) {
			String key = list_entry.get(i).getKey();
			
			int cnt = 0;
			
			for (Music m : list) {
				if (m.genre.equals(key)) {
					result.add(m.index);
					cnt++;
					if (cnt % 2 == 0)
						break;
				}
			}
		}
		
		int[] answer = new int[result.size()];
		
		for (int i = 0; i < answer.length; i++) {
			answer[i] = result.get(i);
		}
		
		return answer;
	}

	static public class Music implements Comparable<Music> {
		int play, index;
		String genre;

		public Music(String genre, int play, int index) {
			this.index = index;
			this.play = play;
			this.genre = genre;
		}

		@Override
		public int compareTo(Music o) {
			if(o.play == this.play)
				return this.index - o.index;
			else	
				return o.play - this.play;
		}
	} 		
}
```

<br><br>

<h2>문제점 해결</h2>

#### 1. Music 객체 만들기

genre, play, index로 구성된 Music객체를 하나씩 만들어 `ArrayList<>`에 저장

```java
ArrayList<Music> list = new ArrayList<>();

for (int i = 0; i < plays.length; i++) {
	list.add(new Music(genres[i], plays[i], i));
```

문제의 핵심은 **많이 재생된 장르를 먼저 수록**하고 **장르 내에서 많이 재생된 순서로 수록**, 여기서 주의할 점은 장르 내에서 재생된 순서가 같다면 **인덱스 번호가 낮은 순서로 수록**하는 것이다.

```java
@Override
public int compareTo(Music o) {
    if(o.play == this.play)
        return this.index - o.index; //index 오름차순
	else	
		return o.play - this.play; //play 내림차순
}
```

`Collections.sort(list);`를 사용해 **재생수가 가장 많은 순서**로 정렬하는데 여기서 재생수가 같을때, **인덱스 번호가 낮은 순서**대로 정렬되도록 `Compatable<>`을 사용했다.

<br>

#### 2. 장르의 총 재생수(Value)를 기준으로 내림차순 정렬

Map을 정렬하기 위해 List에 Entry타입으로 저장한 뒤, `Collections.sort()`를 사용했다.

```java
List<Entry<String, Integer>> list_entry = new ArrayList<Map.Entry<String, Integer>>(map.entrySet());
Collections.sort(list_entry, new Comparator<Entry<String, Integer>>() {

    @Override
    public int compare(Entry<String, Integer> o1, Entry<String, Integer> o2) {
        return o2.getValue().compareTo(o1.getValue());
    }
});
```

여기선 `Comparator<>`를 사용해 `map`에 저장된 `value`값을 기준으로 정렬했다.

<br>

<h3>Comparable과 Comparator의 차이점</h3>

**Comparable**은 구현 객체(여기선 Music)에게 부여하는 한 가지 기본 정렬 기준이 되는 메서드를 정의해 놓는 **인터페이스**이다.

**Comparator**는 기본 정렬 기준과 다른 정렬 방식을 지정하고 싶을 때 사용하는 **클래스**이다.

<br>

#### 3. 장르별로 두 개씩만

이 문제의 함정은 장르 별로 가장 많이 재생된 노래를 **두 개씩 모아** 베스트 앨범을 출시한다는 부분이다.

즉, TC에 나와있는 classic장르에서 2곡 pop장르에서 2곡 씩만 `answer`배열에 저장해야한다.

```java
//list_entry = [pop=3100, classic=1450]
for (int i = 0; i < list_entry.size(); i++) {
    String key = list_entry.get(i).getKey();
			
	int cnt = 0;
			
	for (Music m : list) {
        if (m.genre.equals(key)) {
            result.add(m.index);
			cnt++;
			if (cnt % 2 == 0)
                break;
		}
	}
}
```

`map`의 `value`값을 기준으로 내림차순 정렬된 데이터를 `list_entry`에 저장한다.

첫번째로 pop장르가 key에 저장되고 list에 저장된 Music객체들을 하나씩 꺼내어 객체에 저장된 genre값과 key값이 같은지 비교한다. list는 이미 play, index를 조건으로 정렬이 되어있기 때문에 장르만 같다면 가장 앞쪽에 저장된 객체의 play값이 그 장르에서 가장 재생 수가 높기때문에 해당 index값을 일단 result에 저장한다.

**여기서 answer배열에 바로 저장하지 않고 result라는 ArrayList<>를 만들어 저장한 이유는** 문제에 나와있듯이 한 장르의 최대 2곡만 answer에 저장하기 때문에 만약 pop장르에 곡이 1개 뿐이면 그 1개만 answer에 저장된다. 미리 한 장르당 2개씩이라 생각하고 answer배열 길이를 장르*2 크기로 지정해두면 1개의 곡만 가진 장르로 인해 채워지지 않은 배열에 쓰레기 값이 들어가게 된다.

때문에 이러한 과정을 거쳐 answer배열에 저장한것이다.
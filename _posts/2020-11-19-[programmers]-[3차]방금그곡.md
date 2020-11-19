---
layout: post
title: "[programmers] [3차]방금그곡"
subtitle : level 2, 2018 KAKAO BLIND RECRUITMENT 문제
tags: [algorithm]
author: Gaeun Kim
comments : True
---

<h2>문제</h2>

[programmers [3차]방금그곡 문제 보러가기](https://programmers.co.kr/learn/courses/30/lessons/17683)

<br><br>

<h2>문제점 해결</h2>

실패를 너무 많이 해서 구글링의 도움을 받아 해결한 문제이다.

#### 1. '#'이 붙은 음 변환하기

입력 예시에도 나와있듯이 "CC#B"와 같이 '#'이 붙은 음이 있는데 이 음은 소문자로 변경해줬다. 그 이유는 재생된 시간만큼 멜로디를 반복시킬 때 변환 없이 진행한다면 원래 3분짜리 멜로디인 "CC#B"가 4분으로 인식되어 제대로 된 답을 얻을 수 없다. 이때, 중요한 점은 **네오가 기억하는 멜로디 또한 이 과정을 거쳐줘야 한다.**

```java
static String edit(String s) {
    StringBuilder sb = new StringBuilder();
    for (int i = 0; i < s.length()-1; i++) {
		char c = s.charAt(i);
		char next = s.charAt(i+1);
		if(c == '#') continue;
		if(next == '#') sb.append(String.valueOf(c).toLowerCase());
		else sb.append(c);
	}
		
	if(s.charAt(s.length()-1) != '#') sb.append(s.charAt(s.length()-1));
		
	return new String(sb);
}
```

<br>

#### 2. 시간 계산

> 음악이 00:00를 넘겨서까지 재생되는 일은 없다.

TC중에 끝나는 시간이 "00:00"인 경우가 존재할 수 있기에 절댓값을 적용했다. 이 부분과 관련된 TC는 22, 23, 24이다.

```java
tmp = infos[0].split(":");
len = (Integer.parseInt(tmp[0])*60) + Integer.parseInt(tmp[1]);
tmp = infos[1].split(":");
len -= (Integer.parseInt(tmp[0])*60) + Integer.parseInt(tmp[1]);
len = Math.abs(len);
```

<br>

#### 3. 멜로디 길이 보다 재생된 시간이 적을 때

재생된 시간만큼 멜로디를 반복하는 과정을 진행하기 전에 **멜로디 길이 > 재생시간**일 경우엔 재생시간에 맞게 멜로디 길이를 잘라주었다. 이 부분을 거치지 않으면 TC 6, 7, 8, 9, 30번에서 실패가 뜬다.

```java
if(len > edit_melody.length()) {
    int trans_music_len = edit_melody.length();
	for (int j = 0; j < len - trans_music_len; j++) {
		edit_melody += edit_melody.charAt(j % trans_music_len);
	}
} else if(len < edit_melody.length()) {
    edit_melody = edit_melody.substring(0, len);
}
```

<br>

#### 4. 조건에 맞는 정렬(Sort)

> 조건이 일치하는 음악이 여러 개일 때에는 라디오에서 재생된 시간이 제일 긴 음악 제목을 반환한다. 재생된 시간도 같을 경우 먼저 입력된 음악 제목을 반환한다.

```java
Arrays.sort(music, new Comparator<MusicInfo>() {
	@Override
	public int compare(MusicInfo o1, MusicInfo o2) {
		if(o1.melody.length() == o2.melody.length())
			return o1.index - o2.index;
		return o2.melody.length() - o1.melody.length();
	}
});
```

<br>

#### 5. substring() 사용하기

처음엔 substring()을 사용해 **네오가 기억하는 멜로디 길이 > 재생된 멜로디 길이**일 경우엔 네오의 멜로디를 재생 시간에 맞게 잘라 일치하는지를 보았는데 시간 관련 TC(22, 23, 24)에서 실패가 떴다.

**재생된 멜로디 길이에 맞게 네오의 멜로디 길이를 자르면 실패가 뜬다.** 즉, 이 경우엔 그냥 "(None)"을 출력하는게 정답이라는 결론이 나왔다.

#### 6. contains() 사용하기

재생된 멜로디 안에 네오가 기억하는 멜로디가 있는지 확인(contains())를 사용해도 pass할 수 있다.

```java
if(melody.contains(m)) return musicInfo.title;
//or
for (int j = 0; j <= melody.length()-m.length(); j++) {
    String temp = melody.substring(j, j+m.length());
    if(temp.equals(m)) return musicInfo.title;
}
```

<br><br>

<h2>풀이</h2>

```java
import java.util.*;

class Solution {
	public String solution(String m, String[] musicinfos) {
		MusicInfo[] music = new MusicInfo[musicinfos.length];
		
		String[] tmp;
		int len = 0;
		/* 재생시간에 맞게 멜로디를 늘려준다. */
		for (int i = 0; i < musicinfos.length; i++) {
			String[] infos = musicinfos[i].split(",");
			tmp = infos[0].split(":");
			len = (Integer.parseInt(tmp[0])*60) + Integer.parseInt(tmp[1]);
			tmp = infos[1].split(":");
			len -= (Integer.parseInt(tmp[0])*60) + Integer.parseInt(tmp[1]);
			len = Math.abs(len);
			String edit_melody = edit(infos[3]);
			
			if(len > edit_melody.length()) {
				int trans_music_len = edit_melody.length();
				for (int j = 0; j < len - trans_music_len; j++) {
					edit_melody += edit_melody.charAt(j % trans_music_len);
				}
			} else if(len < edit_melody.length()) {
				edit_melody = edit_melody.substring(0, len);
			}
			
			music[i] = new MusicInfo(i, infos[2], edit_melody);
		}
		
		/* 문제에 나온 조건에 맞게 정렬 */
		Arrays.sort(music, new Comparator<MusicInfo>() {

			@Override
			public int compare(MusicInfo o1, MusicInfo o2) {
				if(o1.melody.length() == o2.melody.length())
					return o1.index - o2.index;
				return o2.melody.length() - o1.melody.length();
			}
		});
		
		/* 네오가 들은 음악도 C# -> c 로 수정해준다. */
		m = edit(m);
		
		/* 정답찾기 */
		for (int i = 0; i < music.length; i++) {
			MusicInfo musicInfo = music[i];
			String melody = musicInfo.melody;
			
			if(melody.contains(m)) return musicInfo.title;
			
			//or
//			for (int j = 0; j <= melody.length()-m.length(); j++) {
//					String temp = melody.substring(j, j+m.length());
//					if(temp.equals(m)) return musicInfo.title;
//			}
		}
		
		return "(None)";
	}
	
	static String edit(String s) {
		StringBuilder sb = new StringBuilder();
		for (int i = 0; i < s.length()-1; i++) {
			char c = s.charAt(i);
			char next = s.charAt(i+1);
			if(c == '#') continue;
			if(next == '#') sb.append(String.valueOf(c).toLowerCase());
			else sb.append(c);
		}
		
		if(s.charAt(s.length()-1) != '#') sb.append(s.charAt(s.length()-1));
		
		return new String(sb);
	}
	
	static class MusicInfo {
		int index;
		String title, melody;
		
		public MusicInfo(int index, String title, String melody) {
			this.index = index;
			this.title = title;
			this.melody = melody;
		}
	}
}
```




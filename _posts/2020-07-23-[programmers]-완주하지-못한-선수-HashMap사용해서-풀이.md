---
layout: post
title: "[programmers]완주하지 못한 선수 HashMap사용해서 풀이"
subtitle : level 1
tags: [algorithm]
author: Gaeun Kim
comments : True
---

<h2>문제</h2>

[programmers 완주하지 못한 선수 문제 보러가기](https://programmers.co.kr/learn/courses/30/lessons/42576)

<br><br>

<h2>풀이</h2>

```java
import java.util.HashMap;
import java.util.Iterator;

public class Solution1 {

	public String solution(String[] participant, String[] completion) {
        HashMap<String, Integer> hashMap = new HashMap<String, Integer>();
        
        for (String p : participant) {
        	hashMap.put(p, hashMap.getOrDefault(p, 0) + 1);
        }
        
        for (String c : completion) {
			hashMap.put(c, hashMap.getOrDefault(c, 0) - 1);
		}
        
        Iterator<String> keys = hashMap.keySet().iterator();
        
        while(keys.hasNext()) {
        	String key = keys.next();
        	if(hashMap.get(key) == 1)
        		return key;
        }
        return null;
    }
}
```

<br>

<br>

<h2>문제점 해결</h2>

처음 이 문제를 풀었을 땐 `Arrays.sort()`를 사용해서 풀었었다. 다른 사람들의 풀이를 보고 `HashMap`을 사용해서 푸는 방법이 있음을 알았고 `getOrDefault()`를 처음 접하게 되었다.

<br>

<h3>getOrDefault()</h3>

HashMap<Key, Value> 에서 Key값이 없다면 설정한 default 값을 반환한다.

<br>

```java
//participant = {"mislav", "stanko", "mislav", "ana"}
//completion = {"stanko", "mislav", "ana"}
for (String p : participant) hashMap.put(p, hashMap.getOrDefault(p, 0) + 1);
//mislav=2, stanko=1, ana=1
```

위의 코드를 통해 participant에 "mislav"가 2명인것을 확인할 수 있다.

예를 들어 hashMap에 put할 때, participant배열의 0번 인덱스 값은 "mislav"이다.

key는 "mislav"이며 value는 hashMap에 **"mislav"**가 **이미 존재**하면 **key값**이 **"mislav"인 value값을 반환**하고 **"mislav"가 없다면 default값을 반환**한다.

현재 hashMap엔 "mislav"가 없기 때문에 defalut로 설정한 0에 1이 더해진 값이 들어가므로 `hashMap.put(mislav, 1)`과 같다.

participant의 2번째 인덱스 값 또한 mislav이다.  **"mislav"**가 **이미 존재**하기 때문에 **key "mislav"의 value값 1을 반환한다.** 여기에 1이 더해지면 2가 된다.

<br>

```java
 for (String c : completion) hashMap.put(c, hashMap.getOrDefault(c, 0) - 1);
```

completion은 반대로 생각하면 되는데, 이미 key값이 hashMap에 존재하면 반환되는 값에서 1을 빼준다. 해당 문제는 participant와 completion가 무조건 1이 차이가 나기 때문에 결론적으로 값이 1인 한 명만 찾아내면 풀 수 있는 문제이다.

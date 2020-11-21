---
layout: post
title: "[programmers] [1차]뉴스 클러스터링"
subtitle : level 2, 2018 KAKAO BLIND RECRUITMENT 문제
tags: [algorithm]
author: Gaeun Kim
comments : True
---

<h2>문제</h2>

[programmers [1차]뉴스 클러스터링 문제 보러가기](https://programmers.co.kr/learn/courses/30/lessons/17677)

<br><br>

<h2>문제점 해결</h2>

문제에 나와있는 조건에 맞게 코딩하면 되는 문제였지만 공집합 처리 과정에서 시간이 조금 걸린 문제였다. A집합과 B집합 모두 공집합일 경우엔 자카드 유사도를 1로 정의되어있어,  **교집합이 0이거나 합집합이 0이면 65536을 return**하는 것으로 코딩했는데, 5번과 13번 TC에서 실패했다.

교집합이 0이면 자카드 유사도를 계산해도 0이 나오니 65536을 return하면 된다고 생각했는데, 실험결과 **TC 5,12,13**의 답이 0이므로 교집합이 0인 경우엔 그냥 0을 출력하는게 정답이었다. 단순하게 생각하면 되는데 0은 정답으로 나올 수 없다는 편견이 있던 것 같다.

**A,B 모두 공집합인 경우에만 return 65536으로 바꿔주니 pass되었다.**

<br><br>

<h2>풀이</h2>

```java
import java.util.*;
class Solution {
    static int union, intersection;
    public int solution(String str1, String str2) {
        int answer = 0;
        
        Map<String, Integer> map1 = new HashMap<>();
        Map<String, Integer> map2 = new HashMap<>();
        union = 0;
        map1 = set(str1, map1);
        map2 = set(str2, map2);
        
        if(map1.size() == 0 && map2.size() == 0) return 65536; //공집합 처리
        
        Iterator<String> it = map1.keySet().iterator();
        while(it.hasNext()){
            String key = it.next();
            if(map2.get(key) != null) {
                int value1 = map1.get(key);
                int value2 = map2.get(key);
                intersection += value1 < value2 ? value1 : value2;
            }
        }

        union -= intersection;
        return (int)((double)intersection / (double)union * 65536); //정답으로 0이 나올 수 있음.
    }
    
    static Map<String, Integer> set(String s, Map<String, Integer> map){
        s = s.toLowerCase();
        for(int i = 0; i < s.length()-1; i++){
            char first = s.charAt(i);
            char second = s.charAt(i+1);
            if(first >= 'a' && first <= 'z' && second >= 'a' && second <= 'z'){
                String str = first +""+ second;
                map.put(str, map.getOrDefault(str, 0)+1);
                union++;
            }
        }
        return map;
    }
}
```




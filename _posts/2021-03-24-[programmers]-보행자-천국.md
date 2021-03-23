---
layout: post
title: "[programmers] 보행자 천국"
subtitle : level 3, 2017 카카오코드 예선 문제
tags: [algorithm]
author: Gaeun Kim
comments : True
---

<h2>문제</h2>

[programmers 보행자 천국 문제](https://programmers.co.kr/learn/courses/30/lessons/1832)

<br><br>

<h2>문제점 해결</h2>

해당 문제는 DP로 해결할 수 있는 문제였다. 어려운 부분은 숫자 2를 만날 때 직진만 가능하다(왼쪽에서 오던 차는 오른쪽으로만, 위에서 오던 차는 아래쪽으로만 진행 가능하다)는 점인데, 이걸 DP로 어떻게 해결하느냐가 문제다.

현재 위치 dp\[i]\[j]를 기준으로 위(dp\[i-1][j]) + 왼쪽(dp\[i]\[j-1])의 값을 저장해주는 대신, 2인 값을 더해야하는 경우엔 이전의 값을 찾아서 저장해준다. 예를 들어 위의 값이 2라면 한칸 더 올라가서 dp\[i-2][j]의 값이 존재하며 그 값이 0인 경우에 저장해주는 것이다.

아래는 실수를 한 부분이다.

```java
int index = 1;
if(i-index >= 0 && cityMap[i-index][j] != 1)
    while(i-index >= 0){
        if(cityMap[i-index][j] == 0) {
            dp[i][j]+= dp[i-index][j] % MOD;
            break;
        }

        index++;
    }
}   
```

문제는 while문 밖에 if문의 조건이다. 위의 값이 1이면 if문에 못들어가 while문을 돌리지 못하는 것 까진 괜찮은데 0이나 2일 경우 들어갔다가 1을 만나는 경우를 생각하지 않고 코딩해서 테스트케이스 통과를 하지 못했다. 예를 들어 처음 2를 만나고 또 올라가서 2를 만났는데 그 위에 값이 1이면 그대로 while문을 끝내야하는데 그 조건이 없다보니 한칸 더 올라가게 되는 오류가 발생한다.

제대로 된 코드는 아래코드이다.

```java
int index = 1;
while(i-index >= 0 && cityMap[i-index][j] != 1){
    if(cityMap[i-index][j] == 0) {
        dp[i][j]+= dp[i-index][j] % MOD;
        break;
    }

    index++;
}   
```

<br><br>

<h2>풀이</h2>

```java
class Solution {
    int MOD = 20170805;
    public int solution(int m, int n, int[][] cityMap) {
        int[][] dp = new int[m][n];
        
        dp[0][0] = 1;
        
        for(int i = 0; i < m; i++){
            for(int j = 0; j < n; j++){
                
                if(cityMap[i][j] == 1) continue;
                
                
                int index = 1;
                while(i-index >= 0 && cityMap[i-index][j] != 1){
                        if(cityMap[i-index][j] == 0) {
                            dp[i][j]+= dp[i-index][j] % MOD;
                            break;
                        }
                        
                        index++;
                }   
                
                index = 1;
                while(j-index >= 0 && cityMap[i][j-index] != 1){
                    if(cityMap[i][j-index] == 0){
                        dp[i][j] += dp[i][j-index] % MOD;
                        break;
                    }

                    ++index;
                }
            }
        }

        return dp[m-1][n-1] % MOD;
    }
}
```


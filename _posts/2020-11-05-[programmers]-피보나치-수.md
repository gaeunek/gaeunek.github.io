---
layout: post
title: "[programmers] 피보나치 수"
subtitle : level 2 문제
tags: [algorithm]
author: Gaeun Kim
comments : True
---

<h2>문제</h2>

[programmers 피보나치 수 문제 보러가기](https://programmers.co.kr/learn/courses/30/lessons/12945)

<br><br>

<h2>문제점 해결 및 풀이</h2>

풀이할 수 있는 방법 3가지를 사용했지만 3개의 테스트케이스에서 자꾸 실패가 떴다. 알고보니 int범위를 넘어간 값 때문이었고 long타입으로 계산 후 결과를 int형변환 해주니 통과되었다.

#### 방법 1.

```java
int f1 = 0, f2 = 1, sum = 0;
for (int i = 2; i <= n; i++) {
	sum = f1 + f2;
	sum %= 1234567;
	f1 = f2;
	f2 = sum;
}
return sum;
```

#### 방법 2.

```java
long[] dp = new long[n+1];
dp[1] = 1;
for (int i = 2; i <= n; i++) 
    dp[i] = (dp[i-1] + dp[i-2])%1234567L;
return (int)dp[n];
```

#### 방법 3.

```java
static int fibo(int n) {
	if(n <= 1) return 1;
	memo[n-1] = fibo(n-1);
	return (int)((memo[n-1] + memo[n-2])%1234567L);
}
```

방법 1, 2번은 검색과 다른 사람들의 풀이를 보고 참조하였습니다. 
---
layout: post
title: "[programmers] 자물쇠와 열쇠"
subtitle : 2020 KAKAO BLIND RECRUITMENT 문제
tags: [algorithm]
author: Gaeun Kim
comments : True
---

<h2>문제</h2>

[programmers 자물쇠와 열쇠 문제 보러가기](https://programmers.co.kr/learn/courses/30/lessons/60059)

![]({{ https://programmers.co.kr/learn/courses/30/lessons/60059 }}{{ https://programmers.co.kr/learn/courses/30/lessons/60059 }}/assets/images/bio-photo-keyboard.jpg    )

<br><br>

<h2>문제점 해결</h2>

해당 문제는 2차원 배열을 어떻게 활용하는가가 중요한 문제였다. 결국 내 머리로는 풀지 못했으나 [kakao Tech 홈페이지](https://tech.kakao.com/2019/10/02/kakao-blind-recruitment-2020-round1/)에 나와있는 해설의 도움을 받아 pass할 수 있었다.

#### 1. 확장된 2차원 배열을 만들어라.

![lock_and_key.jpg](/assets/img/200913_lock_and_key.jpg)

그림처럼 최소 key배열 1개의 인덱스와 lock배열 1개의 인덱스가 겹쳐지도록 확장된 배열을 생성하면 7 X 7 배열이 생성된다. 이때, 확장된 2차원 배열을 만들 때 크기가 정확하지않으면 런타임 에러에 빠질 수 있다.(오랜 시간이 걸린 부분이었다 ㅠㅠ)

**확장 배열의 크기는 lock배열의 크기 + (key배열의 크기 - 1) * 2 로 1을 빼주는 이유는 겹쳐지는 부분을 제외시키기 때문이다.**

확장된 배열 padding이 생성되었다면 이제 배열의 중심에 lock배열 값을 배치시킨다.

```java
int[][] padding = new int[lock_len + (key_len - 1) * 2][lock_len + (key_len - 1) * 2];
int[][] origin = new int[padding.length][padding.length];
int hole = 0;

// padding 중심에 lock배열 값을 배치하고 hole의 갯수를 카운트한다.
for (int i = 0; i < lock_len; i++) {
	for (int j = 0; j < lock_len; j++) {
		if (lock[i][j] == 1) {
			origin[key_len + i - 1][key_len + j - 1] = padding[key_len + i - 1][key_len + j - 1] = 1;
			continue;
		}
		hole++;
	}
}
```

**origin배열** : key배열을 padding배열 안에서 한칸씩 이동시킨 후 원래의 padding배열로 초기화 해주지 않으면 이전의 더해진 값들이 남아있기 때문에 생성된 배열이다.

**hole** : 만약 lock이 모두 1로 채워진 배열이라면 key를 맞춰볼 필요가 없기 때문에 key가 필요한 구멍의 개수를 세기 위한 변수로 만약 hole값이 0이라면 바로 return 해준다.

<br>

#### 2. key배열 회전

key배열은 회전 및 이동이 가능하다. 회전 -> 오른쪽 이동 -> 왼쪽 이동 -> ... 이런식으로 방향을 예측하지 못하기 때문에 key배열을 90도씩 회전 시킨 후 한칸씩 이동하면서 padding배열에 맞춰보았다.

<br>

#### 3. 자물쇠와 키의 만남

0은 홈, 1은 돌기 부분을 나타내며 키와 자물쇠엔 홈과 돌기로 이루어져 있다. 키의 돌기와 자물쇠의 돌기는 만나선 안되며 키의 돌기와 자물쇠의 홈이 만나 자물쇠 배열을 1로 모두 채워주면 true를 return한다. 즉, keky배열을 이동시키면서 해당 위치의 padding배열 값과 더해주면 하나의 인덱스가 가질 수 있는 값은 최대 2이며 2는 키의 돌기와 자물쇠의 돌기가 만났기 때문에 더 볼 필요 없이 다음 이동을 해주면 된다.

또한, 인덱스 값이 0이라면 여전히 홈이 존재하기 때문에 이 역시 볼 필요 없이 다음 이동을 진행해준다.

```java
static boolean open(int[][] padding) {
	for (int i = key_len - 1; i < lock_len + key_len - 1; i++) {
		for (int j = key_len - 1; j < lock_len + key_len - 1; j++) {
			if (padding[i][j] == 2 || padding[i][j] == 0) {
				return false;
			}
		}
	}

	return true;
}
```

<br><br>

<h2>풀이(Java)</h2>

```java
public boolean solution(int[][] key, int[][] lock) {
	key_len = key.length;
	lock_len = lock.length;

	// padding의 크기가 중요!
	int[][] padding = new int[lock_len + (key_len - 1) * 2][lock_len + (key_len - 1) * 2];
	int[][] origin = new int[padding.length][padding.length];
	int hole = 0;

	// padding 중심에 lock배열 값을 배치하고 hole의 갯수를 카운트한다.
	for (int i = 0; i < lock_len; i++) {
		for (int j = 0; j < lock_len; j++) {
			if (lock[i][j] == 1) {
				origin[key_len + i - 1][key_len + j - 1] = padding[key_len + i - 1][key_len + j - 1] = 1;
				continue;
			}
			hole++;
		}
	}

	int key_cnt = 0;
	for (int i = 0; i < key_len; i++) {
		for (int j = 0; j < key_len; j++) {
			if (key[i][j] == 1)
				key_cnt++;
		}
	}

	// lock이 모두 1이면 true를 return한다.
	if (hole == 0)
		return true;

	// key 갯수가 hole 갯수보다 적으면 어차피 열 수 없으니까 바로 종료
	if (key_cnt < hole)
		return false;

	for (int i = 0; i < 4; i++) { // 90, 180, 270 회전
		if (i != 0)
			key = rotat(key);

		if (move(key, lock, padding, origin))
			return true;
	}

	return false;
}

static boolean move(int[][] key, int[][] lock, int[][] padding, int[][] origin) {
	for (int a = 0; a < lock_len + key_len - 1; a++) {
		for (int b = 0; b < lock_len + key_len - 1; b++) {

			// 회전시킨 Key를 이동시킴
			for (int i = 0; i < key_len; i++) {
				for (int j = 0; j < key_len; j++) {
					padding[i + a][j + b] += key[i][j];
				}
			}

			if (open(padding))
				return true;
			else
				padding = copy(origin, padding);
		}
	}
	return false;
}

static int[][] copy(int[][] origin, int[][] after) {
	for (int i = 0; i < after.length; i++) {
		for (int j = 0; j < after.length; j++) {
			after[i][j] = origin[i][j];
		}
	}
	return after;
}

// 자물쇠에 일치하는지 확인
static boolean open(int[][] padding) {
	for (int i = key_len - 1; i < lock_len + key_len - 1; i++) {
		for (int j = key_len - 1; j < lock_len + key_len - 1; j++) {
			if (padding[i][j] == 2 || padding[i][j] == 0) {
				return false;
			}
		}
	}

	return true;
}

// key를 시계방향으로 회전
static int[][] rotat(int[][] key) {
	int[][] result = new int[key_len][key_len];
	int[] tmp = new int[key_len];

	for (int i = 0; i < key_len; i++) {
		tmp = key[i];
		for (int j = 0; j < key_len; j++)
			result[j][key_len - 1 - i] = tmp[j];
	}

	return result;
}
```




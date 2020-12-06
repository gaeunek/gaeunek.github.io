---
layout: post
title: "[programmers] 카카오프렌즈 컬러링북"
subtitle : level 2, 2017 카카오코드 예선 문제
tags: [algorithm]
author: Gaeun Kim
comments : True
---

<h2>문제</h2>

[programmers 카카오프렌즈 컬러링북 문제 보러가기](https://programmers.co.kr/learn/courses/30/lessons/1829)

<br><br>

<h2>문제점 해결</h2>

BFS(너비 우선 탐색 : Breadth-First Search)를 사용했다.

1. 0이 아닌 숫자를 만나면 queue에 위치(Point)를 저장한다.
2. queue에 저장된 위치를 꺼낸다. 이때, 영역의 크기를 파악하기 위해 queue에서 poll()할 때 마다 countBlock을 1 증가시킨다.
3. 해당 위치를 중심으로 4방향으로 같은 값을 가진 위치가 있는지 탐색하여 queue에 저장한다.
4. queue가 빈 상태가 되면 전체 영역의 개수(numberOfArea)를 1 증가시킨다.

<br><br>

<h2>풀이</h2>

{% raw %}

```java
import java.util.*;
class Solution {
    static boolean check(int i, int j, int m, int n){
        if(i >= 0 && i < m && j >= 0 && j < n) return true;
        else return false;
    }
    public int[] solution(int m, int n, int[][] picture) {
        int numberOfArea = 0;
        int maxSizeOfOneArea = 0;

        int[] answer = new int[2];
        
        Queue<Point> queue = new LinkedList<>();
        boolean[][] visited = new boolean[m][n];
        int[][] dir = {{0, 1}, {1, 0}, {0, -1}, {-1, 0}};
        
        for(int i = 0; i < m; i++){
            for(int j = 0; j < n; j++){
                if(picture[i][j] == 0 || visited[i][j]) continue;
                
                int block = picture[i][j];
                int countBlock = 0;
                queue.add(new Point(i, j));
                visited[i][j] = true;
                
                while(!queue.isEmpty()){
                    Point now = queue.poll();
                    countBlock++;
                        for(int d = 0; d < 4; d++){
                        int ni = dir[d][0] + now.i;
                        int nj = dir[d][1] + now.j;

                        if(check(ni, nj, m, n) && picture[ni][nj] == block && !visited[ni][nj]) {
                            queue.add(new Point(ni, nj));
                            visited[ni][nj] = true;
                        }
                    }
                }
                
                maxSizeOfOneArea = countBlock > maxSizeOfOneArea ? countBlock : maxSizeOfOneArea;
                numberOfArea++;
            }
        }
        
        answer[0] = numberOfArea;
        answer[1] = maxSizeOfOneArea;
        return answer;
    }
    
    static class Point {
        int i, j;
        
        public Point(int i, int j){
            this.i = i;
            this.j = j;
        }
    }
}
```

{% endraw %}
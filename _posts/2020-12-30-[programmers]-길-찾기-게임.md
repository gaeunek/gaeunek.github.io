---
layout: post
title: "[programmers] 길 찾기 게임"
subtitle : level 3, 2019 KAKAO BLIND RECRUITMENT 문제
tags: [algorithm]
author: Gaeun Kim
comments : True
---

<h2>문제</h2>

[programmers 길 찾기 게임 문제 보러가기](https://programmers.co.kr/learn/courses/30/lessons/42892)

<br><br>

<h2>문제점 해결</h2>

문제에 나와있는 그대로 이진트리를 만들어 전위, 후위 순회 결과를 출력하는 문제였다. 

<br>

#### 1. 트리 순회의 개념

중위순회는 **왼쪽 → Root → 오른쪽** 전위순회는 **Root → 왼쪽 → 오른쪽** 마지막으로 후위순회는 **왼쪽 → 오른쪽 → Root**이다.

해당 문제는 전위와 후위를 구하는 문제이다.

> [위키백과 트리 순회](https://ko.wikipedia.org/wiki/%ED%8A%B8%EB%A6%AC_%EC%88%9C%ED%9A%8C)

<br>

#### 2. Node 객체 생성

입력으로 주어진 nodeinfo배열 값을 이진트리로 만들기 위해 먼저 Node 객체를 생성한다. Node 객체 구성 요소는 좌표(x, y)와 nodeinfo배열 순서 번호(num)이다. 그리고, 루트 노드(root node)를 찾기 위해 입력 배열의 정렬이 필요하기 때문에 Comparable를 사용하였다.

```java
static class Node implements Comparable<Node> {
	int x, y, num;

	public Node(int x, int y, int num) {
		this.x = x;
		this.y = y;
		this.num = num;
	}

	@Override
	public int compareTo(Node o) {
		if(this.y == o.y) return this.x - o.x;
		return o.y - this.y;
	}
}
```

<br>

#### 3. 이진 트리 생성

먼저 이진 트리를 구성한 후 부모 노드 값과 비교 후 작은 값이면 왼쪽, 큰 값이면 오른쪽 노드로 연결시켰다.

```java
public void make(TreeNode top, Node node) {
    TreeNode tnode = new TreeNode(node);
			
	if(top.root.x > node.x) {
		if(top.left == null) makeLeftChild(top, tnode);
		else make(top.left, node);
	} else {
		if(top.right == null) makeRightChild(top, tnode);
		else make(top.right, node);
	}
}

public void makeLeftChild(TreeNode top, TreeNode tnode) {
	top.left = tnode;
}
		
public void makeRightChild(TreeNode top, TreeNode tnode) {
	top.right = tnode;
}
```

<br>

#### 4. 순회

전위순회는 진행 순서가 **Root → 왼쪽 → 오른쪽**이기 때문에 재귀함수를 호출 할 때 마다 노드값을 먼저 저장해준다. 반대로 후위순회는 **왼쪽 → 오른쪽 → Root**이기 때문에 노드값을 마지막에 저장해준다.

```java
public void preorder(TreeNode tnode){
	po.add(tnode.root.num);
			
	if(tnode.left != null) preorder(tnode.left);
	if(tnode.right != null) preorder(tnode.right);
}
		
public void postorder(TreeNode tnode) {
	if(tnode.left != null) postorder(tnode.left);
	if(tnode.right != null) postorder(tnode.right);
			
	ps.add(tnode.root.num);
}
```

<br>

순회 종류의 진행 순서를 정확하게 알고있지 못한 상태에서 풀어 시간이 꽤 걸렸지만, 스스로 문제를 풀어본 후 순회 개념에 대해 다시 한 번 공부하며 소스코드를 깔끔하게 정리하였다. 그림을 그려가며 규칙성이나 반대되는 개념임을 잘 파악했다면 좀 더 빨리 풀 수 있던 문제라고 생각한다. 전위 순회와 다르게 처음엔 후위 순회 메소드를 복잡하게 코딩하였는데, 개념에 대해 다시 공부한 후 전위 순회 소스 코드를 반대로만 바꾸면 되는 간단한 문제임을 깨달았다. 개념 공부를 후에 했지만 결과적으론 기억에 더 잘 남게 되었다.

<br><br>

<h2>풀이</h2>

```java
import java.util.*;
class Solution {
	static List<Integer> po, ps;
	public int[][] solution(int[][] nodeinfo) {
		Node[] nodes = new Node[nodeinfo.length];
		po = new ArrayList<>();
		ps = new ArrayList<>();
		
		for (int i = 0; i < nodes.length; i++) {
			int[] tmp = nodeinfo[i];
			nodes[i] = new Node(tmp[0], tmp[1], i + 1);
		}
	
		Arrays.sort(nodes);
		
		TreeNode treeNode = new TreeNode(nodes[0]);
		for (int i = 1; i < nodes.length; i++) {
			treeNode.make(treeNode, nodes[i]);
		}
		
		/*전위 순회*/
		treeNode.preorder(treeNode);
		/*후위 순회*/
		treeNode.postorder(treeNode);
		
		int[][] answer = new int[2][po.size()];
		for (int i = 0; i < po.size(); i++) {
			answer[0][i] = po.get(i);
			answer[1][i] = ps.get(i);
		}
		
		return answer;
	}
	
	static class TreeNode {
		Node root;
		TreeNode left, right;
		
		public TreeNode(Node node) {
			this.root = node;
			this.left = null;
			this.right = null;
		}
		
		public void make(TreeNode top, Node node) {
			TreeNode tnode = new TreeNode(node);
			
			if(top.root.x > node.x) {
				if(top.left == null) makeLeftChild(top, tnode);
				else make(top.left, node);
			} else {
				if(top.right == null) makeRightChild(top, tnode);
				else make(top.right, node);
			}
		}
		
		public void makeLeftChild(TreeNode top, TreeNode tnode) {
			top.left = tnode;
		}
		
		public void makeRightChild(TreeNode top, TreeNode tnode) {
			top.right = tnode;
		}
		
		public void preorder(TreeNode tnode){
			po.add(tnode.root.num);
			
			if(tnode.left != null) preorder(tnode.left);
			if(tnode.right != null) preorder(tnode.right);
		}
		
		public void postorder(TreeNode tnode) {
			if(tnode.left != null) postorder(tnode.left);
			if(tnode.right != null) postorder(tnode.right);
			
			ps.add(tnode.root.num);
		}
	}
	

	static class Node implements Comparable<Node> {
		int x, y, num;

		public Node(int x, int y, int num) {
			this.x = x;
			this.y = y;
			this.num = num;
		}

		@Override
		public int compareTo(Node o) {
			if(this.y == o.y) return this.x - o.x;
			return o.y - this.y;
		}
	}
}
```


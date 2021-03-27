---
layout: post
title: "[programmers] 가사 검색"
subtitle : level 4, 2020 KAKAO BLIND RECRUITMENT 문제
tags: [algorithm]
author: Gaeun Kim
comments : True
---

<h2>문제</h2>

[programmers 가사 검색 문제 보러가기](https://programmers.co.kr/learn/courses/30/lessons/60060)

<br><br>

<h2>문제점 해결</h2>

Trie 알고리즘의 대표 문제라고 생각하며 Trie 알고리즘 공부하기에 딱이다! '?'가 중간에 나오는 쿼리가 없이 인덱스의 가장 앞 또는 뒤에 연속적으로 나타나기 때문에 Node를 저장할 때 앞, 뒤로 저장해준다. 예를 들어 키워드가 "frodo"라면 "f" 부터 시작하는 Node와 "o"부터 시작하는 Node를 저장해준다.

그리고 검색 키워드의 최대 길이는 10,000이므로 이 크기의 배열을 만들어 query에 해당하는 키워드가 애초에 저장되어 있지 않을땐 빠르게 필터링 해준다.

<br><br>

<h2>풀이</h2>

```java
import java.util.*;

class Solution {
	public int[] solution(String[] words, String[] queries) {
		int[] answer = new int[queries.length];

		TreeNode[] treeNode = new TreeNode[10001];

		for (int i = 0; i < words.length; i++) {
			int len = words[i].length();

			if (treeNode[len] == null) {
				treeNode[len] = new TreeNode();
			}

			treeNode[len].insert(words[i]);
		}
		
		for (int i = 0; i < queries.length; i++) {
			int len = queries[i].length();
			String query = queries[i];

			if (treeNode[len] == null)
				continue;
			
			answer[i] = treeNode[len].get(query);
		}

		return answer;
	}

	static class TreeNode {
		Node front, back;

		public TreeNode() {
			front = new Node();
			back = new Node();
		}

		public void insert(String word) {
			insertFront(word.toCharArray());
			insertBack(word.toCharArray());
		}

		public void insertFront(char[] c) {
			Node nowNode = front;

			for (int i = 0; i < c.length; i++) {
				nowNode.count++;
				nowNode = nowNode.children.computeIfAbsent(c[i], key -> new Node());
			}
		}

		public void insertBack(char[] c) {
			Node nowNode = back;

			for (int i = c.length - 1; i >= 0; i--) {
				nowNode.count++;
				nowNode = nowNode.children.computeIfAbsent(c[i], key -> new Node());
			}
		}

		public int get(String query) {
			char[] c = query.toCharArray();

			if (c[0] == '?') {
				return getBack(c);
			} else {
				return getFront(c);
			}
		}

		public int getFront(char[] c) {
			Node nowNode = front;

			for (int i = 0; i < c.length; i++) {
				if(nowNode == null) return 0;
				
				if (c[i] == '?') {
					return nowNode.count;
				}

				nowNode = nowNode.children.get(c[i]);
			}

			return nowNode.count;
		}

		public int getBack(char[] c) {
			Node nowNode = back;

			for (int i = c.length - 1; i >= 0; i--) {
				if(nowNode == null) return 0;
				
				if (c[i] == '?') {
					return nowNode.count;
				}

				nowNode = nowNode.children.get(c[i]);
			}

			return nowNode.count;
		}
	}

	static class Node {
		Map<Character, Node> children;
		int count;

		public Node() {
			children = new HashMap<>();
			count = 0;
		}
	}
}
```


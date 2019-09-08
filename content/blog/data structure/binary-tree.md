---
title: binary tree
date: 2019-09-04 22:09:34
category: data structure
---

자료구조에서 가장 중요한건 배열이나 리스트이겠지만, 그 다음은 이진트리라고 나는 생각한다.

이진트리는 간단해 보이지만 배울게 많다.

## 정의

이진트리는 한 노드가 최대 두 개의 자식 노드를 가지는 트리를 뜻한다.

## 종류

### 정 이진 트리 (full binary tree)

단말 노드가 아닌 모든 노드가 2개인 자식을 가진 트리

### 포화 이진 트리 (perfect binary tree)

모든 단말 노드의 깊이가 같은 정 이진 트리

> 포화 이진 트리를 full binary tree로 알고 있는 분들이 있지만 아니다!

### 완전 이진 트리 (complete binary tree)

끝 부분을 제외하고 모든 노드가 채워진 이진트리. **마지막 레벨의 노드들은 왼쪽으로 채워져 있다.**

완전 이진트리들은 노드를 삽입할 때 왼쪽부터 차례대로 삽입하는 트리!

## 트리 순회

트리순회는 전위순회(preorder), 중위순회(inorder), 후위순회(postorder)가 있다.

<img src='./images/binary01.png'/>

### 시간 복잡도

최적: ![img](./images/binary02.png)

최악: O(N)

### reference

* [이진트리:: 위키백과](https://ko.wikipedia.org/wiki/%EC%9D%B4%EC%A7%84_%ED%8A%B8%EB%A6%AC)
* [완전이진 트리 :: 조물조물](https://jomuljomul.tistory.com/entry/완전이진트리Complete-Binary-Tree란)
* [이진트리개념과 구현 :: 문메이](https://meylady.tistory.com/16)
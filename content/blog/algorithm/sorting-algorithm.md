---
title: Sorting Algorithm
date: 2019-09-11 22:09:19
category: algorithm
---

sorting algorithm들을 정리 해보려고 합니다.

실제 코드를 짤때는 그냥 라이브러리를 사용하겠지만 적어도 어떤 정렬 알고리즘들이 있는지, 어떤 장단점이 있는지 그리고 코드로 구현할 수 있어야합니다.

모든 알고리즘을 정리할 수는 없겠지만 [정렬 알고리즘::나무위키](https://namu.wiki/w/%EC%A0%95%EB%A0%AC%20%EC%95%8C%EA%B3%A0%EB%A6%AC%EC%A6%98)에 있는 내용을 기반으로 많이 사용되는 알고리즘을 정리하겠습니다.

## Overview

데이터를 정렬해야 하는 이유는 탐색을 위해서입니다. 탐색할 대상 데이터가 정렬되어 있지 않다면 순차 탐색 이외에 다른 알고리즘을 사용할 수는 없겠지만 **데이터가 정렬되어 있다면 [이진 탐색](https://namu.wiki/w/%EC%9D%B4%EC%A7%84%20%ED%83%90%EC%83%89)이라는 강력한 알고리즘을 사용할 수 있습니다.** 삽입과 삭제가 자주 되는 자료의 경우 정렬에 더 많은 시간이 들어가므로 순차 탐색을 하는 경우도 있지만 대부분의 경우 삽입/삭제보다는 데이터를 조회하는 것이 압도적으로 많고 조회에 필요한 것이 바로 검색입니다.

## 정렬 알고리즘 시간복잡도 비교

<img src='./images/sort-time.png'/>

* 단순(구현 간단)하지만 비효율적인 방법
  * 삽입 정렬, 선택 정렬, 버블 정렬
* 복잡하지만 효율적인 방법
  * 퀵 정렬, 힙 정렬, 합병 정렬, 기수 정렬

## 1. 버블 정렬 (Bubble Sort)

1번째와 2번째 원소를 비교하여 정렬하고, 2번째와 3번째, ..., n-1번째와 n번째를 정렬한 뒤 다시 처음으로 돌아가 이번에는 n-2번째와 n-1번째까지, ...해서 최대 n(n - 1) / 2번 정렬합니다.

버블 정렬은 거의 모든 상황에서 최악의 성능을 보여줍니다. 단, 이미 정렬된 자료에서는 1번만 돌면 되기 때문에 최선의 성능을 보여줍니다.

가장 손쉽게 구현하여 사용할 수 있지만, 만들기가 쉽고 직관적일 뿐이지 알고리즘적 관점에 보면 대단히 비효율적인 정렬방식입니다.

### 시간복잡도

O(n^2)

### 특징

#### 장점

* 구현이 매우 간단하다.

#### 단점
* 순서에 맞지 않은 요소를 인접한 요소와 교환한다.
* 하나의 요소가 가장 왼쪽에서 가장 오른쪽으로 이동하기 위해서는 배열에서 모든 다른 요소들과 교환되어야 한다.
* 특히 특정 요소가 최종 정렬 위치에 이미 있는 경우라도 교환되는 일이 일어난다.

일반적으로 자료의 교환 작업(SWAP)이 자료의 이동 작업(MOVE)보다 더 복잡하기 때문에 버블 정렬은 단순성에도 불구하고 **거의 쓰이지 않는다.**

### 구현 코드

```c++
void bubleSort(int list[], int listSize) {
	for (int i = listSize - 1; i > 0; i--) {
		for (int j = 0; j < i; j++) {
			if (list[j] > list[j + 1]) {
				int tmp = list[j];
				list[j] = list[j + 1];
				list[j + 1] = tmp;
			}
		}
	}
}
```



## 2. 선택 정렬 (Selection Sort)

버블 정렬이 비교하고 바로 바꿔 넣는 걸 반복한다면 이쪽은 일단 1번째부터 끝까지 훑어서 가장 작은 게 1번째, 2번째부터 끝까지 훑어서 가장 작은 게 2번째……해서 (n-1)번 반복합니다.

어찌 보면 인간이 사용하는 정렬 방식을 가장 많이 닮았습니다. 어떻게 정렬이 되어 있든 일관성 있게 n(n - 1) / 2 에 비례하는 시간이 걸린다는게 특징입니다. 또한, 버블 정렬보다 두 배 정도 빠릅니다.

### 시간복잡도

O(n^2)

### 특징

#### 장점

* 자료 이동 횟수가 미리 결정된다.

  #### 단점

* 안정성을 만족하지 않는다.

* 즉, 값이 같은 레코드가 있는 경우에 상대적인 위치가 변경될 수 있다.

### 구현 코드

```c++
void selectionSort(int list[], int listSize) {
	for (int i = 0; i < listSize - 1; i++) {
		int minNumIdx = i;
		for (int j = i; j < listSize; j++) {
			if (list[minNumIdx] > list[j]) {
				list = j;
			}
		}
		int tmp = randomList[list];
		list[minNumIdx] = list[i];
		list[i] = tmp;
	}
}
```

### 3. 삽입 정렬 (Insertion Sort)

<img src='https://w.namu.la/s/e2cca975b1e03bd676ae5e11433526429e9cf77953039ca19a2df4b1112eb75c9c45701ca4f75bcb78194f07ec7b60f28040a4bae7ceed58729887ff62fc13f641682dd0a76feac03e643811437a0c40f3c53b338e965e5dd6c271d7a4064bdf' />

k번째 원소를 1부터 k-1까지와 비교해 적절한 위치에 끼워넣고 그 뒤의 자료를 한 칸씩 뒤로 밀어내는 방식으로, 평균적으로 O(n^2)중 빠른 편이나 자료구조에 따라선 뒤로 밀어내는데 걸리는 시간이 크며, 앞의 예시처럼 작은 게 뒤쪽에 몰려있으면 그야말로 헬게이트입니다.

그밖에도 배열이 작을 경우에 역시 상당히 효율적인데요. 일반적으로 빠르다고 알려진 알고리즘들도 배열이 작을 경우에는 대부분 삽입 정렬에 밀립니다.

 따라서 고성능 알고리즘들 중에서는 배열의 사이즈가 클때는 *O*(*n*log*n*) 알고리즘을 쓰다가 정렬해야 할 부분이 작을때 는 삽입 정렬로 전환하는 것들도 있습니다.

### 시간복잡도

O(n^2)

### 특징

#### 장점

* 안정한 정렬 방법
* 레코드의 수가 적을 경우 알고리즘 자체가 매우 간단하므로 다른 복잡한 정렬 방법보다 유리할 수 있다.
* 대부분위 레코드가 이미 정렬되어 있는 경우에 매우 효율적일 수 있다.

#### 단점
* 비교적 많은 레코드들의 이동을 포함한다.
* 레코드 수가 많고 레코드 크기가 클 경우에 적합하지 않다.

### 구현 코드

```c++
void insertionSort(int list[], int listSize){
  
  for(int i = 1; i < listSize; i++){
    for(int j = i; j >= 0 && list[j] > list[i]; j--){
      list[j + 1] = list[j];
    }
  }
}
```



## 4. 합병 정렬 (Merge Sort)

<img src='https://ww.namu.la/s/30bb5bb955f72d8a4b70c88e0cb83fe97ae0c349bd9c27d1204e8939df903ef7748c25b1928455ad76d70fd7a283b1c131feecabca2fe5a9c36b4ab72fe3e778320db817cf709f625c4132640aee1d47aca18f0bd40ac09a7f95c78db18c05b4' />

개발자는 [존 폰 노이만](https://namu.wiki/w/%EC%A1%B4%20%ED%8F%B0%20%EB%85%B8%EC%9D%B4%EB%A7%8C)으로 원소 개수가 1 또는 0이 될 때까지 두 부분으로 쪼개고 쪼개서 자른 순서의 역순으로 크기를 비교해 병합해 나갑니다. 병합된 부분 안은 이미 정렬되어 있으므로 전부 비교하지 않아도 제자리를 찾을 수 있습니다. 대표적인 분할 정복 알고리즘으로 존 폰 노이만의 천재성을 엿볼 수 있는 알고리즘입니다.

성능은 아래의 퀵 정렬보다 전반적으로 뒤떨어지고, **데이터 크기만한 메모리가 더 필요**하지만 최대의 장점은 **데이터의 상태에 별 영향을 받지 않는다는 점**입니다. 힙이나 퀵의 경우에는 배열 `A[25]=100`, `A[33]=100`인 정수형 배열을 정렬한다고 할 때, 33번째에 있던 100이 25번째에 있던 100보다 앞으로 오는 경우가 생길 수 있다. 그에 반해서 병합정렬은 그런 거 없다.정렬되어 있는 두 배열을 합집합할 때 이 알고리즘의 마지막 단계만을 이용하면 가장 빠르게 정렬된 상태로 합칠 수 있다.

### 시간복잡도

항상 **O(nlogn)**의 성능을 내는 알고리즘으로 힙정렬과 같고 최악의 상황의 퀵(n^2) 보다 안정적이다.

## 특징

#### 단점

- 만약 레코드를 배열(Array)로 구성하면, 임시 배열이 필요하다. 제자리 정렬(in-place sorting)이 아니다.
- 레크드들의 크기가 큰 경우에는 이동 횟수가 많으므로 매우 큰 시간적 낭비를 초래한다.

#### 장점

- 안정적인 정렬 방법 
  - 데이터의 분포에 영향을 덜 받는다. 즉, 입력 데이터가 무엇이든 간에 정렬되는 시간은 동일하다. O(nlog₂n)로 동일
- 만약 레코드를 연결 리스트(Linked List)로 구성하면, 링크 인덱스만 변경되므로 데이터의 이동은 무시할 수 있을 정도로 작아진다. 
  - 제자리 정렬(in-place sorting)로 구현할 수 있다.
- 따라서 크기가 큰 레코드를 정렬할 경우에 연결 리스트를 사용한다면, 합병 정렬은 퀵 정렬을 포함한 다른 어떤 졍렬 방법보다 효율적이다.

### 구현 코드

<div class="colorscripter-code" style="color:#010101;font-family:Consolas, 'Liberation Mono', Menlo, Courier, monospace !important; position:relative !important;overflow:auto"><table class="colorscripter-code-table" style="margin:0;padding:0;border:none;background-color:#fafafa;border-radius:4px;" cellspacing="0" cellpadding="0"><tr><td style="padding:6px;border-right:2px solid #e5e5e5"><div style="margin:0;padding:0;word-break:normal;text-align:right;color:#666;font-family:Consolas, 'Liberation Mono', Menlo, Courier, monospace !important;line-height:130%"><div style="line-height:130%">1</div><div style="line-height:130%">2</div><div style="line-height:130%">3</div><div style="line-height:130%">4</div><div style="line-height:130%">5</div><div style="line-height:130%">6</div><div style="line-height:130%">7</div><div style="line-height:130%">8</div><div style="line-height:130%">9</div><div style="line-height:130%">10</div><div style="line-height:130%">11</div><div style="line-height:130%">12</div><div style="line-height:130%">13</div><div style="line-height:130%">14</div><div style="line-height:130%">15</div><div style="line-height:130%">16</div><div style="line-height:130%">17</div><div style="line-height:130%">18</div><div style="line-height:130%">19</div><div style="line-height:130%">20</div><div style="line-height:130%">21</div><div style="line-height:130%">22</div><div style="line-height:130%">23</div><div style="line-height:130%">24</div><div style="line-height:130%">25</div><div style="line-height:130%">26</div><div style="line-height:130%">27</div><div style="line-height:130%">28</div><div style="line-height:130%">29</div><div style="line-height:130%">30</div><div style="line-height:130%">31</div><div style="line-height:130%">32</div><div style="line-height:130%">33</div><div style="line-height:130%">34</div><div style="line-height:130%">35</div><div style="line-height:130%">36</div><div style="line-height:130%">37</div><div style="line-height:130%">38</div><div style="line-height:130%">39</div><div style="line-height:130%">40</div><div style="line-height:130%">41</div></div></td><td style="padding:6px 0;text-align:left"><div style="margin:0;padding:0;color:#010101;font-family:Consolas, 'Liberation Mono', Menlo, Courier, monospace !important;line-height:130%"><div style="padding:0 6px; white-space:pre; line-height:130%"><span style="color:#066de2">int</span>&nbsp;sorted[MAX_SIZE];</div><div style="padding:0 6px; white-space:pre; line-height:130%">&nbsp;</div><div style="padding:0 6px; white-space:pre; line-height:130%"><span style="color:#a71d5d">void</span>&nbsp;merge(<span style="color:#066de2">int</span>&nbsp;list[],&nbsp;<span style="color:#066de2">int</span>&nbsp;left,&nbsp;<span style="color:#066de2">int</span>&nbsp;mid,&nbsp;<span style="color:#066de2">int</span>&nbsp;right){</div><div style="padding:0 6px; white-space:pre; line-height:130%">&nbsp;&nbsp;<span style="color:#066de2">int</span>&nbsp;i,&nbsp;j,&nbsp;k,&nbsp;l;</div><div style="padding:0 6px; white-space:pre; line-height:130%">&nbsp;&nbsp;i&nbsp;<span style="color:#ff3399"></span><span style="color:#a71d5d">=</span>&nbsp;left;</div><div style="padding:0 6px; white-space:pre; line-height:130%">&nbsp;&nbsp;j&nbsp;<span style="color:#ff3399"></span><span style="color:#a71d5d">=</span>&nbsp;mid<span style="color:#ff3399"></span><span style="color:#a71d5d">+</span><span style="color:#0099cc">1</span>;</div><div style="padding:0 6px; white-space:pre; line-height:130%">&nbsp;&nbsp;k&nbsp;<span style="color:#ff3399"></span><span style="color:#a71d5d">=</span>&nbsp;left;</div><div style="padding:0 6px; white-space:pre; line-height:130%">&nbsp;</div><div style="padding:0 6px; white-space:pre; line-height:130%">&nbsp;&nbsp;<span style="color:#999999">/*&nbsp;분할&nbsp;정렬된&nbsp;list의&nbsp;합병&nbsp;*/</span></div><div style="padding:0 6px; white-space:pre; line-height:130%">&nbsp;&nbsp;<span style="color:#a71d5d">while</span>&nbsp;(i&nbsp;<span style="color:#ff3399"></span><span style="color:#a71d5d">&lt;</span><span style="color:#ff3399"></span><span style="color:#a71d5d">=</span>&nbsp;mid&nbsp;<span style="color:#ff3399"></span><span style="color:#a71d5d">&amp;</span><span style="color:#ff3399"></span><span style="color:#a71d5d">&amp;</span>&nbsp;j&nbsp;<span style="color:#ff3399"></span><span style="color:#a71d5d">&lt;</span><span style="color:#ff3399"></span><span style="color:#a71d5d">=</span>&nbsp;right)&nbsp;{</div><div style="padding:0 6px; white-space:pre; line-height:130%">&nbsp;&nbsp;&nbsp;&nbsp;<span style="color:#a71d5d">if</span>&nbsp;(list[i]&nbsp;<span style="color:#ff3399"></span><span style="color:#a71d5d">&lt;</span><span style="color:#ff3399"></span><span style="color:#a71d5d">=</span>&nbsp;list[j])</div><div style="padding:0 6px; white-space:pre; line-height:130%">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;sorted[k<span style="color:#ff3399"></span><span style="color:#a71d5d">+</span><span style="color:#ff3399"></span><span style="color:#a71d5d">+</span>]&nbsp;<span style="color:#ff3399"></span><span style="color:#a71d5d">=</span>&nbsp;list[i<span style="color:#ff3399"></span><span style="color:#a71d5d">+</span><span style="color:#ff3399"></span><span style="color:#a71d5d">+</span>];</div><div style="padding:0 6px; white-space:pre; line-height:130%">&nbsp;&nbsp;&nbsp;&nbsp;<span style="color:#a71d5d">else</span></div><div style="padding:0 6px; white-space:pre; line-height:130%">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;sorted[k<span style="color:#ff3399"></span><span style="color:#a71d5d">+</span><span style="color:#ff3399"></span><span style="color:#a71d5d">+</span>]&nbsp;<span style="color:#ff3399"></span><span style="color:#a71d5d">=</span>&nbsp;list[j<span style="color:#ff3399"></span><span style="color:#a71d5d">+</span><span style="color:#ff3399"></span><span style="color:#a71d5d">+</span>];</div><div style="padding:0 6px; white-space:pre; line-height:130%">&nbsp;&nbsp;}</div><div style="padding:0 6px; white-space:pre; line-height:130%">&nbsp;</div><div style="padding:0 6px; white-space:pre; line-height:130%">&nbsp;&nbsp;<span style="color:#999999">//&nbsp;남아&nbsp;있는&nbsp;값들을&nbsp;일괄&nbsp;복사</span></div><div style="padding:0 6px; white-space:pre; line-height:130%">&nbsp;&nbsp;<span style="color:#a71d5d">if</span>&nbsp;(j&nbsp;<span style="color:#ff3399"></span><span style="color:#a71d5d">&lt;</span>&nbsp;right)&nbsp;{</div><div style="padding:0 6px; white-space:pre; line-height:130%">&nbsp;&nbsp;&nbsp;&nbsp;<span style="color:#a71d5d">for</span>&nbsp;(l&nbsp;<span style="color:#ff3399"></span><span style="color:#a71d5d">=</span>&nbsp;j;&nbsp;l&nbsp;<span style="color:#ff3399"></span><span style="color:#a71d5d">&lt;</span><span style="color:#ff3399"></span><span style="color:#a71d5d">=</span>&nbsp;right;&nbsp;l<span style="color:#ff3399"></span><span style="color:#a71d5d">+</span><span style="color:#ff3399"></span><span style="color:#a71d5d">+</span>)</div><div style="padding:0 6px; white-space:pre; line-height:130%">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;sorted[k<span style="color:#ff3399"></span><span style="color:#a71d5d">+</span><span style="color:#ff3399"></span><span style="color:#a71d5d">+</span>]&nbsp;<span style="color:#ff3399"></span><span style="color:#a71d5d">=</span>&nbsp;list[l];</div><div style="padding:0 6px; white-space:pre; line-height:130%">&nbsp;&nbsp;}</div><div style="padding:0 6px; white-space:pre; line-height:130%">&nbsp;&nbsp;<span style="color:#a71d5d">else</span>{</div><div style="padding:0 6px; white-space:pre; line-height:130%">&nbsp;&nbsp;&nbsp;&nbsp;<span style="color:#a71d5d">for</span>&nbsp;(l&nbsp;<span style="color:#ff3399"></span><span style="color:#a71d5d">=</span>&nbsp;i;&nbsp;l&nbsp;<span style="color:#ff3399"></span><span style="color:#a71d5d">&lt;</span><span style="color:#ff3399"></span><span style="color:#a71d5d">=</span>&nbsp;mid;&nbsp;l<span style="color:#ff3399"></span><span style="color:#a71d5d">+</span><span style="color:#ff3399"></span><span style="color:#a71d5d">+</span>)</div><div style="padding:0 6px; white-space:pre; line-height:130%">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;sorted[k<span style="color:#ff3399"></span><span style="color:#a71d5d">+</span><span style="color:#ff3399"></span><span style="color:#a71d5d">+</span>]&nbsp;<span style="color:#ff3399"></span><span style="color:#a71d5d">=</span>&nbsp;list[l];</div><div style="padding:0 6px; white-space:pre; line-height:130%">&nbsp;&nbsp;}</div><div style="padding:0 6px; white-space:pre; line-height:130%">&nbsp;</div><div style="padding:0 6px; white-space:pre; line-height:130%">&nbsp;&nbsp;<span style="color:#999999">//&nbsp;배열&nbsp;sorted[](임시&nbsp;배열)의&nbsp;리스트를&nbsp;배열&nbsp;list[]로&nbsp;재복사</span></div><div style="padding:0 6px; white-space:pre; line-height:130%">&nbsp;&nbsp;<span style="color:#a71d5d">for</span>(l&nbsp;<span style="color:#ff3399"></span><span style="color:#a71d5d">=</span>&nbsp;left;&nbsp;l&nbsp;<span style="color:#ff3399"></span><span style="color:#a71d5d">&lt;</span><span style="color:#ff3399"></span><span style="color:#a71d5d">=</span>&nbsp;right;&nbsp;l<span style="color:#ff3399"></span><span style="color:#a71d5d">+</span><span style="color:#ff3399"></span><span style="color:#a71d5d">+</span>){</div><div style="padding:0 6px; white-space:pre; line-height:130%">&nbsp;&nbsp;&nbsp;&nbsp;list[l]&nbsp;<span style="color:#ff3399"></span><span style="color:#a71d5d">=</span>&nbsp;sorted[l];</div><div style="padding:0 6px; white-space:pre; line-height:130%">&nbsp;&nbsp;}</div><div style="padding:0 6px; white-space:pre; line-height:130%">}</div><div style="padding:0 6px; white-space:pre; line-height:130%">&nbsp;</div><div style="padding:0 6px; white-space:pre; line-height:130%"><span style="color:#a71d5d">void</span>&nbsp;mergeSort(<span style="color:#066de2">int</span>&nbsp;list[],&nbsp;<span style="color:#066de2">int</span>&nbsp;left,&nbsp;<span style="color:#066de2">int</span>&nbsp;right){</div><div style="padding:0 6px; white-space:pre; line-height:130%">&nbsp;</div><div style="padding:0 6px; white-space:pre; line-height:130%">&nbsp;&nbsp;<span style="color:#a71d5d">if</span>&nbsp;(left&nbsp;<span style="color:#ff3399"></span><span style="color:#a71d5d">&lt;</span>&nbsp;right)&nbsp;{</div><div style="padding:0 6px; white-space:pre; line-height:130%">&nbsp;&nbsp;&nbsp;&nbsp;<span style="color:#066de2">int</span>&nbsp;mid&nbsp;<span style="color:#ff3399"></span><span style="color:#a71d5d">=</span>&nbsp;(left&nbsp;<span style="color:#ff3399"></span><span style="color:#a71d5d">+</span>&nbsp;right)&nbsp;<span style="color:#ff3399"></span><span style="color:#a71d5d">/</span>&nbsp;<span style="color:#0099cc">2</span>;</div><div style="padding:0 6px; white-space:pre; line-height:130%">&nbsp;&nbsp;&nbsp;&nbsp;mergeSort(list,&nbsp;left,&nbsp;mid);</div><div style="padding:0 6px; white-space:pre; line-height:130%">&nbsp;&nbsp;&nbsp;&nbsp;mergeSort(list,&nbsp;mid<span style="color:#ff3399"></span><span style="color:#a71d5d">+</span><span style="color:#0099cc">1</span>,&nbsp;right);</div><div style="padding:0 6px; white-space:pre; line-height:130%">&nbsp;&nbsp;&nbsp;&nbsp;merge(list,&nbsp;left,&nbsp;mid,&nbsp;right);</div><div style="padding:0 6px; white-space:pre; line-height:130%">&nbsp;&nbsp;}</div><div style="padding:0 6px; white-space:pre; line-height:130%">}</div></div><div style="text-align:right;margin-top:-13px;margin-right:5px;font-size:9px;font-style:italic"><a href="http://colorscripter.com/info#e" target="_blank" style="color:#e5e5e5text-decoration:none">Colored by Color Scripter</a></div></td><td style="vertical-align:bottom;padding:0 2px 4px 0"><a href="http://colorscripter.com/info#e" target="_blank" style="text-decoration:none;color:white"><span style="font-size:9px;word-break:normal;background-color:#e5e5e5;color:white;border-radius:10px;padding:1px">cs</span></a></td></tr></table></div>



## 5. 힙 정렬 (Heap Sort)

힙정렬은 자료구조 **힙**을 사용하는 정렬방법입니다

1. 원소들을 전부 힙에 삽입한다.
2. 힙의 루트에 있는 값은 남은 수들 중에서 최솟값(혹은 최댓값)을 가지므로 루트를 출력하고 힙에서 제거한다.
3. 힙이 빌 때까지 2의 과정을 반복한다.

<img src='./images/types-of-heap.png'/>

### 특징

#### 장점

1. 시간 복잡도가 좋은편
2. 힙 정렬이 가장 유용한 경우는 전체 자료를 정렬하는 것이 아니라 가장 큰 값 몇개만 필요할 때 이다.

### 시간복잡도

시간복잡도를 계산한다면

* 힙 트리의 전체 높이가 거의 log₂n(완전 이진 트리이므로)이므로 하나의 요소를 힙에 삽입하거나 삭제할 때 힙을 재정비하는 시간이 log₂n만큼 소요된다.
* 요소의 개수가 n개 이므로 전체적으로 O(nlog₂n)의 시간이 걸린다.
* T(n) = O(nlog₂n)

## 6. 퀵 정렬 (Quick Sort)

분할 정복 알고리즘의 하나, 평균적으로 매우 빠른 수행 속도를 자랑하는 정렬 방법
분할 정복 알고리즘의 하나로, 평균적으로 **매우 빠른 수행 속도**를 자랑하는 정렬 방법
합병 정렬(merge sort)과 달리 퀵 정렬은 리스트를 **비균등하게** 분할한다.

### 특징

분할 정복 알고리즘의 하나로, 평균적으로 매우 빠른 수행 속도를 자랑하는 정렬 방법
합병 정렬(merge sort)과 달리 퀵 정렬은 리스트를 비균등하게 분할한다.

#### 장점

* 속도가 빠르다.
  * 시간 복잡도가 O(nlog₂n)를 가지는 다른 정렬 알고리즘과 비교했을 때도 가장 빠르다.
* 추가 메모리 공간을 필요로 하지 않는다.
  * 퀵 정렬은 O(log n)만큼의 메모리를 필요로 한다.

#### 단점
* 정렬된 리스트에 대해서는 퀵 정렬의 불균형 분할에 의해 오히려 수행시간이 더 많이 걸린다.

퀵 정렬의 불균형 분할을 방지하기 위하여 피벗을 선택할 때 더욱 리스트를 균등하게 분할할 수 있는 데이터를 선택한다.
EX) 리스트 내의 몇 개의 데이터 중에서 크기순으로 중간 값(medium)을 피벗으로 선택한다

### 구현 코드

<div class="colorscripter-code" style="color:#010101;font-family:Consolas, 'Liberation Mono', Menlo, Courier, monospace !important; position:relative !important;overflow:auto"><table class="colorscripter-code-table" style="margin:0;padding:0;border:none;background-color:#fafafa;border-radius:4px;" cellspacing="0" cellpadding="0"><tr><td style="padding:6px;border-right:2px solid #e5e5e5"><div style="margin:0;padding:0;word-break:normal;text-align:right;color:#666;font-family:Consolas, 'Liberation Mono', Menlo, Courier, monospace !important;line-height:130%"><div style="line-height:130%">1</div><div style="line-height:130%">2</div><div style="line-height:130%">3</div><div style="line-height:130%">4</div><div style="line-height:130%">5</div><div style="line-height:130%">6</div><div style="line-height:130%">7</div><div style="line-height:130%">8</div><div style="line-height:130%">9</div><div style="line-height:130%">10</div><div style="line-height:130%">11</div><div style="line-height:130%">12</div><div style="line-height:130%">13</div><div style="line-height:130%">14</div><div style="line-height:130%">15</div><div style="line-height:130%">16</div><div style="line-height:130%">17</div><div style="line-height:130%">18</div></div></td><td style="padding:6px 0;text-align:left"><div style="margin:0;padding:0;color:#010101;font-family:Consolas, 'Liberation Mono', Menlo, Courier, monospace !important;line-height:130%"><div style="padding:0 6px; white-space:pre; line-height:130%"><span style="color:#a71d5d">void</span>&nbsp;quickSort(<span style="color:#066de2">int</span>&nbsp;list[],&nbsp;<span style="color:#066de2">int</span>&nbsp;i,&nbsp;<span style="color:#066de2">int</span>&nbsp;j)</div><div style="padding:0 6px; white-space:pre; line-height:130%">{</div><div style="padding:0 6px; white-space:pre; line-height:130%">&nbsp;&nbsp;&nbsp;&nbsp;<span style="color:#066de2">int</span>&nbsp;left&nbsp;<span style="color:#ff3399"></span><span style="color:#a71d5d">=</span>&nbsp;i;</div><div style="padding:0 6px; white-space:pre; line-height:130%">&nbsp;&nbsp;&nbsp;&nbsp;<span style="color:#066de2">int</span>&nbsp;right&nbsp;<span style="color:#ff3399"></span><span style="color:#a71d5d">=</span>&nbsp;j;</div><div style="padding:0 6px; white-space:pre; line-height:130%">&nbsp;&nbsp;&nbsp;&nbsp;<span style="color:#a71d5d">if</span>&nbsp;(left&nbsp;<span style="color:#ff3399"></span><span style="color:#a71d5d">&gt;</span><span style="color:#ff3399"></span><span style="color:#a71d5d">=</span>&nbsp;right)&nbsp;<span style="color:#a71d5d">return</span>;</div><div style="padding:0 6px; white-space:pre; line-height:130%">&nbsp;&nbsp;&nbsp;&nbsp;<span style="color:#066de2">int</span>&nbsp;pivot&nbsp;<span style="color:#ff3399"></span><span style="color:#a71d5d">=</span>&nbsp;list[(left<span style="color:#ff3399"></span><span style="color:#a71d5d">+</span>right)<span style="color:#ff3399"></span><span style="color:#a71d5d">/</span><span style="color:#0099cc">2</span>];</div><div style="padding:0 6px; white-space:pre; line-height:130%">&nbsp;&nbsp;&nbsp;&nbsp;</div><div style="padding:0 6px; white-space:pre; line-height:130%">&nbsp;&nbsp;&nbsp;&nbsp;<span style="color:#a71d5d">while</span>&nbsp;(left&nbsp;<span style="color:#ff3399"></span><span style="color:#a71d5d">&lt;</span><span style="color:#ff3399"></span><span style="color:#a71d5d">=</span>&nbsp;right)&nbsp;{</div><div style="padding:0 6px; white-space:pre; line-height:130%">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span style="color:#a71d5d">while</span>&nbsp;(list[left]&nbsp;<span style="color:#ff3399"></span><span style="color:#a71d5d">&lt;</span>&nbsp;pivot)&nbsp;left<span style="color:#ff3399"></span><span style="color:#a71d5d">+</span><span style="color:#ff3399"></span><span style="color:#a71d5d">+</span>;</div><div style="padding:0 6px; white-space:pre; line-height:130%">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span style="color:#a71d5d">while</span>&nbsp;(list[right]&nbsp;<span style="color:#ff3399"></span><span style="color:#a71d5d">&gt;</span>&nbsp;pivot)&nbsp;right<span style="color:#ff3399"></span><span style="color:#a71d5d">-</span><span style="color:#ff3399"></span><span style="color:#a71d5d">-</span>;</div><div style="padding:0 6px; white-space:pre; line-height:130%">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span style="color:#a71d5d">if</span>&nbsp;(left&nbsp;<span style="color:#ff3399"></span><span style="color:#a71d5d">&lt;</span><span style="color:#ff3399"></span><span style="color:#a71d5d">=</span>&nbsp;right)&nbsp;{</div><div style="padding:0 6px; white-space:pre; line-height:130%">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;swap(list[left],&nbsp;list[right]);</div><div style="padding:0 6px; white-space:pre; line-height:130%">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;left<span style="color:#ff3399"></span><span style="color:#a71d5d">+</span><span style="color:#ff3399"></span><span style="color:#a71d5d">+</span>;&nbsp;right<span style="color:#ff3399"></span><span style="color:#a71d5d">-</span><span style="color:#ff3399"></span><span style="color:#a71d5d">-</span>;</div><div style="padding:0 6px; white-space:pre; line-height:130%">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;}</div><div style="padding:0 6px; white-space:pre; line-height:130%">&nbsp;&nbsp;&nbsp;&nbsp;}</div><div style="padding:0 6px; white-space:pre; line-height:130%">&nbsp;&nbsp;&nbsp;&nbsp;quickSort(i,&nbsp;right);</div><div style="padding:0 6px; white-space:pre; line-height:130%">&nbsp;&nbsp;&nbsp;&nbsp;quickSort(left,&nbsp;j);</div><div style="padding:0 6px; white-space:pre; line-height:130%">}</div></div><div style="text-align:right;margin-top:-13px;margin-right:5px;font-size:9px;font-style:italic"><a href="http://colorscripter.com/info#e" target="_blank" style="color:#e5e5e5text-decoration:none">Colored by Color Scripter</a></div></td><td style="vertical-align:bottom;padding:0 2px 4px 0"><a href="http://colorscripter.com/info#e" target="_blank" style="text-decoration:none;color:white"><span style="font-size:9px;word-break:normal;background-color:#e5e5e5;color:white;border-radius:10px;padding:1px">cs</span></a></td></tr></table></div>



## 7. 카운팅 정렬 (Counting Sort)

카운팅 정렬은 가장 큰 데이터에 따라 효율이 좌지우지된다. 쉽게 설명하자면 특정 데이터의 개수(1이 두 개 있다면 2)를 데이터의 값에 대응하는 위치에 저장한 뒤, 자신의 위치에서 앞에 있던 값을 모두 더한 배열을 만든 뒤, 거기서 데이터가 들어가야 할 위치를 찾아내는 정렬 알고리즘이다. 이 경우에 데이터의 최댓값을 k라 두면, 시간 복잡도는 *O*(*n*+*k*). 하지만, 만약 k가 억 단위를 넘어간다면? n이 아무리 작아도 동작시간이 크다. 그럴 땐 위의 정렬들을 사용하는 게 바람직하다. 반대로 k가 매우 작다면, 오히려 선형시간의 효과를 볼 수 있다. 즉, k가 작다는 조건이라면 매우 효율적인 정렬. 또한 카운팅 정렬은 배열을 사용하는 특성상, 정수라는 전제를 깔고 한다.

1. 자료를 탐색해서 그 최댓값을 구한다.
   - `input = [1, 5, 4, 6, 3, 7, 8, 9, 10, 2]`
   - 최댓값: `k = 10`
2. `k+1`만큼의 크기로 모든 자료가 0으로 초기화된 배열을 생성한다.
   - 배열 `counts = [0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0]` (11개)
3. `input`의 모든 원소 `n`에 대하여 `counts`의 `n`에 대응하는 곳에 +1을 해준다.
   - `counts = [0, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1]`
   - 이때 `counts[n]`은 배열 `input`에 n이 몇 개 있는지를 의미한다.
4. `counts[i] += counts[i-1]`의 [점화식](https://namu.wiki/w/%EC%A0%90%ED%99%94%EC%8B%9D)을 1부터 k의 위치까지 행한다.
   - `counts = [0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10]`
   - 이때 `counts[n]`은 배열 `input`에 n 이하의 원소가 몇 개 있는지를 의미한다.
5. 길이가 `counts[k]`인 배열을 하나 더 생성한다.
   - 배열 `ans = [N, N, N, N, N, N, N, N, N, N]` (10개, N은 [Null](https://namu.wiki/w/Null#s-2))
6. `counts`의 `input[0]`에 대응하는 곳의 원소를 찾아서 t로 놓는다. 이제 `ans`의 `t-1`에 대응하는 곳에 `input[0]`을 저장하고, `counts`의 `input[0]`에 대응하는 곳의 값은 -1 해준다.
   - 1이 주어짐
   - `counts[1] = 1`
   - 대응하는 값인 1-1=0의 위치에 1을 삽입
   - `ans = [1, N, N, N, N, N, N, N, N, N]`
   - `counts`는 `[0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10]`에서 `[0, 0, 2, 3, 4, 5, 6, 7, 8, 9, 10]`로 바뀜
7. 6의 과정을 남아 있는 자료에 대하여 반복한다.
   - 5가 주어짐
   - `counts[5] = 5`
   - 대응하는 값인 5-1=4의 위치에 5를 삽입
   - `ans = [1, N, N, N, 5, N, N, N, N, N]`
   - `counts`는 `[0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10]`에서 `[0, 0, 2, 3, 4, 4, 6, 7, 8, 9, 10]`로 바뀜
   - ...
8. 이런 식으로 n개의 자료를 모두 조사하면 `[1, 2, 3, 4, 5, 6, 7, 8, 9, 10]`순서로 정렬이 된다.

## 8. 쉘 정렬 (Shell Sort)

가장 오래된 정렬 알고리즘의 하나로, 삽입정렬을 보완한 알고리즘

* ‘Donald L. Shell’이라는 사람이 제안한 방법으로, 삽입정렬을 보완한 알고리즘이다.
* 삽입 정렬이 어느 정도 정렬된 배열에 대해서는 대단히 빠른 것에 착안
  * 삽입 정렬의 최대 문제점: 요소들이 삽입될 때, 이웃한 위치로만 이동
  * 즉, 만약 삽입되어야 할 위치가 현재 위치에서 상당히 멀리 떨어진 곳이라면 많은 이동을 해야만 제자리로 갈 수 있다.
  * 삽입 정렬과 다르게 셸 정렬은 전체의 리스트를 한 번에 정렬하지 않는다.
* 과정 설명
  * 먼저 정렬해야 할 리스트를 일정한 기준에 따라 분류
  * 연속적이지 않은 여러 개의 부분 리스트를 생성
  * 각 부분 리스트를 삽입 정렬을 이용하여 정렬
  * 모든 부분 리스트가 정렬되면 다시 전체 리스트를 더 적은 개수의 부분 리스트로 만든 후에 알고리즘을 반복
  * 위의 과정을 부분 리스트의 개수가 1이 될 때까지 반복

 ### 특징
#### 장점

* 연속적이지 않은 부분 리스트에서 자료의 교환이 일어나면 더 큰 거리를 이동한다. 따라서 교환되는 요소들이 삽입 정렬보다는 최종 위치에 있을 가능성이 높아진다.

* 부분 리스트는 어느 정도 정렬이 된 상태이기 때문에 부분 리스트의 개수가 1이 되게 되면 셸 정렬은 기본적으로 삽입 정렬을 수행하는 것이지만 삽입 정렬보다 더욱 빠르게 수행된다.

* 알고리즘이 간단하여 프로그램으로 쉽게 구현할 수 있다.

### 시간복잡도
시간복잡도를 계산한다면

* 평균: T(n) = O(n^1.5)
* 최악의 경우: T(n) = O(n^2)

## 9. 기수 정렬 (Radix Sort)

  위에 나온 알고리즘은 모두 데이터끼리의 직접적인 비교를 이용하는데, 이렇게 데이터끼리 직접적인 비교를 하여 정렬할 경우 시간복잡도는 *O*(*n*log*n*)보다 작아질 수 없다. 이 기수 정렬은 자릿수가 있는 데이터(정수, 문자열 등)에서만 수행이 가능하며, **데이터끼리의 직접적인 비교 없이** 정렬을 수행한다. 비교를 이용한 정렬이 아니기 때문에 k가 상수일 경우 시간복잡도가 *O*(*n*)으로 **퀵정렬보다 빠른 시간복잡도**가 나오는 것이 가능하다. 어떻게 데이터끼리 비교하지도 않고 정렬을 할 수 있는지는 후술. 다만 이 알고리즘은 자릿수가 적은 4바이트 정수 등에서나 제대로 된 성능을 발휘할 수 있으며, 자릿수가 무제한에 가까운 문자열 정렬 등에 사용할 경우 오히려 퀵정렬보다 느릴 수 있고, 부동 소수점의 경우는 부호여부, 지수부, 가수부에 대해 각각 기수정렬을 실행해야 한다.

기수 정렬의 방식은 대충 이렇다. 데이터가 x진법이라고 가정하자. 0번부터 x-1번까지의 리스트를 만들어 놓고, 각 데이터를 순서대로 현재 자릿수의 숫자가 가리키는 리스트에 밀어넣고, 리스트를 0번부터 x-1번까지 순서대로 이어붙인다. 이 과정을 가장 낮은 자릿수부터 가장 높은 자릿수까지 반복하면 정렬이 끝나게 된다.

(예시) 10, 5, 15, 234, 1: 10진법, 최대 3자리 수인 정수들을 정렬해보자. 편의상 010, 005, 015, 234, 001로 표기.

1의 자리: 0) 010, 1) 001, 4) 234, 5) 005, 015. 순서대로 이어붙이면 010, 001, 234, 005, 015.
10의 자리: 0) 001, 005, 1) 010, 015, 3) 234. 순서대로 이어붙이면 001, 005, 010, 015, 234.
100의 자리: 0) 001, 005, 010, 015, 2) 234. 순서대로 이어붙이면 001, 005, 010, 015, 234.

보다시피 마지막 자리까지 과정을 거치면 정렬된 결과인 1, 5, 10, 15, 234가 나온다. 실제 프로그램에서 정수를 정렬하는 코드를 짜는 경우, 10진법 대신 컴퓨터가 바이트 단위로 처리하기 쉬운 2진법을 사용하고, 각 자릿수를 정렬하는 과정은 밑의 카운팅 정렬을 이용하는 경우가 많다. 그렇게 코드를 짜면 퀵 정렬보다도 훨씬 빠른 속도로 정수를 정렬할 수 있다. 다만 위에 설명된 대로 이는 자릿수가 적은 상황에서만 그렇고 대부분의 경우에는 그렇지 않다.

그 이유는 시간 복잡도가 엄밀하게는 *O*(*n*)이 아니라 O(kn)이기 때문이다. 해당 데이터의 자릿수(k)는 상수로 간주하지만 어쨌든 k는 log에 해당되므로 데이터의 수(n)가 데이터의 최댓값보다 더 큰 경dn가 아니면 k(자릿수)가 nlog*n* 보다 크기 때문이다.  

### reference

* [정렬 알고리즘::나무위키](https://namu.wiki/w/%EC%A0%95%EB%A0%AC%20%EC%95%8C%EA%B3%A0%EB%A6%AC%EC%A6%98)
* <https://gmlwjd9405.github.io/2018/05/10/algorithm-quick-sort.html>


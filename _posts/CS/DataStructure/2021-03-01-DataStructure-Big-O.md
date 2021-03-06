---
title: "DataStructure : Big-O 표기법"
excerpt_separator: <!--more-->
categories:
  - CS(Computer Science)
  - DataStructure
tags:
  - CS(Computer Science)
  - DataStructure
  - "DataStructure : Big-O"
toc: true
toc_sticky: true
toc_label: 목차
---

# Big-O 표기법

**알고리즘의 성능을 수학적으로 표현**해주는 표기법입니다. Big-O 표기법으로 알고리즘의 시간과 공간복잡도를 표현할 수 있습니다. Big-O 표기법은 알고리즘의 실제 러닝타임을 표시하기보다 **데이터나 사용자의 증가율에 따른 알고리즘의 성능을 예측하는 게 목표**이기 때문에 상수와 같은 숫자들은 모두 1회가 됩니다.



## O(1)

입력데이터의 크기에 상관없이 **언제나 일정한 시간이 걸리는 알고리즘을 표현**할 때 사용합니다.

```python
F(int[] n) {
	return (n[] == 0)? true : false
}
```

함수를 보면 배열이 얼마나 큰지에 상관없이 언제나 일정한 속도로 결과를 반환합니다. 이런 경우에 `O(1)의 시간복잡도를 가진다`라고 표한합니다.



![O(1)](https://user-images.githubusercontent.com/79291114/109441909-009fbf00-7a7a-11eb-8709-39ca4cc76bb1.jpg)

O(1)알고리즘을 그래프로 그려보면 가로축이 `데이터의 크기`고 세로축을 `처리시간`이라고 할 때 데이터가 증가함에 따라 성능에 변화가 없습니다.



## O(n)

**입력 데이터의 크기에 비례해서 처리시간이 걸리는 알고리즘을 표현**할 때 사용합니다.

```python
F(int[] n) {
	for i = 0 to n.length
		print i
}
```

함수를 보면 n개의 데이터를 받으면 n번 루프를 돌기 때문에 n이 하나 늘어날 때 마다 처리가 한 번씩 늘어나서 n의 크기만큼 처리시간이 늘어나게 됩니다.

 

![O(n)](https://user-images.githubusercontent.com/79291114/109442134-aa7f4b80-7a7a-11eb-8aaf-ae81a7f36653.jpg)

O(n)의 그래프는 **데이터가 증가함에 따라 비례해서 처리시간도 같이 증가**합니다. 언제나 데이터나 시간이 같은 비율로 증가합니다.



## O(n<sup>2</sup>)

```python
F(int[] n) {
	for i = 0 to n.length
		for j = 0 to n.length
			print i + j;
}
```

n을 루프를 돌리면서 그 안에서 n을 루프로 또 돌릴 때 n 스퀘어가 됩니다.



![O(n2)](https://user-images.githubusercontent.com/79291114/109442294-1d88c200-7a7b-11eb-9ed5-388aa4fd5630.jpg)

n개의 데이터를 받으면 첫 번째 루프에서 n번 돌면서 각각의 엘리먼트에서 n번씩 또 도니까 처리 횟수가 n을 가로 세로 길이를 가지는 면적만큼 됩니다. n이 하나 늘어날 때마다 아래 한 줄, 오른쪽 한 줄 n만큼씩 계속 붙으니까 **데이터가 커지면 커질수록 처리시간의 부담도 커집니다.**



![O(n2)그래프](https://user-images.githubusercontent.com/79291114/109442401-735d6a00-7a7b-11eb-8152-49a9169e14e5.jpg)

n 스퀘어를 그래프로 그리면 처음에는 조금씩 상승하다가 **나중에 가면 하나 추가할 때마다 그래프가 수직상승**이 됩니다.



## O(nm)

```python
F(int[] n, int[] m) {
	for i = 0 to n.length
		for j = 0 to m.length
			print i + j;
}
```

코드가 n스퀘어랑 비슷하지만 n을 두 번 돌리는 게 아니고 **n을 m만큼 돌립니다.** 루프 모양 2개 겹쳐져 있는 것만 보고 n스퀘어구나 하고 실수하기 쉽습니다.



![O(nm)](https://user-images.githubusercontent.com/79291114/109442541-dd760f00-7a7b-11eb-9dbd-4e357ec34e62.jpg)

n은 엄청크고 m은 아주 작을 수도 있기 때문에 `변수가 다르다면 Big-O표기법에도 반드시 다르게 표시`를 해주어야 합니다.



![O(nm)그래프](https://user-images.githubusercontent.com/79291114/109442639-1f06ba00-7a7c-11eb-940b-03c360146584.jpg)

nm도 n스퀘어와 마찬가지로 **데이터가 증가할수록 그래프가 점점 수직에 가까운 모양이 됩니다.**



## O(n<sup>3</sup>)

```python
F(int[] n) {
	for i = 0 to n.length
		for j = 0 to n.length
			for k = 0 to n.lenghth
				print i + j + k;
}
```

n의 제곱에 n을 한번 더 돌리면 n의 세제곱이 됩니다.



![O(n3)](https://user-images.githubusercontent.com/79291114/109443039-e4515180-7a7c-11eb-89ee-70ce0d033f5e.jpg)

O(n)일 때는 직선, n의 제곱이면 면적이 됩니다. 그렇다면 n의 세제곱은 n의 제곱을 n만큼 더 돌리니까 큐빅이 되겠습니다.



![O(n3)그래프](https://user-images.githubusercontent.com/79291114/109443117-1662b380-7a7d-11eb-86fa-e82e270310b7.jpg)

O(n<sup>3</sup>)은 n스퀘어랑 비슷한 양상을 보이지만 데이터가 늘어날 때마다 가로 세로의 높이까지 더 해 지기때문에 **데이터가 증가함에 따라 더 급격하게 처리시간이 늘어나**는 걸 확인할 수 있습니다.



## O(2<sup>n</sup>)

O(n<sup>2</sup>)과 비슷하게 생겼는데 둘은 너무 다른 알고리즘입니다 . O(2<sup>n</sup>)은 `피보나치 수열`을 이용하게 됩니다. 그 밖의 m개씩 n번 늘어나는 알고리즘을 표현할 때는 O(m<sup>n</sup>)로 표현합니다.

```python
F(n, r) {
	if (n <= 0) return 0;
	else if (n == 1) return r[n] = 1;
	return r[n] = F(n - 1, r) + F(n - 2, r);
}
```

피보나치 수열을 구하는 코드는 재귀 함수를 이용해서 구현할 수 있습니다. 함수를 호출 할 때마다 바로 전 숫자와 전전의 숫자를 알아야 더 할 수 있습니다. 해당 숫자(전 숫자와 전전 숫자)를 알아내기 위해  1을 뺀 숫자를 가지고 재귀호출을 하고 2를 뺀 숫자를 가지고 한번 더 재귀호출을 하게 됩니다.

![FIbonacci9](https://user-images.githubusercontent.com/79291114/109444628-077e0000-7a81-11eb-9759-26aef35ec444.jpg)

이렇게 매번 함수가 호출될 때 마다 두 번씩 또 호출을 합니다. 이 작업을 트리의 높이(k)만큼 반복하게 됩니다.



![FIbonacci10](https://user-images.githubusercontent.com/79291114/109444621-051ba600-7a81-11eb-8569-2a6dc1f632af.jpg)

그래프를 보면 n의 **제곱이나 세제곱보다도 데이터의 증가에 따라 처리시간이 현저하게 늘어납니다.**



### 피보나치 수열

![FIbonacci](https://user-images.githubusercontent.com/79291114/109444622-064cd300-7a81-11eb-8074-84d5c5c7bd12.jpg)

1. 피보나치 수열은 1부터 시작해서 한 면을 기준으로 정사각형을 만듭니다.
2. 두개가 붙어서 큰 면이 2인 직사각형이 되었으면 큰 면을 기준으로 정사각형을 한개 더 붙여줍니다.
3. 그렇게 큰 면이 3인 직사각형이 되면 그 면을 기준으로 정사각형을 만들어 줍니다.

4. 이런 방식으로 한면이 8인 정사각형 까지 만들어주게 됩니다.



![FIbonacci7](https://user-images.githubusercontent.com/79291114/109444623-06e56980-7a81-11eb-9527-663281d4ab99.jpg)

그러면 피보나치의 나선 형이 그려지게 됩니다.



![FIbonacci8](https://user-images.githubusercontent.com/79291114/109444626-06e56980-7a81-11eb-8922-d0ffa687170d.jpg)

수학적으로 접근해 보겠습니다.

1. 피보나치 수열은 1부터 시작해서 1의 다른 한 면은 1을 한번 더 써줍니다.
2. 앞의 1 두 개를 더해서 2를 써줍니다.
3. 앞의 1과 2를 더해 3을 써줍니다.
4. 앞의 2와 3을 더해 5를 써주고 또, 3과 5를 더해 8을 써줍니다.



## O(log n)

O(log n)의 대표적인 알고리즘은 `이진 검색(Binary Search)` 입니다.



### 이진 검색(Binary Search)

![O(log n)](https://user-images.githubusercontent.com/79291114/109446291-be2faf80-7a84-11eb-908a-053f855e2aee.jpg)

오름차순으로 정렬된 배열 안에서 6을 찾아보겠습니다.

1. 정렬이 되어 있기 때문에 중간값을 찾아서 키 값과 비교합니다.
2. 이 중간값보다 키 값이 더 크면 오른쪽에 찾는 값이 있다는 뜻이기 때문에 왼쪽의 데이터들은 이제 보지 않아도 됩니다.
3. 뒤 쪽의 데이터들도 다시 중간값을 찾아서 키 값과 비교합니다.
4. 키 값이 더 작으면 왼쪽에 찾는 값이 있다는 뜻이기 때문에 오른쪽의 데이터들은 보지 않습니다.

5. 왼쪽에 찾는 값인 6이 있습니다.

이렇게 **한번 처리될 때마다 검색해야 하는 데이터의 양이 절반씩 떨어지는 알고리즘을 O(log n) 알고리즘** 이라고 합니다.



```python
F(K, arr, s, e) {
	if (s > e) return -1;
	m = (s + e) / 2;
	if (arr[m] == k) return m;
	else if (arr[m] > k) return F(k, arr, s, m-1);
	else return F(k, arr, m+1, e);
}
```

이진 검색의 코드를 보면 처음 호출될 때 시작 번호는 0, 끝은 배열의 맨 마지막이 됩니다. 주어진 배열의 중간값을 찾고 찾는 값이 중간값보다 작으면 중간 지점 바로 전까지 배열을 조정해서 다시 호출합니다. 찾는 값이 중간값보다 큰 경우에는 중간 지점 다음부터 맨 끝까지 배열을 조정해서 호출합니다.

함수가 호출될 때마다 중간값을 기준으로 절반은 검색 영역에서 제외시켜 버리기 때문에 순차 검색에 비교해서 속도가 현저하게 빠릅니다.



![O(log n)그래프](https://user-images.githubusercontent.com/79291114/109446287-bcfe8280-7a84-11eb-87e8-c1531866b4b4.jpg)

그래프를 보면 **O(log n)이 O(n)보다 훨씬 시간이 적게 드는 것**을 볼 수 있고 **데이터가 증가해도 성능이 크게 차이나지 않습니다.**



## O(sqrt(n))

O(sqrt(n))은 제곱근을 이용하게 됩니다.



![O(sqrt(n))](https://user-images.githubusercontent.com/79291114/109455172-5e8fcf00-7a99-11eb-8ff7-277732a5f1bf.jpg)

- n = 4라면 4를 정사각형에 채워서 맨 위에 한 줄이 제곱근이 됩니다.



![O(sqrt(n))2](https://user-images.githubusercontent.com/79291114/109455173-5f286580-7a99-11eb-9fa1-36513a5ce10d.jpg)

- n = 9라면 마찬가지로 9개의 수를 정사각형에 채우고 맨 위에 한 줄이 제곱근입니다.



![O(sqrt(n))3](https://user-images.githubusercontent.com/79291114/109455169-5df73880-7a99-11eb-9c22-a311ddd17f9f.jpg)

- n = 16도 마찬가지로 수를 다 채운 후 맨 위에 한 줄만 돌리는 알고리즘을 Big-O 표기법으로 하면 O(sqrt(n))입니다.



## Big-O 표기법에서 중요한 것

Big-O 표기법에서 `상수는 과감히 버립니다.`



```python
F(int[] n) {
	for i = 0 to n.length
		print i;
    for i = 0 to n.length
    	print i;
}
```

이 코드의 시간 복잡도는 n만큼씩 2번 돌리니까 2n만큼의 시간이 걸립니다. 그런데 Big-O표기법에서는 2n은 그냥 n으로 표시합니다. 그 이유는 Big-O표기법은 실제 알고리즘의 러닝타임을 재기위해 만든 게 아니고 장기적으로 데이터가 증가함에 따른 처리 시간의 증가율을 예측하기 위해서 만들어진 표기법 이기 때문입니다.

상수는 고정된 숫자여서 증가율에 영향을 미칠 때 언제나 고정된 상수만큼 씩만 영향을 미치기 때문에 Big-O 표기법에선 증가하지 않는 숫자는 신경쓰지 않습니다.



---

참고 : https://www.youtube.com/watch?v=6Iq5iMCVsXA&t=301s
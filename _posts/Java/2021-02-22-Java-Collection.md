---
title: "Java : Collection"
excerpt_separator: <!--more-->
categories:
  - Java
tags:
  - Java
  - "Java : Collection"
toc: true
toc_sticky: true
toc_label: 목차
---


# Collection
- 컬렉션이란 데이터의 집합, 그룹을 의미한다.
- JCF(Java Collections Framework)는 컬렉션과 이를 구현하는 클래스를 정의하는 인터페이스(공통된 메소드를 모아놓은)를 제공한다.
- Collection 인터페이스는 List, Set, Queue 크게 3가지 상위 인터페이스로 분류할 수 있다.
- Map의 경우 Collection 인터페이스를 상속받고 있진 않지만 Collection으로 분류된다.

![컬렉션](https://user-images.githubusercontent.com/58713853/73814627-f84d6400-4826-11ea-95dc-0de32bd8be6c.PNG)

- Collection 인터페이스의 특징

![컬렉션 특징](https://user-images.githubusercontent.com/58713853/73814690-229f2180-4827-11ea-8cf8-380b98c75a20.PNG)



## List 인터페이스

- 순서가 있는 데이터의 집합
- 색인을 사용하여 특정 위치에 요소를 삽입하거나 접근할 수 있으며 데이터의 중복을 허용한다.



### ArrayList
  - 상당히 빠르고 크기를 마음대로 조절할 수 있는 배열
  - 단방향 포인터 구조로 각 데이터에 대한 인덱스를 가지고 있어 조회 기능에 성능이 뛰어나다.
  - 초기에 생성하면 10개의 객체를 저장할 수 있는 List가 선언된다.

  ```java
  List<E> list = new ArrayList<E>(); // E에는 어떤 객체 타입이던 올 수 있다.
  ```



### Vector

  - 과거에 대용량 처리를 위해 사용한다.
  - 내부에서 자동으로 동기화처리가 일어나 비교적 성능이 좋지 않고 무거워 잘 쓰이지 않는다.

  ```java
  List<E> list = new Vector<E>();
  ```


### LinkedList

  - 양방향 포인터 구조로 인접 참조를 링크해서 체인처럼 관리한다.
  - 데이터의 삽입, 삭제가 빈번할 경우 데이터의 위치정보만 수정하면 되기에 유용하다. (빠른 성능을 보장)
  - 스택, 큐, 양방향 큐 등을 만들기 위한 용도로 쓰인다.
  - 초기에 생성하면 어떠한 링크도 만들어지지 않기 때문에 내부적으로 비어있다.

  ```java
  List<E> list = new LinkedList<E>();
  ```



## Set 인터페이스

- 순서를 유지하지 않는 데이터의 집합
  - 인덱스로 객체를 검색해서 가져오는 메소드가 없다.
  - 대신 전체 객체를 대상으로 한번씩 반복해서 가져오는 반복자(Iterator)를 제공한다.
- 데이터의 중복을 허용하지 않는다.



### 반복자(Iterator)
- 메소드

리턴 타입 | 메소드 | 설명
:-----:|:-----:|:----------
boolean | hasNext() | 가져올 객체가 있으면 true를 리턴하고 없으면 false
Object | next() | 컬렉션에서 하나의 객체를 가져온다.
void | remove() | 컬렉션에서 객체를 제거한다.



- 사용방법



```java
// 기본적인 Iterator 인터페이스 선언
Set<E> set = ...;
Iterator<E> iterator = set.iterator();

// 방법 1. Iterator를 사용한방법
while(iterator.hasNext()){  // hasNext가 다음 객체가 있는지 체크해 주기 때문에 객체가 있는 만큼 루프를 돈다.
  E str = iterator.next();
}
// 방법 2. For문 사용
for(E e : set){ // 저장된 객체수만큼 루프를 돈다.
  
}
```




### HashSet

  - 가장빠른 임의 접근 속도
  - 순서를 예측할 수 없다.
```java
Set<E> set = new HashSet<E>();
```
  - HashSet이 판단하는 동일한 객체란 꼭 같은 인스턴스를 뜻하지 않는다.
    - hashCode와 equals를 재정의해서 동일한 리턴 값을 가지면 동등한 객체로 간주한다.

![HashSet](https://user-images.githubusercontent.com/58713853/73819147-49af2080-4832-11ea-9175-536cef802708.PNG)
    
### LinkedHashSet
  - 추가된 순서 또는 가장 최근에 접근한 순서대로 접근 가능



## Map 인터페이스
- 키(Key), 값(Value)의 쌍으로 이루어진 데이터의 집합
- 순서는 유지되지 않으며 키(Key)의 중복을 허용하지 않으나 값(Value)의 중복은 허용한다.
- 맵 안에 데이터를 하나하나 얻어 내는 방법



```java
Map<K,V> map = new HashMap<>();

// 방법1. Key를 Set 타입으로 뽑아, Iterator를 이용해 Key 값으로 value를 얻는 방법
final Set<K> e = map.keySet();
final Iterator<K> iterator = e.iterator();
while(iterator.hasNext()){
  K key = iterator.next();
  V value = map.get(key);
  
// 방법2. entrySet을 통해 Entry객체를 Set타입으로 뽑아서 key와 value를 동시에 얻는 방법
final Set<Map.Entry<K, V>> entries = map.entrySet();
final Iterator<Map.Entry<K, V>> iterator = entries.iterator();
while(iterator.hasNext()){
  final Map.Entry<K, V> mapEntry = iterator.next();
  mapEntry.getKey();
  mapEntry.getValue();
}
```



### HashMap

  - ~Map 인터페이스를 구현하기 위해 해시테이블을 사용한 클래스 (아직 이게 무슨의미인지 파악하지 못함..)~
  - 중복과 순서가 허용되지 않는다.
  - 키와 값으로 nll값이 올 수 있다.
  - HashSet과 마찬가지로 hashCode와 equals를 재정의해서 동일한 리턴 값을 가지면 동등한 객체로 간주한다.
```java
Map<K, V> map = new HashMap<K, V>();
```


### Hashtable

- HashMap과 동일한 내부구조를 가진다.
- HashMap 보다는 느리지만 동기화 지원
  - 동기화된 메소드로 구성되어 있기때문에 멀티스레드가 동시에 이 메소드들을 실행할 수 없다.
  - 하나의 스레드가 실행을 완료해야 다른 스레드가 실행할 수 있다.
- 키와 값으로 null값이 허용되지 않는다.
```java
Map<K, V> map = new Hashtable<K, V>();
```


### LinkedHashMap

- 기본적으로 HashMap을 상속받아 HashMap과 매우 흡사
- Map에 있는 엔트리들의 연결 리스트가 유지되므로 입력한 순서대로 반복 가능
```java
Map<K, V> map = new LinkedHashMap<K, V>();
```



### Properties

- Hashtable의 하위 클래스이기 때문에 Hashtable의 모든 특징을 그대로 가지고 있다.
- Hashtable은 키와 값을 다양한 타입으로 지정이 가능하지만 Properties는 키와 값을 String 타입으로 제한한 컬렉션이다.
```java
Properties properties = new Properties();
```


## 이진 트리 구조

- 이진 트리는 여러개의 노드가 트리형태로 이루어져 연결된 구조이다.
- 검색 기능을 강화시킨 구조이다.
- 루트 노드라고 불리는 하나의 노드에서부터 시작해, 각 노드에 최대 2개의 노드를 연결할 수 있는 구조를 가진다.

![이진트리구조](https://user-images.githubusercontent.com/58713853/73824258-2fc70b00-483d-11ea-833f-4ae7311c603d.PNG)

- 부모 노드의 값보다 작은 노드는 왼쪽에 위치 시키고 큰 노드는 오른쪽에 위치시킨다.
- 숫자가 아닌 문자가 저장할경우에는 문자의 유니코드 값으로 비교한다.
- 예를 들어 6,3,9,2,5의 순서로 값을 저장하면 다음과 같은 순서로 진행된다.

![이진트리진행구조](https://user-images.githubusercontent.com/58713853/73824347-56854180-483d-11ea-9ba0-a770b9376b58.PNG)

- 아래와 같이 값들이 정렬되어 있어 그룹핑이 쉽기 때문에 범위 검색을 쉽게할 수 있다.

![이진트리범위검색](https://user-images.githubusercontent.com/58713853/73826092-871aaa80-4840-11ea-9545-27c4679f8267.PNG)

## TreeSet
- 하나의 노드는 노드 값인 value와 왼쪽과 오른쪽 자식노드를 참조하기 위한 두 개의 변수로 구성된다.
- 객체를 저장하면 자동으로 정렬된다.
- 정렬된 순서대로 보관하며 정렬방법을 지정할 수 있다.

```java
TreeSet<E> treeSet = new TreeSet<E>();
```

![TreeSet](https://user-images.githubusercontent.com/58713853/73826479-30fa3700-4841-11ea-8c4d-300f5db522db.PNG)  



### TreeMap

- 키와 값의 쌍으로 이루어진 데이터 MapEntry를 저장
- 객체를 저장하면 기본적으로 부모 키값과 비교해서 키값이 낮은 것은 왼쪽, 높은 것은 오른쪽에 자동으로 정렬된다.
- 정렬된 순서대로 키(Key)와 값(Value)을 저장하여 검색이 빠르다.
- 저장시 정렬(오름차순)을 하기 때문에 저장시간이 다소 오래 걸린다.
  
```java
TreeMap<K, V> treeMap = new TreeMap<K, V>();
```
![TreeMap](https://user-images.githubusercontent.com/58713853/73826910-f3e27480-4841-11ea-8d11-2f6d735f54d9.PNG)



## Comparable 과 Compartor

- TreeSet의 객체와 TreeMap의 키는 크기를 비교해 트리구조를 구성해야 하기 때문에 java.lang.Comparable 인터페이스를 구현해야 쓸 수 있다.



### Comparable
- 기본적인 Wrraper Class들은 Comparable 인터페이스가 구현되어 있어 TreeSet과 TreeMap을 사용할 수 있다.
- ArrayList 등의 자료구조의 Collection.sort()함수를 사용할때도 Comparable을 이용해 compareTo()메소드만 재정의하게되면 쉽게 Collection 프레임워크의 자료구조들을 정렬하여 사용할 수 있다.

리턴 타입 | 메소드 | 설명
:-----:|:-------:|:---------------
int | compareTo(Object o) | 주어진 객체와 같으면 0을 리턴 <br> 주어진 객체보다 작으면 음수를 리턴 <br> 주어진 객체보다 크면 양수를 리턴



### Compartor

- TreeSet, TreeMap이 Comparable을 구현하고 있지 않을 경우에는 저장하는 순간 ClassCastException이 발생한다.
- 이진 트리를 구성하기 위해서는 값 비교가 필수이기 때문에 Comparable 구현체를 구현하는 방법 외에 Comparator 인터페이스를 이용해 정렬할 수 있다.

```java
TreeSet<E> treeSet = new TreeSet<E>(new AscendingComparator());
TreeMap<K, V> treeMap = new TreeMap<K, V>(new DescendingComparator());
```



- 정렬자는 Comparator 인터페이스를 구현한 객체를 말하는데, Comparator 인터페이스는 다음과 같은 메소드가 정의되어 있다.

리턴 타입 | 메소드 | 설명
:-----:|:-------:|:---------------
int | compareTo(Obect o1, Object o2) | o1과 o2가 동등하다면 0을 리턴 <br> o1이 o2보다 앞에 오게하면 음수를 리턴 <br> o1이 o2보다 뒤에 오게하면 양수를 리턴



### Stack

- 후입선출 (LIFO : Last In First Out)으로 이루어진 자료구조 인터페이스이다.
```java
Stack<E> stack = new Stack<E>();
```



### Queue

- 선입선출 (FIFO : First In First Out)으로 이루어진 자료구조 인터페이스이다.
```java
Queue<E> queue = new LinkedList<E>();
```



---

참고 : https://gangnam-americano.tistory.com/41 \
https://hackersstudy.tistory.com/26 \
https://minwan1.github.io/2018/07/03/2018-07-03-collection/


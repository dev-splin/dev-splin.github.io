---
title: "Java : Collection"
excerpt_separator: <!--more-->
categories:
  - Java
tags:
  - Java
  - "Java : Collection"
  - "Java : List"
  - "Java : Queue"
  - "Java : Set"
  - "Java : Map"
toc: true
toc_sticky: true
toc_label: 목차
---


# Collection
- `Collection`이란 **어떠한 자료구조(Queue, Stack 등등...)를 구현한 클래스를 의미**합니다.
- `JCF(Java Collections Framework)`는 컬렉션과 이를 구현하는 클래스를 정의하는 `Interface(공통된 메소드를 모아놓은)`를 제공합니다.
- `Collection Interface`는 `List, Set, Queue` 크게 3가지 상위 `Interface`로 분류할 수 있습니다.
- `Map`의 경우 `Collection Interface`를 상속받고 있진 않지만 `Collection`으로 분류됩니다.



### Collection Interface특징

![컬렉션](https://user-images.githubusercontent.com/58713853/73814627-f84d6400-4826-11ea-95dc-0de32bd8be6c.PNG)

---

<img src="https://user-images.githubusercontent.com/79291114/115977688-29918b80-a5b5-11eb-85b1-84c72a9c9be9.png" alt="Collection" style="zoom: 50%;" />



### 자료 구조 성능 요약

밑의 내용에서 시간 복잡도는 `Average 시간 복잡도`를 말합니다.

![img](https://blog.kakaocdn.net/dn/lCZGB/btqzkrqP9nj/GZnkjF6A5QrkDeQPksw7u1/img.png)



## List Interface

- 순서가 있는 데이터의 집합
- **색인을 사용하여 특정 위치에 요소를 삽입하거나 접근할 수 있으며 데이터의 중복, Null값을 허용**합니다.



### List Interface의 메소드

<img src="https://blog.kakaocdn.net/dn/efYO5c/btqI07cgkG0/9kd7yxy8aMkk2c40FWbPZ1/img.png" alt="img" style="zoom: 33%;" />



---

<img src="https://blog.kakaocdn.net/dn/b6wnl3/btqzk67n3Du/vcSXAqP8ReG7kQewouloF1/img.jpg" alt="img" style="zoom: 80%;" />

### ArrayList

- `ArrayList`는 **RandomAccess 마커 Interface를 상속하여 구현**됩니다.

- 상당히 빠르고 크기를 마음대로 조절할 수 있는 배열

  - 객체들이 추가되어 **`저장 용량(capacity)`을 초과한다면 자동으로 `저장 용량(capacity)`이 늘어난다는 특징**을 가지고 있습니다.

- 초기에 생성하면 10개의 객체를 저장할 수 있는 `List`가 선언됩니다.

- **단방향 포인터 구조**로 각 **데이터에 대한 인덱스를 가지고 있어 조회 기능에 성능이 뛰어납니다.**

  - `Access`의 시간 복잡도가 `O(1)`

- 인덱스가 순차적으로 존재해야하는 구조 특성상, **한 원소를 삽입/삭제 시 배열의 나머지 원소들의 인덱스 참조값을 수정**해야 합니다.

    - 그림에서 처럼  6개의 인덱스를 가지던 배열에서 3번 원소를 삭제하면, 4, 5, 6번째 원소는 각각 3, 4, 5번으로 수정되어야 하므로 한 칸씩 당겨집니다.
    - `Insertion, Deletion`의 시간 복잡도가 `O(n)`

  - 특정한 값을 찾기 위해서는 처음 부터 값을 순서대로 찾아야 합니다.

      - `Search`의 시간 복잡도가 `O(n)`

  ```java
List<E> list = new ArrayList<E>(); // E에는 어떤 객체 타입이던 올 수 있다.
  ```



### Vector

  - 과거에 대용량 처리를 위해 사용했습니다.
  - **내부에서 자동으로 동기화처리**가 일어나 **비교적 성능이 좋지 않고 무거워** 잘 쓰이지 않습니다.
  - `ArrayList`의 시간복잡도와 같습니다.
      - `Access`의 시간 복잡도 `O(1)`, `Insertion, Deletion, Search`의 시간 복잡도가 `O(n)`

  ```java
List<E> list = new Vector<E>();
  ```



### Stack

- `후입선출 (LIFO : Last In First Out)`으로 이루어진 자료구조 `Interface`입니다.
- `Java`의 `Stack`은 `Vector`를 상속받고 있습니다.
  - `Vector`의 뒤에서만 데이터가 오고 가기 때문에  `Insertion, Deletion`의 시간 복잡도가 `O(1)`
  - `Vector`를 사용하기 때문에 `Search`의 시간 복잡도가 `O(n)`, `Access`의 시간 복잡도가 `O(1)`
    - `Access`의 경우에는 어떤 자료구조로 `Stack`을 구현하였는지에 따라 달라지게 되는데, **Java의 경우 Vector를 이용하기 때문에 O(1)**이 될 수 있습니다. (`LinkedList`로 구현할 경우 `O(n)`)

```java
Stack<E> stack = new Stack<E>();
```



### LinkedList

- **LinkedList는 List와 Deque를 구현한 class**입니다.

- **양방향 포인터 구조**로 **인접 참조를 링크해서 체인처럼 관리**합니다.
- **데이터의 삽입, 삭제가 빈번할 경우 데이터의 위치정보만 수정하면 되기에 유용**합니다. (빠른 성능을 보장)
- **스택, 큐, 양방향 큐 등을 만들기 위한 용도**로 쓰입니다.
- 초기에 생성하면 어떠한 링크도 만들어지지 않기 때문에 내부적으로 비어있습니다.
- `랜덤 접근(Random Access)`이 아닌 `순차 접근(Sequential Access)` 방식을 따르므로 `ArrayList`처럼 바로 해당 데이터로 접근할 수가 없습니다.
    - `Access`의 시간 복잡도가 `O(n)`
- 노드값이외에 앞 뒤 포인터 값을 저장하고 있는 자료구조이므로, **삭제와 삽입시 해당 노드 앞 뒤의 포인터 값만 수정해주면 추가/삭제가 완료**됩니다. 
    - 삭제, 추가할 위치를 알고 있다는 가정하의 효율값이고 모른다면 검색이 필요하므로 시간 복잡도는 `O(n)`이 됩니다.
    - 하지만 보통 알고 있다고 가정하기 때문에 `Insertion, Deletion`의 시간 복잡도가 `O(1)`
- `ArrayList`와 마찬가지로 특정한 값을 찾기 위해서는 처음 부터 값을 순서대로 찾아야 합니다.
    - `Search`의 시간 복잡도가 `O(n)`

```java
List<E> list = new LinkedList<E>();
```



#### LinkedList의 종류

`Java`의 `LinkedList`를 말하는 것이 아니고, 자료구조 상의 `LinkedList`를 말합니다. (자바의 `LinkedList`는 `이중 연결 리스트(Doubly Linked List)` 입니다.)

##### 단일 연결 리스트(Singly Linked List)

- **각 노드가 다음 노드에 대해서만 참조하는 가장 단순한 형태**의 `LinkedList`입니다.
- **일반적으로 큐를 구현할 때 사용**됩니다.
- Head 노드를 잃어버려 참조하지 못하는 경우 데이터 전체를 사용 못하게 되는 단점이 있습니다.
- FAT 파일 시스템이 단일 연결 리스트로 파일 청크(동적 메모리로 할당되는 영역)를 연결합니다.



##### 이중 연결 리스트(Doubly Linked List)

- **각 노드가 이전 노드, 다음 노드에 대해서 참조하는 형태**의`LinkedList`입니다.
- 삭제가 간편하며 단일 연결 리스트에 비해 데이터 손상에 강하지만, 관리할 참조가 2개이기 때문에 삽입이나 정렬의 경우 작업량이 더 많아집니다.



##### 원형 연결 리스트(Circular Linked List)

- **연결 리스트에서 마지막 요소가 첫번째 요소를 참조**합니다.
- 스트림 버퍼의 구현에 많이 사용됩니다.



### 성능 비교

|                | Access | Search | Insertion | Deletion |
| -------------- | ------ | ------ | --------- | -------- |
| **ArrayList**  | O(1)   | O(n)   | O(n)      | O(n)     |
| **Vector**     | O(1)   | O(n)   | O(n)      | O(n)     |
| **Stack**      | O(1)   | O(n)   | O(1)      | O(1)     |
| **LinkedList** | O(n)   | O(n)   | O(1)      | O(1)     |

표를 보면, **데이터 조회가 자주 이루어질 때 `ArrayList`를 사용하는 것이 유리**하고, **데이터의 삽입, 삭제가 빈번히 일어날 때 `LinkedList`를 사용하는 것이 유리**한 것을 알 수 있습니다.

**메모리 점유율 면에서는 `ArrayList`가 `LinkedList`보다 더 효율적**입니다. `ArrayList`는 실제 데이터와 인덱스만 저장하는 반면, `LinkedList`의 각각의 노드는 데이터와 앞 뒤의 원소의 포인터값까지 저장함으로써 더 많은 메모리를 점유하기 때문입니다.



## Queue 와 Deque Interface

`Java`에서는 `Deque`는 `Queue`를 상속받는 `sub interface`이지만 자료구조에서  `Queue`와 `Deque`을 구분하기 때문에 나누어 설명하겠습니다.



### Queue Interface

- `선입선출 (FIFO : First In First Out)`으로 이루어진 자료구조 `Interface`입니다.

- 가장 앞쪽에 있는 위치를 `head(헤드)`라고 부르고, 가장 후위(뒤)에 있는 위치를 `tail(꼬리)`라고 부릅니다.

  


### Deque (Double ended Queue) Interface

- `Queue`를 상속받고 있는 `Interface`입니다.

- `Queue`는 한쪽 방향으로만(단방향) 삽입 삭제가 가능하지만, `Deque(Double ended Queue)`는 **양쪽에서 삽입삭제가 가능한 자료구조** 입니다.
- **head에서도 접근 가능**하며, **tail에서도 접근 가능한 양방향 큐**
- 양방향에서 접근할 수 있기 때문에, `Stack`처럼 사용할 수도 있고, `Queue`처럼 사용할 수도 있습니다.
- `Deque`의 주의할 점은 **List처럼 index 관련 기능을 수행하지 않기때문에 index로 요소에 접근이 불가능**하다는 것입니다.

<img src="https://user-images.githubusercontent.com/79291114/115980806-8186bd00-a5ca-11eb-99a5-b9fae3e76831.png" alt="queue,deque" style="zoom: 50%;" />

### Queue와 Deque Interface의 메소드

<img src="https://blog.kakaocdn.net/dn/cfpfoQ/btqI66JL8WF/YgwfZ2O1HRhm67NK3CovEk/img.png" alt="img" style="zoom: 33%;" />



#### LinkedList

- **LinkedList는 List와 Deque를 구현한 interface** 입니다.
- **Deque를 구현하고 있기 때문에 Deque로도 선언**할 수 있고, **Deque는 Queue를 상속받기 때문에 Queue로도 선언**할 수 있습니다.

- 일반적으로 말하는 `Queue`를 `Java`에서는 `LinkedList`로 생성하면 됩니다.
  - `LinkedList`를 사용하기 때문에 `Search`의 시간 복잡도가 `O(n)`, `Insertion, Deletion`의 시간 복잡도가 `O(1)`.
  - `Access`의 시간 복잡도가 `O(n)` -> 원하는 위치에 `Access`하기 위해 `Queue`에서 하나씩 빼야하기 때문

```java
Queue<E> queue = new LinkedList<E>();
Deque<E> deque = new LinkedList<E>();
```



#### PriorityQueue

- `PriorityQueue`는 **Queue에서 데이터 우선순위에 기반하여 우선순위가 높은 데이터가 먼저 나오는 원리**입니다.
  - 따로 정렬방식을 지정하지 않는다면 낮은 숫자가 높은 우선순위를 갖습니다. (오름차순)
- `PriorityQueue`는 `Heap`을 이용하여 구현하는 것이 일반적입니다.
- **삽입과 삭제시, 자동으로 정렬(기본 오름차순)**하기 때문에 저장시간이 다소 오래 걸립니다.
  - 삽입, 삭제 시, `Heap`구조를 이용해 정렬하기 때문에 `Insertion, Deletion`의 시간 복잡도가 `O(log(n))`
  - `PriorityQueue` 자체가 높은 우선순위를 찾는 것이 목적이기 때문에 `Search`의 시간 복잡도는 `O(1)`, `Access`의 시간 복잡도는 `O(n)` (몇 번째로 높은지)
- 주의할 점은 처음에 위치한 데이터가 아닌, 그 밖의 데이터들이 완벽하게 정렬된 것이 아니라는 것입니다. (`약한 힙(weak heap)`)
  - 삽입, 삭제 시에 다시 정렬함으로써, **데이터들 중 우선순위가 가장 높은 데이터를 제일 처음에 위치**하게 만드는 것에만 초점을 둡니다. 
  - 즉, 모든 데이터들이 완벽하게 정렬되지 않을 수 있습니다.
- **사용자가 정의한 객체를 타입으로 쓸 경우 반드시 Comparator 또는 Comparable을 통해 정렬 방식을 구현**해주어야 합니다.

```java
PriorityQueue<E> priorityQueue = new PriorityQueue<E>();
```

[Heap에 대한 자세한 설명](https://dev-splin.github.io/cs(computer%20science)/datastructure/DataStructure-DataStructure/#binary-heap)



##### Comparable 과 Comparator

- `PriorityQueue`의 정렬방식을 지정할 때나, 밑에서 나올 내용인  `TreeSet`의 객체, `TreeMap`의 키의 크기를 비교해 트리구조를 구성해야 할 때, `java.lang.Comparable Interface`를 `Implements`하여 구현해야 합니다.



###### Comparable

- **기본적인 Wrraper Class들은 Comparable Interface가 구현**되어 있어 `PriorityQueue`, `TreeSet`, `TreeMap`을 사용할 수 있습니다.
- **ArrayList 등의 자료구조의 Collection.sort()함수를 사용할때도 Comparable을 이용해 compareTo()메소드만 재정의하게되면 쉽게 Collection 프레임워크의 자료구조들을 정렬하여 사용**할 수 있습니다.

| 리턴 타입 |       메소드        | 설명                                                         |
| :-------: | :-----------------: | :----------------------------------------------------------- |
|    int    | compareTo(Object o) | 주어진 객체와 같으면 0을 리턴 <br>주어진 객체보다 작으면 음수를 리턴 <br>주어진 객체보다 크면 양수를 리턴 |



###### Comparator

- **PriorityQueue, TreeSet, TreeMap이 Comparable을 구현하고 있지 않을 경우에는 저장하는 순간 ClassCastException이 발생**합니다.
- **Comparable 구현체를 구현하는 방법 외에 Comparator Interface를 이용해 정렬**할 수 있습니다.

```java
// DescendingComparator, AscendingComparator는 Comparator Interface를 구현한 객체입니다.
TreeSet<E> treeSet = new TreeSet<E>(new AscendingComparator());
TreeMap<K, V> treeMap = new TreeMap<K, V>(new DescendingComparator());
PriorityQueue<E> priorityQueue = new PriorityQueue<E>(new DescendingComparator());
```



- 정렬자는 `Comparator Interface`를 구현한 객체를 말하는데, `Comparator Interface`는 다음과 같은 메소드가 정의되어 있습니다.

| 리턴 타입 |             메소드             | 설명                                                         |
| :-------: | :----------------------------: | :----------------------------------------------------------- |
|    int    | compareTo(Obect o1, Object o2) | o1과 o2가 동등하다면 0을 리턴 <br>o1이 o2보다 앞에 오게하면 음수를 리턴 <br>o1이 o2보다 뒤에 오게하면 양수를 리턴 |

[Comparable / Comparator에 대한 더 자세한 설명](https://dev-splin.github.io/java/Java-Comparable,Comparator/#comparable--comparator)



#### ArrayDeque

- **ArrayDeque는 배열(Array)과 두개의 포인터(head와 tail을 가리키는 포인터)를 사용해 Deque를 구현한 interface**입니다.
- `Null`은 저장되지 않습니다.
- `ArrayDeque`로 `Stack`과 `Queue`를 사용할 시, **기존의 Stack과 LinkedList보다 빠릅니다.**
- 앞, 뒤에서의 `Insertion, Deletion`의 시간 복잡도가 `O(1)`,  `Search`는 하나씩 빼면서 찾아야 하기 때문에 `O(n)`
- `Java`의 `ArrayDeque`는 `임의 접근(Access)`을 지원하지 않기 때문에 하나씩 빼가면서 접근(`O(n)`)해야 하지만, 직접 구현할 시 `배열(Array)`을 사용하기 때문에 `Access`의 시간 복잡도가 `O(1)`이 될 수 있습니다.



#### 성능 비교

|                   | Access       | Search | Insertion | Deletion  |
| ----------------- | ------------ | ------ | --------- | --------- |
| **LinkedList**    | O(n)         | O(n)   | O(1)      | O(1)      |
| **PriorityQueue** | O(n)         | O(1)   | O(log(n)) | O(log(n)) |
| **ArrayDeque**    | O(n) or O(1) | O(n)   | O(1)      | O(1)      |

<p>

|                | LinkedList                           | PriorityQueue                                               | ArrayDeque                                                   |
| -------------- | :----------------------------------- | :---------------------------------------------------------- | :----------------------------------------------------------- |
| **implements** | List, Deque                          | Queue                                                       | Deque                                                        |
| **내부로직**   | LinkedList                           | Heap                                                        | Array                                                        |
| **순서**       | 선입선출 (FIFO : First In First Out) | `Comporator`가 정의되지 않을 경우 오름차순에 의해 자동 정렬 | `Stack(LIFO)`으로도 사용할 수 있고 `Queue(FIFO)`로도 사용할 수 있음 |
| **성능**       | ArrayDeque보다 느립니다.             | **삽입과 삭제 후 재정렬**해야하므로 가장 느립니다.          | **배열(Array)과 두개의 포인터를 사용**하기 때문에 가장 빠릅니다. |
| **Null Value** | Null 허용                            | 허용하지 않음                                               | 허용하지 않음                                                |
| **사용 용도**  | 일반적으로 사용하는 Queue            | 우선순위를 이용해야 하는 경우                               | Queue나 Stack을 더 빠르게 사용하고 싶을 때                   |



## Set Interface

- **Collection Interface를 상속**합니다.
- **순서를 유지하지 않는 데이터의 집합**
  - 인덱스로 객체를 검색해서 가져오는 메소드가 없습니다.
  - 대신 전체 객체를 대상으로 한번씩 반복해서 가져오는 반복자(`Iterator`)를 제공합니다.
- **데이터의 중복을 허용하지 않습니다.**



### Set Interface의 메소드

<img src="https://blog.kakaocdn.net/dn/lnanb/btqI4UqkeIl/LNvdh82l4b94aDdTkVBB0K/img.png" alt="img" style="zoom: 33%;" />




### HashSet

  - 순서를 예측할 수 없습니다.
  - **인덱스를 사용하지 않고 해쉬함수(`hashCode()`)를 이용해 해쉬코드를 만들어 객체를 판별**합니다.
      - 인덱스를 사용하지 않기 때문에 `Acces`는 불가능합니다.(`N/A`)
  - 대부분의 객체들이 고유한 해쉬코드(중복될 수도 있습니다.)를 가지게 되기 때문에 해쉬코드를 이용해 배열의 인덱스처럼 바로 접근할 수 있습니다.
      -  `Search, Insertion, Deletion`의 시간 복잡도가 `O(1)`
  - `HashSet`이 판단하는 동일한 객체란 꼭 같은 인스턴스를 뜻하지 않습니다.
    - `hashCode()`와 `equals()`를 재정의해서 동일한 리턴 값을 가지면 동등한 객체로 간주합니다.

![HashSet](https://user-images.githubusercontent.com/58713853/73819147-49af2080-4832-11ea-9175-536cef802708.PNG)

```java
Set<E> set = new HashSet<E>();
```



#### 해쉬 함수(Hash Function)

해쉬 함수란, **어떤 가변 값의 key를 input으로 해쉬함수에 넣으면 output으로 고정된 hash값이 나오게 하는 것**입니다. 예를 들어, `내부 로직이 f(x)= x % 10` 인 해쉬함수가 있다고 가정했을 때 84, 123, 27의 해쉬값은 각각 4, 3, 7이 되고, 이 해쉬값에 따른 위치에 저장합니다. 즉, 123은 3에, 84는 4에, 27은 7에 저장됩니다. 



![img](https://blog.kakaocdn.net/dn/olSrY/btqzklR2far/fDLyNQXhw5kqlIUlolwjaK/img.png)



그런데, **input은 거의 무한대의 가변값인데 반해 hash는 고정값을 가지므로, 중복 hash값이 발생할 수 있습니다**. 가령 위의 해쉬함수에 223이라는 값을 input으로 넣어도 123을 넣었을 때와 같은 hash값(3)을 얻게 됩니다. 이러한 현상을 `해쉬 충돌(hash collision)`이라고 하는데, **이 때에는 Java에서는 동일한 해쉬값에 대해 LinkedList로 저장**됩니다. 해쉬 함수를 잘못 설계하게 되면 하나의 해쉬코드에 대량의 객체가 들어가 있을 수 있기 때문에 Worst 시간 복잡도가 `O(n)`인 것입니다.

![img](https://blog.kakaocdn.net/dn/nQESZ/btqzkQX7Gmi/Sdrgofkb5PLBsgbPQwZm7k/img.png)

[Hash에 대한 더 자세한 설명](https://dev-splin.github.io/cs(computer%20science)/datastructure/DataStructure-DataStructure/#hash-table)



### LinkedHashSet
  - **추가된 순서 또는 가장 최근에 접근한 순서대로 접근 가능한 HashSet** 입니다.
      - **HashSet과 시간복잡도는 동일**합니다.
      - `Search, Insertion, Deletion`의 시간 복잡도가 `O(1)`,  `Acces`는 (`N/A`).
  - **데이터에 앞 뒤 포인터 정보를 포함하여 저장하는 선형구조** 입니다.
  - 저장되는 순서를 보장하지만, 점유하는 메모리가 더 많습니다.



### TreeSet

- **이진 트리 구조에 자료를 저장**합니다.
  - 트리 구조를 이용하기 때문에  `Search, Insertion, Deletion`의 시간 복잡도가 `O(log(n))`,  `Acces`는 (`N/A`).
- **하나의 노드는 노드 값인 value와 왼쪽과 오른쪽 자식노드를 참조하기 위한 두 개의 변수로 구성**됩니다.
- 객체를 저장하면 자동으로 정렬됩니다. (기본 오름차순)
- **정렬된 순서대로 보관하며 Comparable나 Comparator를 이용해 정렬방법을 지정**할 수 있습니다.

```java
TreeSet<E> treeSet = new TreeSet<E>();
```

![TreeSet](https://user-images.githubusercontent.com/58713853/73826479-30fa3700-4841-11ea-8c4d-300f5db522db.PNG)



#### 이진 트리 구조

- **이진 트리는 여러개의 노드가 트리형태로 이루어져 연결된 구조**입니다.
- **검색 기능을 강화시킨 구조**입니다.
- 루트 노드라고 불리는 하나의 노드에서부터 시작해, 각 노드에 최대 2개의 노드를 연결할 수 있는 구조를 가집니다.

<img src="https://user-images.githubusercontent.com/58713853/73824258-2fc70b00-483d-11ea-833f-4ae7311c603d.PNG" alt="이진트리구조" style="zoom:80%;" />

- 부모 노드의 값보다 작은 노드는 왼쪽에 위치 시키고 큰 노드는 오른쪽에 위치시킵니다.
- **숫자가 아닌 문자가 저장할경우에는 문자의 유니코드 값으로 비교**합니다.
- 예를 들어 6,3,9,2,5의 순서로 값을 저장하면 다음과 같은 순서로 진행됩니다.

<img src="https://user-images.githubusercontent.com/58713853/73824347-56854180-483d-11ea-9ba0-a770b9376b58.PNG" alt="이진트리진행구조" style="zoom:80%;" />

- 아래와 같이 값들이 정렬되어 있어 그룹핑이 쉽기 때문에 범위 검색을 쉽게할 수 있습니다.

<img src="https://user-images.githubusercontent.com/58713853/73826092-871aaa80-4840-11ea-9545-27c4679f8267.PNG" alt="이진트리범위검색" style="zoom:80%;" />

[Tree에 대한 더 자세한 설명](https://dev-splin.github.io/cs(computer%20science)/datastructure/DataStructure-DataStructure/#tree)



### 성능 비교

|                   | Access | Search    | Insertion | Deletion  |
| ----------------- | ------ | --------- | --------- | --------- |
| **HashSet**       | N/A    | O(1)      | O(1)      | O(1)      |
| **LinkedHashSet** | N/A    | O(1)      | O(1)      | O(1)      |
| **TreeSet**       | N/A    | O(log(n)) | O(log(n)) | O(log(n)) |

**해시셋과 링크드 해시셋은 해시값으로 호출/삭제/삽입을 하므로 모든 연산에 있어 O(1)의 시간 효율성**을 가집니다.<br>**트리셋은 이진 탐색 트리를 사용하므로, 검색/삽입/삭제의 시간 효율성은 log(n)**을 가지게 됩니다.



|                | HashSet                                                | LinkedHashSet                                                | TreeSet                                                      |
| -------------- | :----------------------------------------------------- | :----------------------------------------------------------- | :----------------------------------------------------------- |
| **내부로직**   | Hashing                                                | Hashing                                                      | 이진 탐색 트리(BST : Binary Search Tree)                     |
| **순서**       | 유지되지 않음                                          | 삽입순서 유지                                                | `Comporator`가 정의되지 않을 경우 오름차순에 의해 자동 정렬  |
| **성능**       | 가장 좋음                                              | 삽입순서를 유지하기 위하여 포인터값을 저장하므로 HashSet 보다 약간 느립니다. | **삽입과 삭제 후 재정렬**해야하므로 셋 중에서 가장 느립니다. |
| **Null Value** | 최대 한 개의 Null 허용                                 | 최대 한 개의 Null 허용                                       | 허용하지 않음                                                |
| **사용 용도**  | 순서를 유지할 필요가 없다면 일반적으로 **권장**됩니다. | 삽입순서를 유지하고 싶다면 선택                              | `Comparator`에 의해 원소들을 정렬하고 싶다면 선택            |



## Map Interface

- `키(Key), 값(Value)`의 쌍으로 이루어진 데이터의 집합
- 순서는 유지되지 않으며 **키(Key)의 중복을 허용하지 않으나 값(Value)의 중복은 허용**합니다.
- 맵 안에 데이터를 하나하나 얻어 내는 방법
- `Map`의 `(Key, Value)`로 이루어진 구조가 다른 자료구조들과는 다르기 때문에  `Collection Interface`를 상속받고 있진 않지만 `Collection`으로 분류됩니다.



### HashMap

  - `Map Interface`를 구현하기 위해 `해쉬코드`를 사용한 class
        - `Search, Insertion, Deletion`의 시간 복잡도가 `O(1)`,  `Acces`는 (`N/A`).
  - **Key의 중복과 순서가 허용되지 않습니다.**
  - **키와 값으로 nll값이 올 수 있습니다.**
  - **HashSet과 마찬가지로 hashCode와 equals를 재정의해서 동일한 리턴 값을 가지면 동등한 객체로 간주**합니다.
```java
Map<K, V> map = new HashMap<K, V>();
```



### Hashtable

- **HashMap과 동일한 내부구조**를 가집니다.
  - `Search, Insertion, Deletion`의 시간 복잡도가 `O(1)`,  `Acces`는 (`N/A`).
- **HashMap 보다는 느리지만 동기화 지원**
  - 동기화된 메소드로 구성되어 있기때문에 멀티스레드가 동시에 이 메소드들을 실행할 수 없습니다.
  - 하나의 스레드가 실행을 완료해야 다른 스레드가 실행할 수 있습니다.
- **키와 값으로 null값이 허용되지 않습니다.**
```java
Map<K, V> map = new Hashtable<K, V>();
```



### LinkedHashMap

- 기본적으로 **HashMap을 상속받아 HashMap과 매우 흡사**합니다.
  - `Search, Insertion, Deletion`의 시간 복잡도가 `O(1)`,  `Acces`는 (`N/A`).
- **삽입순서를 유지하기 위하여 포인터값을 저장**하므로 HashMap 보다 약간 느립니다.
- **Map에 있는 엔트리들의 연결 리스트가 유지되므로 입력한 순서대로 반복 가능**합니다.
```java
Map<K, V> map = new LinkedHashMap<K, V>();
```



### TreeMap

- 키와 값의 쌍으로 이루어진 데이터 `MapEntry`를 저장
- **객체를 저장하면 기본적으로 부모 키값과 비교해서 키값이 낮은 것은 왼쪽, 높은 것은 오른쪽에 자동으로 정렬**됩니다.
  - 트리 구조를 이용하기 때문에  `Search, Insertion, Deletion`의 시간 복잡도가 `O(log(n))`,  `Acces`는 (`N/A`).
- **정렬된 순서대로 키(Key)와 값(Value)을 저장**하여 검색이 빠릅니다.
- **삽입과 삭제 시, 정렬**을 하기 때문에 저장시간이 다소 오래 걸립니다.
- **정렬된 순서(기본 오름차순)대로 보관하며 Comparable나 Comparator를 이용해 정렬방법을 지정**할 수 있습니다.
```java
TreeMap<K, V> treeMap = new TreeMap<K, V>();
```


![TreeMap](https://user-images.githubusercontent.com/58713853/73826910-f3e27480-4841-11ea-8d11-2f6d735f54d9.PNG)





### 성능 비교

|                   | Access | Search    | Insertion | Deletion  |
| ----------------- | ------ | --------- | --------- | --------- |
| **HashMap**       | N/A    | O(1)      | O(1)      | O(1)      |
| **Hashtable**     | N/A    | O(1)      | O(1)      | O(1)      |
| **LinkedHashMap** | N/A    | O(1)      | O(1)      | O(1)      |
| **TreeMap**       | N/A    | O(log(n)) | O(log(n)) | O(log(n)) |

**HashMap과 LinkedHashMap, Hashtable은 해시값으로 호출/삭제/삽입을 하므로 모든 연산에 있어 O(1)의 시간 효율성**을 가집니다.<br>**TreeMap은 이진 탐색 트리를 사용하므로, 검색/삽입/삭제의 시간 효율성은 log(n)**을 가지게 됩니다.



|                | HashTable                          | HashMap                                                    | LinkedHashMap                                                | TreeMap                                                     |
| -------------- | ---------------------------------- | ---------------------------------------------------------- | ------------------------------------------------------------ | ----------------------------------------------------------- |
| **내부로직**   | Hashing                            | Hashing                                                    | Hashing                                                      | 이진 탐색 트리(BST : Binary Search Tree)                    |
| **동기화**     | Thread safe                        | Not Thread safe                                            | Not Thread safe                                              | Not Thread safe                                             |
| **정렬 순서**  | 유지되지 않음                      | 유지되지 않음                                              | 삽입순서 유지                                                | `Comporator`가 정의되지 않을 경우 오름차순에 의해 자동 정렬 |
| **성능**       | 동기화되므로 HashMap보다 약간 느림 | 가장 좋음                                                  | **삽입순서를 유지하기 위하여 포인터값을 저장**하므로 HashMap 보다 약간 느림 | **삽입과 삭제 후 재정렬**해야하므로 가장 느림               |
| **Null Value** | 허용하지 않음                      | Key값으로 최대 한 개의 Null 허용, V값으로 복수의 Null 허용 | Key값으로 최대 한 개의 Null 허용, V값으로 복수의 Null 허용   | 허용하지 않음                                               |
| **사용 용도**  | 쓰레드 세이프티가 요구될 때 사용   | 가장 좋은 퍼포먼스를 보이므로, 일반적으로 **권장**         | 삽입순서를 유지하고 싶다면 선택                              | `Comparator`에 의해 원소들을 정렬하고 싶다면 선택           |



### Properties

- **Hashtable의 하위 클래스이기 때문에 Hashtable의 모든 특징**을 그대로 가지고 있습니다.
- Hashtable은 키와 값을 다양한 타입으로 지정이 가능하지만 **Properties는 키와 값을 String 타입으로 제한한 컬렉션**입니다.
- `.properties`라는 확장자를 가진 파일을 만들어  **DB의 연결정보 등을 저장해두는 용도**로 많이 쓰입니다.

```java
Properties properties = new Properties();
```



####   간단한 사용법

- 파일을 직접 여는 클래스가 아니므로 **FileReader 또는 FileInputStream 객체를 매개변수**로 받습니다.
- 외부 리소스와 직접 연결되지 않으므로 확인된 **예외가 throw되어 있지 않습니다. (예외 처리 필요)**
- `load()` 메소드를 통해 파일 정보를 넣어 줍니다.



##### db.properties

자바 프로젝트의 `config` 라는 패키지에 `db.properties` 라는 파일이 있다고 가정하겠습니다.

```properties
driver=org.postgresql.Driver
url=jdbc:postgresql://localhost:5432/postgres
username=postgres
password=#goodday#
```



##### properties파일 가져오기

```java
import java.io.IOException;
import java.io.Reader;
import java.util.Properties;

public class EntryMain {
    public static void main(String[] args) {
        String resource = "config/db.properties";
        Properties properties = new Properties();
        
        try {
            Reader reader = Resources.getResourceAsReader(resource);
            properties.load(reader);
            
            System.out.println(properties.getProperty("driver"));
            System.out.println(properties.getProperty("username"));
            System.out.println(properties.getProperty("password"));
            System.out.println(properties.getProperty("url"));
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
```

코드를 실행해 보면 **db.properties 파일을 담겨져 있는 4개의 정보가 콘솔에 표시**되는 것을 볼 수 있습니다.



---

참고 : https://gangnam-americano.tistory.com/41 

https://hackersstudy.tistory.com/26

https://minwan1.github.io/2018/07/03/2018-07-03-collection/

https://gem1n1.tistory.com/97

[https://freestrokes.tistory.com/84](https://freestrokes.tistory.com/84)

[https://codevang.tistory.com/163](https://codevang.tistory.com/163)

[http://www.gisdeveloper.co.kr/?p=5160](http://www.gisdeveloper.co.kr/?p=5160)

[https://st-lab.tistory.com/142](https://st-lab.tistory.com/142)

[https://sup2is.github.io/2019/09/23/array-deque.html](https://sup2is.github.io/2019/09/23/array-deque.html)

[https://st-lab.tistory.com/185#%ED%81%B4%EB%9E%98%EC%8A%A4](https://st-lab.tistory.com/185#%ED%81%B4%EB%9E%98%EC%8A%A4)

[https://www.baeldung.com/java-array-deque](https://www.baeldung.com/java-array-deque)
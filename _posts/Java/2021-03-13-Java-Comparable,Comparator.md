---
title: "Java : Comparable / Comparator"
excerpt_separator: <!--more-->
categories:
  - Java
tags:
  - Java 
  - "Java : Comparable / Comparator"
toc: true
toc_sticky: true
toc_label: 목차
---

# Comparable / Comparator

자바와 같이 객체 지향 언어를 사용하여 프로그래밍을 하다보면 객체들을 정렬해야하는 경우가 생깁니다. 예를 들면, 온라인 게임 서비스에서 게이머들을 높은 점수 순으로 보여주는 게이머 랭킹 페이지를 생각해볼 수 있겠습니다.



## 정렬 대상 클래스

먼저, 각 게이머의 정보를 표현하기 위해 다음과 같은 간단한 클래스를 생각해보겠습니다.

```java
public class Player {
    private String name;
    private int score;

    public Player(String name, int score) {
        this.name = name;
        this.score = score;
    }

    // Getters, Setters 생략
}
```

그리고 5 명의 게이머가 담고 있는 리스트를 생성하였습니다. 자, 이제 이 게이머 리스트를 점수 기준으로 어떻게 정렬할 수 있을까요?

```java
List<Player> players = new ArrayList<>();
players.add(new Player("Alice", 899));
players.add(new Player("Bob", 982));
players.add(new Player("Chloe", 1090));
players.add(new Player("Dale", 982));
players.add(new Player("Eric", 1018));
```

`Chloe` 가 일등이기 때문에 리스트의 맨 앞으로 나와야하고, `Alice` 는 꼴등이기 때문에 리스트의 맨 뒤로 보내야합니다.



### 객체 정렬 기준의 필요성

단순한 숫자나 문자와 같은 `기본형(primitive)` 데이터는 사람들이 하는 일반적으로 받아드리는 대소 비교라는 개념이 있습니다. `(Natural Order)` 따라서 우리는 `1`보다는 `2`가 크며, `'A'` 보단 `'B'`가 크다는 것을 알기 때문에 자바는 이런 통념에 따라 정렬을 해줍니다.

예를 들어, 게이머들의 점수만으로 별도의 배열을 만들면 다음과 같이 `Arrays.sort()` 메서드를 사용하여 정렬을 할 수 있습니다.

```java
int[] scores = {899, 982, 1090, 982, 1018};
Arrays.sort(scores);
System.out.println(Arrays.toString(scores)); // [899, 982, 982, 1018, 1090]
```

하지만 **특정 타입의 객체는 기본형 데이터와 달리 정렬 기준이 없으면 정렬을 할 수가 없으며**, 따라서 **정렬 기준을 정의**하여 알려줘야 합니다. 즉, 다음과 같은 코드는 컴파일 에러를 일으킵니다.

```java
Collections.sort(players); // Compile error!
```



---

## Comparable 인터페이스

객체의 정렬 기준을 정의하는 첫번째 방법은 정렬 대상 클래스를 자바에서 기본적으로 제공하고 있는 `Comparable` 인터페이스를 구현하도록 변경하는 것입니다. 이를 적용하면 `Player` 클래스는 다음과 같이 수정됩니다.



### 사용 방법

`Comparable` 인터페이스의 **`compareTo()` 메서드를  `Override`하여 인자로 넘어온 같은 타입의 다른 객체와 대소 비교**하고 `Arrays.sort()`나 `Collections.sort()`에 넣어 사용할 수 있습니다.



#### Arrays.sort()와 Collections.sort()의 차이

1. **Arrays.sort()**
   - 배열 정렬의 경우
   - Ex) `byte[]`, `char[]`, `double[]`, `int[]`, `Object[]`, `T[]` 등 <br><u>Object Array에서는 TimSort(Merge Sort + Insertion Sort)를 사용</u>
   - Object Array: 새로 정의한 클래스에 대한 배열<br> <u>Primitive Array에서는 Dual Pivot QuickSort(Quick Sort + Insertion Sort)를 사용</u>
   - Primitive Array: 기본 자료형에 대한 배열
2. **Collections.sort()**
   - List Collection 정렬의 경우
   - Ex) ArrayList, LinkedList, Vector 등<br> <u>내부적으로 Arrays.sort()를 사용</u>



#### compareTo() 메서드 작성법

- 현재 객체 < 파라미터로 넘어온 객체: 음수 리턴
- 현재 객체 == 파라미터로 넘어온 객체: 0 리턴
- 현재 객체 > 파라미터로 넘어온 객체: 양수 리턴
- 음수 또는 0이면 객체의 자리가 그대로 유지되며, **양수인 경우에는 두 객체의 자리가 바뀝니다.**



```java
public class Player implements Comparable<Player> {
    // Fields, Getters, Setters 생략

    @Override
    public int compareTo(Player o) {
        return o.getScore() - this.getScore();
    }
}
```

게이머 랭킹 페이지의 경우 **높은 점수 순으로 내림 차순 정렬**을 원하기 때문에, 인자로 넘어온 게이머의 점수에서 메서드를 호출하는 게이머의 점수를 빼면 됩니다. 

예를 들어 인자로 넘어온 게이머의 점수가 `982`이고 `compareTo()` 메서드를 호출하는 객체의 점수가 `1018`이라면, `compareTo()` 메서드는 음수(`982-1018`)를 리턴하게 되며, 자리가 바뀌지 않습니다.<br> 다시 말해, 메서드를 호출를 호출하는 메서드가 점수가 더 크지만, **음수를 리턴함으로써 두 객체의 자리가 바뀌지 않게하여 내림 차순으로 만들 수 있는 것**입니다.



```java
Collections.sort(players);
System.out.println(players); // [Player(name=Chloe, score=1090), Player(name=Eric, score=1018), Player(name=Bob, score=982), Player(name=Dale, score=982), Player(name=Alice, score=899)]
```

이제 `Collections.sort()` 메서드에는 `Comparable` 인터페이스를 구현한 `Comparable` 타입의 `Player` 객체의 리스트가 인자로 넘어가기 때문에 더 이상 컴파일 에러가 발생하지 않습니다. 정렬 후 게이머 리스트를 출력해보면 원하는 바와 같이 점수가 제일 높은 `Chloe` 가 리스트의 맨 앞으로 위치하고, 점수가 제일 낮은 `Alice` 가 리스트의 맨 뒤에 위치하게 됩니다.



---

## Comparator 객체 사용

만약 정렬 대상 **클래스의 코드를 직접 수정할 수 없는 경우**에는 어떻게 객체의 정렬 기준을 정의할 수 있을까요? 또는 **정렬 하고자 하는 객체에 이미 존재하고 있는 정렬 기준과 다른 정렬 기준으로 정렬을 하고 싶을 때**는 어떻게 해야할까요?

이 때 필요한 것이 바로 `Comparable` 인터페이스와 이름이 유사한 `Comparator` 인터페이스입니다. `Comparator` 인터페이스의 구현체를 `Arrays.sort()`나 `Collections.sort()`와 같은 정렬 메서드의 두 번째 인자로 넘기면 **정렬 기준을 누락된 클래스의 객체나 기존 정렬 기준을 무시하고 새로운 정렬 기준으로 객체를 정렬**할 수 있습니다. 기본적인 정렬 방법인 오름차순 정렬을 내림차순으로 정렬할 때 많이 사용합니다.



### 두 번째 인자로 Comparator interface를 받는 경우

`우선 순위 큐(PriorityQueue)` 생성자의 두 번째 인자로 `Comparator interface`를 받을 수 있습니다. `PriorityQueue(int initialCapacity, Comparator<? super E> comparator)`
지정된 `Comparator`의 정렬 방법에 따라 우선 순위를 할당합니다.



### compare() 메서드 작성법

위의 `compareTo()`의 작성법과 같습니다.

- 첫 번째 파라미터로 넘어온 객체 < 두 번째 파라미터로 넘어온 객체: 음수 리턴
- 첫 번째 파라미터로 넘어온 객체 == 두 번째 파라미터로 넘어온 객체: 0 리턴
- 첫 번째 파라미터로 넘어온 객체 > 두 번째 파라미터로 넘어온 객체: 양수 리턴
- 음수 또는 0이면 객체의 자리가 그대로 유지되며, **양수인 경우에는 두 객체의 자리가 변경됩니다.**



### 익명 클래스로 사용하기

```java
Comparator<Player> comparator = new Comparator<Player>() {
    @Override
    public int compare(Player a, Player b) {
        return b.getScore() - a.getScore();
    }
};

Collections.sort(players, comparator);
System.out.println(players); // [Player(name=Chloe, score=1090), Player(name=Eric, score=1018), Player(name=Bob, score=982), Player(name=Dale, score=982), Player(name=Alice, score=899)]
```

위 코드는 `Comparator` 객체를 `Collections.sort()` 메서드의 두번째 인자로 넘겨서 이전 섹션과 동일한 정렬 결과를 만들어 내고 있습니다. 이렇게 **`Comparator` 객체를를 인자로 넘기면, 정렬 대상 객체가 `Comparable` 인터페이스를 구현 여부와 상관없이, 넘어온 `Comparator` 구현체의 `compare()` 메서드 기준으로 정렬을 수행**합니다. `compare()` 메서드는 비교 대상 2 개의 객체를 인자를 차례로 인자로 받습니다. 첫번째 인자가 두번째 인자보다 작다면 음수, 같다면 0, 크다면 양수를 리턴하면 됩니다.



### 람다 함수로 사용하기

`Comparator` 객체는 메서드가 하나 뿐인 함수형 인터페이스를 구현하기 때문에 `람다 함수`로 대체가 가능합니다.

```java
Collections.sort(players, (a, b) -> b.getScore() - a.getScore());
System.out.println(players); // [Player(name=Chloe, score=1090), Player(name=Eric, score=1018), Player(name=Bob, score=982), Player(name=Dale, score=982), Player(name=Alice, score=899)]
```

람다 함수를 사용하면 코드가 간결해짐을 알 수 있습니다.

[함수형 인터페이스, 람다식이란?](https://dev-splin.github.io/java/Java-%ED%95%A8%EC%88%98%ED%98%95%ED%94%84%EB%A1%9C%EA%B7%B8%EB%9E%98%EB%B0%8D(%EB%9E%8C%EB%8B%A4%EC%8B%9D)/)



### Stream 으로 정렬

`Stream` 클래스의 `sorted()` 메서드도 `Comparator` 객체를 인자로 받아 정렬을 해줍니다. 스트림을 사용하면 위에서 살펴본 배열과 리스트의 정렬과 달리 **기존 객체의 순서를 변경하지 않고, 새롭게 정렬된 객체를 생성하고자 할 때 사용**됩니다.

```java
List<Player> sortedPlayers = players.stream()
        .sorted((a, b) -> b.getScore() - a.getScore())
        .collect(Collectors.toList());
System.out.println(sortedPlayers); // [Player(name=Chloe, score=1090), Player(name=Eric, score=1018), Player(name=Bob, score=982), Player(name=Dale, score=982), Player(name=Alice, score=899)]
```

[스트림,스트림 파이프라인 이란?](https://dev-splin.github.io/java/Java-%EC%8A%A4%ED%8A%B8%EB%A6%BC,%EC%8A%A4%ED%8A%B8%EB%A6%BC%ED%8C%8C%EC%9D%B4%ED%94%84%EB%9D%BC%EC%9D%B8/)



---

참고 : https://www.daleseo.com/java-comparable-comparator/

https://gmlwjd9405.github.io/2018/09/06/java-comparable-and-comparator.html
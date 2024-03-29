---
title: "Clean Code : 부록 A(동시성 ll)"
excerpt_separator: <!--more-->
categories:
  - Clean Code
tags:
  - Clean Code
  - "Clean Code : Chapter Supplement A"
toc: true
toc_sticky: true
toc_label: 목차 
---

# 부록 A. 동시성 ll

이 장은 전에 스터디하였던 [동시성](https://dev-splin.github.io/clean%20code/CleanCode-Chapter13/)을 좀 더 자세히 설명하고 보안하는 장입니다. 저자는 코드와 함께 다양한 운영체제 개념, 디자인 패턴을 말합니다. 저는 여기서 코드 보다는 운영체제 개념과 디자인 패턴 위주로 설명하려고 합니다.





## 1. 클라이언트/서버 예제

보통 클라이언트와 서버 간의 성능 테스트를 진행합니다. 이 때, 테스트가 실패한다면 폴링을 구현한다면 모를까, 단일 스레드 환경에서 속도를 끌어올릴 방법은 거의 없습니다. 때문에 어디서 속도가 느려지는지 알아야 합니다. 가능성은 아래의 두 가지 입니다.

> **폴링(polling)** : *시간을 맞추기 위하여* 작업이 진행 중인 주체를 주기적으로 검사하는 작업

1. `I/O` - 소켓 사용, 데이터 베이스 연결, 가상 메모리 스와핑 기다리기 등
2. `프로세서` - 수치 계산, 정규 표현식 처리, 가비지 컬렉션 등

`I/O` 연산에 시간을 보낸다면 스레드를 추가해 동시성 성능을 높여주어야 합니다. 하지만 프로 세서 연산에 시간을 보내는 프로그램은 스레드를 추가한다고 빨라지지 않습니다. CPU 사이클은 한계가 있기 때문입니다. 이 내용에 대해서는 아래의 링크를 참고하면 좋을 것 같습니다.



**참고 링크**

- [CPU와 I/O 연산](https://chogyujin.github.io/2019/03/19/2.2-CPU%EC%99%80-IO-%EC%97%B0%EC%82%B0/)





## 2. 가능한 실행 경로

이전에 만약 아래의 코드를 두 개의 스레드가 실행하게 된다면, [13장 동시성](https://dev-splin.github.io/clean%20code/CleanCode-Chapter13/)에서도 나왔듯이 아래 처럼 다양한 결과를 얻을 수 있습니다.

1. 스레드 1이 94를 얻고, 스레드2가 95를 얻고, `lastIdUsed`가 95가 됨
2. 스레드 1이 95를 얻고, 스레드2가 94를 얻고, `lastIdUsed`가 95가 됨
3. 스레드 1이 94를 얻고, 스레드2가 94를 얻고, `lastIdUsed`가 94가 됨

```java
public class IdGenerator {
    int lastIdUsed;
    
    public int incrementValue() {
        return ++lastIdUsed;
    }
}
```



### 심층 분석

아래의 코드 중 **lastID = 0은 원자적 연산** 이고 **++lastId는 원자적 연산이 아닙니다.**

> **원자적 연산** : 중단이 불가능한 연산을 말함

**원자적 연산이 아니라면 연산간의 다른 스레드 간섭이 가능**하기 때문에 위의 3번 같은 문제가 생기는 것입니다.

즉, 여러 스레드에서 코드를 실행할 때, `원자적 연산(lastID = 0)`은 무조건 같은 결과가 나올 것이고, `원자적 연산이 아닌(++lastId)` 것은 스레드 간섭으로 인해 다른 결과가 나올 수 있습니다.





## 3. 라이브러리를 이해하라

자바의 Executor를 이용해 스레드 풀을 관리하는 방법에 대하여 나오지만, 직접 사용하지 않는 이상 와닿지 않을 거라 생각합니다. 스레드 풀 관리 중 알아야할 기본 개념을 참고 링크를 통해 아는 것이 더 좋을 것 같습니다.



**참고 링크**

- [Blocking vs Non-Blocking / Sync vs Async](https://dev-splin.github.io/cs(computer%20science)/operating%20system/OS-Blocking,Non-blocking-Sync,Async/)

- [뮤텍스(Mutex) / 세마포어(Semaphore) / 모니터(Monitor)](https://dev-splin.github.io/cs(computer%20science)/operating%20system/OS-Mutex,Semaphore,Monitor/)



이 장에서 여기서 하나 괜찮다고 생각하는 내용은 여러 스레드가 같은 값을 갱신하지 않더라도 무조건 락을 거는 방법을 아래와 같이 비교 조건을 통하여 조건부로 변경하는 것이었습니다.

```java
int variableBeingSet;

void simulateNonBlockingSet(int newValue) {
    int currentValue;
    do {
        // 현재 값에 설정 중인 값을 넣음
        currentValue = variableBeingSet
    } while(currentValue != compareAndSwap(currentValue, newValue));
}

int synchronized compareAndSwap(int currentValue, int newValue) {
    // 현재 값과 설정 중인 변수가 같다면 설정 중인 변수에 새로운 값을 넣음 -> 다음 반복문에서 현재 값을 갱신하게 됨
    if(variableBeingSet == currentValue) {
        variableBeingSet = newValue;
        return currentValue;
    }
    
    return variableBeingSet;
}
```

이렇게 비교하여 값을 갱신하는 것을 `CAS(Compare And Swap) : 비교한 후 바꿔주는 것` 이라고 합니다.





## 4. 메서드 사이에 존재하는 의존성을 조심하라

여러 개의 스레드가 코드를 실행할 때, 간혈적으로 버그가 발생한다면 해결하는 방법으로 아래의 3가지 방법이 있습니다.

1. `실패를 용인` : 클라이언트에서 예외를 받아 처리(조잡한 방법)
2. `클라이언트 - 기반 잠금` : 버그가 발생하는 클라이언트 모든 부분에 적용해야 하므로, 시스템이 커질 수록 실수할 확률이 높아짐
3. `서버-기반 잠금` : 클라이언트에서 일일이 잠금에 대한 처리를 해줄 필요가 없기 때문에 제일 바람직하며, 아래와 같은 장점이 있음
   - 클라이언트에서 잠금 코드를 추가할 필요가 없기 때문에 코드 중복이 줄어듦
   - 스레드를 변경할 시 서버만 교체하면 되기 때문에 성능이 좋아짐
   - 잠금 코드 작성을 깜빡하여 오류가 발생할 가능성을 줄여줌
   - 서버 한 곳에서 정책을 구현하기 때문에 정책이 분산되지 않음
   - 클라이언트가 공유 변수 자체를 모르거나 잠긴 방식을 모르기 때문에 공유 변수 범위가 줄어듦





## 5. 작업 처리량 높이기

이장은 **3.라이브러리를 이해하라의 Blocking vs Non-Blocking / Sync vs Async**의 맨 아래의 `동영상 스트리밍 서비스` 부분으로 설명할 수 있습니다.





## 6. 데드락

데드락은 아래의 4가지 조건을 만족해야 합니다.

1. `상호 배제(Mutual exclusion)` : 동시에 사용해도 괜찮은 자원을 사용함으로써 해결 :arrow_right: 대부분은 동시에 자원을 사용하기가 어려움
2. `잠금 대기(Lock & Wait)` : 각 자원을 점유하기 전에 확인하고 어느 하나라도 점유하지 못한다면 지금까지 점유한 자원을 반환하고 다시 시작 함으로써 해결 :arrow_right: 기아`, 라이브락`이 발생할 수 있음
3. `선점 불가(No Preemption)` : 다른 스레드로부터 자원을 뺏어오는 방법으로 해결 :arrow_right: 모든 요청을 관리하기가 힘듬
4. `순환 대기(Circular Wait)` : 자원에 접근하는 순서를 고정함으로써 해결 :arrow_right: 다양한 상황이 있기에 자원 사용 순서가 다를 가능성이 높음

위의 4가지 조건 중 하나라도 만족하지 않으면 데드락은 발생하지 않습니다.



## 7. 다중 스레드 코드 테스트

`몬테 카를로 테스트`로 아래와 같은 상황에서 돌아가는 테스트 케이스를 만들어 다중 스레드 테스트를 하더라도 간혈적으로 일어나는 버그를 발견하기는 쉽지 않습니다. 

- 시스템을 배치할 플랫폼 전부에서 반복적으로 테스트를 돌림 :arrow_right: 실패 없이 오래 돌아간다면, 아래의 두 가지 중 하나일 확률이 높음
  - 실제 코드가 올바름
  - 테스트가 부족해 문제를 드러내지 못함
- 부하가 변하는 장비에서 테스트를 돌림 :arrow_right: 실제 환경과 비슷하게 부하를 걸어 주는 게 좋음





## 8. 스레드 코드 테스트를 도와주는 도구

저자는 **IBM의 ConTest같은 스레드 코드 테스트를 도와주는 도구를 적극 활용**하는게 좋다고 합니다.





## 9. 결론

이 장에서는 동시성을 위한 다양한 개념들과 방법들을 소개했는데, 다중 스레드 시스템을 구현하려면 알아야 할 내용이 아주 많기 때문에 추가로 공부하는 것이 좋다고 합니다. :arrow_right: 동시성을 당장에 다루는 것이 아니라면, 동시성에서 사용되는 다양한 개념들을 정확히 알고 가는 것이 좋지 않을까 생각합니다.



---

참고 : Clean Code - 로버트 C. 마틴


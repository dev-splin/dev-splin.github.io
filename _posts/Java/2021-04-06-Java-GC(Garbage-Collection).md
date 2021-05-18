---
title: "Java : GC(Garbage Collection)"
excerpt_separator: <!--more-->
categories:
  - Java
tags:
  - Java
  - "Java : GC(Garbage Collection)"
toc: true
toc_sticky: true
toc_label: 목차
---

# GC(Garbage Collection)

`GC(Garbage Collection)`은 메모리 관리 기법 중 하나로 **프로그램이 동적으로 할당했던 메모리 영역 중에서 필요없게 된 영역을 해제하는 기능**입니다. 
즉, **동적 할당된 메모리 영역 가운데 어떤 변수도 가리키지 않는 메모리 영역을 탐지하여 자동으로 해제하는 기법**입니다. 

Java에서는 개발자가 프로그램 코드로 메모리를 명시적으로 해제하지 않기 때문에 `가비지 컬렉터(Garbage Collector)`가 더 이상 필요 없는 `가비지(쓰레기)` 객체를 찾아 지우는 작업을 합니다. 



## 장단점

### 장점

`GC`를 이용하게 되면 프로그래머가 동적으로 할당한 메모리 영역 전체를 완벽하게 관리하지 않아도 됩니다. 즉, `GC`를 통해 아래와 같은 버그를 줄이거나 막을 수 있습니다. 

- **유효하지 않은 포인터 접근** : 이미 동적 할당한 메모리를 해제한 영역에 접근하게 되는 버그 
- **이중 해제** : 이미 해제된 메모리를 또 다시 해제하는 오류를 줄일 수 있습니다. 대표적으로 C에서는`free()`를 통해 해제하고 또 다시 `free()`를 하면 정상 종료 되지 않습니다. 

- **메모리 누수** : 더이상 사용하지 않는 메모리 영역을 해제하지 않고 남겨진 것이 쌓이게 되면 메모리 누수가 일어납니다. 이러한 메모리 누수가 지속되면 메모리 고갈로 인해 프로그램이 중단 될 수 있습니다. 



### 단점

**어떤 메모리를 해제해야 할 지 결정하는데 사용되는 알고리즘에 의한 비용**이 듭니다. 객체가 필요없어지는 시점을 프로그래머가 알고 있는 경우에도 `GC` 알고리즘이 메모리 해제 시점을 추적해야하기에 비용이 들게됩니다. **`GC`가 행동하는 타이밍이나 `GC`의 점유 시간을 사전에 예측하기 어렵**기에 실시간 시스템에는 적합하지 않습니다. 할당된 메모리가 해제되는 시점을 알 수 없게 됩니다. 



### 가비지 컬렉션의 위험성

**실시간 시스템에서 `Garbage Collection`이 사용된다면 치명적인 오류를 발생할 수 있습니다.** 군사목적의 프로그래밍(미사일 발사 등) 혹은 비행시스템 등에서 실시간으로 목표물 지점으로 날아가고 있는 중간에 `Garbage Collection`이 발생하여 동작하게 되면 잠시 동안 앞에서 말한 알고리즘의 동작이 멈출 수 있는 가능성 때문에 **실시간 시스템에서는 `Garbage Collection`은 지양**해야 합니다.



## Minor / Major GC

![Heap](https://user-images.githubusercontent.com/79291114/113572095-8a7f1100-9652-11eb-83d4-596e9caf096b.PNG)

### Minor GC

**새롭게 생성한 대부분의 객체는 `Eden` 영역에 위치**합니다. **`Eden` 영역에서 `GC`가 한 번 발생한 후 살아남은 객체는 `Survivor` 영역 중 하나로 이동**합니다. 이렇게 계속 **`Survivor` 영역에 객체가 계속 쌓이다가 가득 차게 되면, 이 `Survivor` 영역 중 살아남은 객체를 다른 `Survivor`영역으로 이동**합니다. 그 후, **가득찬 `Survivor` 영역은 아무 데이터도 없는 상태가 됩니다.** 이 과정을 반복하다가 **계속해서 살아남아 있는 객체는 `Old` 영역으로 이동**하게 됩니다. 

<img src="https://d2.naver.com/content/images/2015/06/helloworld-1329-3.png" alt="JavaGarbage3" style="zoom: 80%;" />

### Major GC(Full GC)

**Old영역이 가득 차면 Old영역에 있는 모든 객체들을 검사하여 참조되지 않은 객체들을 한꺼번에 삭제**합니다.(Permanent Generation 영역에 GC가 발생해도 Major GC의 횟수에 포함) 시간이 오래 걸리고 실행 중 프로세스가 정지됩니다. 이것을 `stop-the-world`라고 하는데, **`Major GC`가 발생하면 GC를 실행하는 스레드를 제외한 나머지 스레드는 모두 작업을 멈춥니다.** GC작업을 완료한 이후에야 중단했던 작업을 다시 시작합니다.

GC 방식은 JDK 7을 기준으로 5가지 방식이 있습니다

- Serial GC
- Parallel GC
- Parallel Old GC(Parallel Compacting GC)
- Concurrent Mark & Sweep GC(이하 CMS)
- G1(Garbage First) GC



#### Serial GC (-XX:+UseSerialGC)

**Old 영역의 GC는 `Mark-Sweep-Compact`이라는 알고리즘을 사용**합니다.

1. **Mark**
   - 이 알고리즘의 첫 단계는 `Old 영역`에 살아 있는 객체를 식별하는 것입니다. 
2. **Sweep**
   - 그 다음에는 `힙(heap)`의 앞 부분부터 확인하여 살아 있는 것만 남깁니다.
3. **Compaction**
   - 마지막 단계에서는 각 객체들이 연속되게 쌓이도록 힙의 가장 앞 부분부터 채워서 객체가 존재하는 부분과 객체가 없는 부분으로 나눕니다.

`Serial GC`는 적은 메모리와 CPU 코어 개수가 적을 때 적합한 방식입니다.



#### Parallel GC (-XX:+UseParallelGC)

`Parallel GC`는 `Serial GC`와 기본적인 알고리즘은 같습니다. 그러나 **Serial GC는 GC를 처리하는 스레드가 하나인 것에 비해, Parallel GC는 GC를 처리하는 쓰레드가 여러 개**입니다. 그렇기 때문에 `Serial GC`보다 빠르게 객체를 처리할 수 있습니다. **Parallel GC는 메모리가 충분하고 코어의 개수가 많을 때 유리**합니다. `Parallel GC`는 `Throughput GC`라고도 부릅니다.

![JavaGarbage4](https://d2.naver.com/content/images/2015/06/helloworld-1329-4.png)



#### Parallel Old GC(-XX:+UseParallelOldGC)

`Parallel Old GC`는 JDK 5 update 6부터 제공한 GC 방식입니다. 앞서 설명한 **Parallel GC와 비교하여 Old 영역의 GC 알고리즘만 다릅니다.** **이 방식은 `Mark-Summary-Compaction` 단계를 거칩니다.** `Summary` 단계는 앞서 GC를 수행한 영역에 대해서 별도로 살아 있는 객체를 식별한다는 점에서` Mark-Sweep-Compaction` 알고리즘의 `Sweep` 단계와 다르며, 약간 더 복잡한 단계를 거칩니다.

#### CMS(Concurrent Mark-Sweep) GC (-XX:+UseConcMarkSweepGC)

앞서 살펴보았던 GC 보다 좀 더 개선된 방식입니다. 개선이 된 만큼 성능은 좋아졌지만 GC의 과정은 좀 더 복잡해졌습니다. **`CMS`는 GC 과정에서 발생하는 `stop-the-world` 시간을 최소화 하는데 초점을 맞춘 GC 방식**입니다. 다음 그림은 `Serial GC`와 `CMS GC`의 절차를 비교한 그림입니다.

![JavaGarbage5](https://d2.naver.com/content/images/2015/06/helloworld-1329-5.png)



`CMS GC`는 `Initial Mark -> Concurrent Mark -> Remark -> Concurrent Sweep` 과정을 거칩니다.

1. **Initial Mark**
   - `CMS GC`의 초기  단계에서는 클래스 로더에서 가장 가까운 객체 중 살아 있는 객체만 찾는 것으로 끝냅니다. 따라서, 멈추는 시간은 매우 짧습니다.
2. **Concurrent Mark**
   - 방금 살아있다고 확인한 객체에서 참조하고 있는 객체들을 따라가면서 확인합니다. 이 단계의 특징은 다른 스레드가 실행 중인 상태에서 동시에 진행된다는 것입니다.
3. **Remark**
   - 그 다음 단계에서는 `Concurrent Mark` 단계에서 새로 추가되거나 참조가 끊긴 객체를 확인합니다. 이 검증과정은 `stop-the-world`를 유발하기 때문에 `stop-the-world`지속시간을 최대한 줄이기 위해 멀티스레드로 검증 작업을 수행합니다.
4. **Concurrent Sweep**
   - 마지막으로  단계에서는 쓰레기를 정리하는 작업을 실행합니다. 이 작업도 다른 스레드가 실행되고 있는 상황에서 진행합니다.

이러한 단계로 진행되는 GC 방식이기 때문에 **stop-the-world 시간이 매우 짧습니다.** 모든 애플리케이션의 응답 속도가 매우 중요할 때 `CMS GC`를 사용하며, `Low Latency GC`라고도 부릅니다.

그런데 `CMS GC`는 `stop-the-world` 시간이 짧다는 장점에 반해 다음과 같은 단점이 존재합니다.

- 다른 GC 방식보다 메모리와 CPU를 더 많이 사용합니다.
- `Compaction` 단계가 기본적으로 제공되지 않습니다.

따라서, `CMS GC`를 사용할 때에는 신중히 검토한 후에 사용해야 합니다. 그리고 조각난 메모리가 많아 **Compaction 작업을 실행하면 다른 GC 방식의 stop-the-world 시간보다 더 길기 때문에 Compaction 작업이 얼마나 자주, 오랫동안 수행되는지 확인**해야 합니다.



#### G1(Garbage First) GC

`G1(Garbage First) GC`는 `Young 영역`과 `Old 영역`으로 나누는 방식을 사용하지 않습니다. `Eden`, `Survivor`, `Old` 영역이 존재하지만 고정된 크기로 고정된 위치에 존재하는 것이아니며, 전체 힙 메모리 영역을 `Region` 이라는 특정한 크기로 나눠서 각 `Region`의 상태에 따라 그 `Region`에 역할(`Eden, Survivor, Old`)이 동적으로 부여되는 상태입니다.  **JVM 힙은 2048개의 Region 으로 나뉠 수 있으며, 각 Region의 크기는 1MB ~ 32MB 사이로 지정될 수 있습니다.** (-XX:G1HeapRegionSize 로 설정) 즉, **`G1 GC`는 큰 힙 메모리에서 짧은 GC 시간을 보장하는데 그 목적**을 둡니다.

다음 그림에서 보다시피, **G1 GC는 바둑판의 각 Region에 객체를 할당하고 GC를 실행**합니다. **그러다가, 해당 Region이 꽉 차면 다른 영역에서 객체를 할당하고 GC를 실행**합다. 즉, **Young의 세가지 영역에서 데이터가 Old 영역으로 이동하는 단계가 사라진 GC 방식**이라고 이해하면 됩니다. `G1 GC`는 장기적으로 말도 많고 탈도 많은 `CMS GC`를 대체하기 위해서 만들어 졌습니다.

![G1Heap](https://mirinae312.github.io/img/jvm_gc/G1Heap.png)



`G1 GC`에서는 기존의 `Heap 영역`에서 보지 못한 `Humongous`, `Available/Unused` 이 존재하며 두 `Region`에 대한 역할은 아래와 같습니다.

- **Humongous** : `Region` 크기의 50%를 초과하는 큰 객체를 저장하기 위한 공간이며, 이 `Region` 에서는 GC 동작이 최적으로 동작하지 않습니다.
- **Available/Unused** : 아직 사용되지 않은 `Region`을 의미합니다.

`G1 GC`에서 `Young GC` 를 수행할 때는 `stop-the-world` 현상이 발생하며, `stop-the-world` 시간을 최대한 줄이기 위해 **멀티스레드로 GC를 수행**합니다. **`Young GC`는 각 Region 중 GC대상 객체가 가장 많은 Region(Eden 또는 Survivor 역할) 에서 수행** 되며, 이 Region에서 살아남은 객체를 다른 `Region(Survivor 역할)` 으로 옮긴 후, 비워진 영역을 사용가능한 영역으로 돌리는 형태 로 동작합니다.

![G1FullGC](https://user-images.githubusercontent.com/79291114/113676063-f87d1400-96f6-11eb-9a63-0aef76375c9c.png)



`G1 GC`에서 `Full GC` 가 수행될 때는 `Initial Mark -> Root Region Scan -> Concurrent Mark -> Remark -> Cleanup -> Copy` 단계를 거치게됩니다.

1. **Initial Mark**
   - `Old Region` 에 존재하는 객체들이 참조하는 `Survivor Region` 을 찾습니다. 이 과정에서는 `stop-the-world`현상이 발생하게 됩니다.
2. **Root Region Scan**
   - `Initial Mark` 에서 찾은 `Survivor Region`에 대한 GC 대상 객체 스캔 작업을 진행합니다.

3. **Concurrent Mark**
   - 전체 힙의 `Region`에 대해 스캔 작업을 진행하며, GC 대상 객체가 발견되지 않은 `Region` 은 이후 단계를 처리하는데 제외되도록 합니다.
4. **Remark**
   - 애플리케이션을 멈추고(`stop-the-world`) 최종적으로 GC 대상에서 제외될 객체(살아남을 객체)를 식별해냅니다.

5. **Cleanup**
   - 애플리케이션을 멈추고(`stop-the-world`) 살아있는 객체가 가장 적은 `Region` 에 대한 미사용 객체 제거를 수행합니다. 이후 `stop-the-world`를 끝내고, 앞선 GC 과정에서 완전히 비워진 `Region`을 `Freelist`에 추가하여 재사용될 수 있게 합니다.

6. **Copy**
   - GC 대상 `Region`이었지만 `Cleanup` 과정에서 완전히 비워지지 않은 `Region`의 살아남은 객체들을 새로운`(Available/Unused) Region` 에 복사하여 `Compaction` 작업을 수행한다.



##### G1 GC의 설정옵션

|                 Option                  | Default |                         Description                          |
| :-------------------------------------: | :-----: | :----------------------------------------------------------: |
|          -XX: G1HeapRegionSize          |         | Region 크기. 1MB~32MB 범위에서 설정 가능. 최소 힙크기를 2048 개의 Region 으로 나눌 수 있도록 설정해야함 |
|          -XX:MaxGCPauseMillis           |   200   | G1 GC가 유발하는 STW의 최대 시간. G1은 설정값을 최대한 맞추려고 노력할 뿐이며, 보장되는 값은 아닙니다. |
|       -XX:DefaultMinNewGenPercent       |    5    | Young 영역으로 사용할 힙 최소 크기 (전체 힙 크기대비 비율, %) |
|       -XX:DefaultMaxNewGenPercent       |   60    | Young 영역으로 사용할 힙 최대 크기 (전체 힙 크기대비 비율, %) |
|          -XX:ParallelGCThreads          |         | STW 상황에서 GC를 수행하는 스레드 개수. CPU core 수가 8개 이하인 경우 core 수와 동일하게 설정하는 것이 좋습니다. |
|            -XX:ConcGCThreads            |         | Concurrent Mark 를 수행하는 스레드 개수. ParallelGCThreads의 25% 로 설정하는 것이 좋습니다 |
|   -XX:InitiatingHeapOccupancyPercent    |   45    | 힙을 전체 크기 대비 특정 비율(%)만큼 사용하게될 경우 Mark 를 수행해야한다는 옵션 |
| -XX:G1OldCSetRegionLiveThresholdPercent |   65    |          Mixed GC 가 시작되는 Old Region 크기 비율           |

> **Mixed GC** : Full GC 를 완료하는 시점에 Young/Old Region을 동시에 GC



## GC와 Java Reference

Java의 `가비지 컬렉터(Garbage Collector)`는 그 동작 방식에 따라 매우 다양한 종류가 있지만 공통적으로 크게 다음 2가지 작업을 수행한다고 볼 수 있습니다.

1. `힙(heap)` 내의 객체 중에서 `가비지(garbage)`를 찾아냅니다다.
2. 찾아낸 `가비지`를 처리해서 힙의 메모리를 회수합니다.

**`JDK 1.2`부터는 `java.lang.ref` 패키지를 추가해 제한적이나마 사용자 코드와 GC가 상호작용**할 수 있습니다.



### GC와 Reachability

`Java GC`는 객체가 `가비지`인지 판별하기 위해서 `reachability`라는 개념을 사용합니다. **어떤 객체에 유효한 참조가 있으면 `reachable`로, 없으면 `unreachable`로 구별**하고, **`unreachable` 객체를 가비지로 간주해 `GC`를 수행**합니다. 

한 객체는 여러 다른 객체를 참조하고, 참조된 다른 객체들도 마찬가지로 또 다른 객체들을 참조할 수 있으므로 **객체들은 참조 사슬을 이룹니다.** 이런 상황에서 유효한 참조 여부를 파악하려면 **항상 유효한 최초의 참조가 있어야 하는데 이를 객체 참조의 `root set`**이라고 합니다.



<img src="https://user-images.githubusercontent.com/79291114/113660968-299e1a00-96e0-11eb-8a8e-e5331e85e91c.png" alt="reachability"  />

런타임 데이터 영역은 위와 같이 `스레드가 차지하는 영역`들과, `객체를 생성 및 보관하는 하나의 큰 힙`, `클래스 정보가 차지하는 영역인 메서드 영역`, 크게 세 부분으로 나눌 수 있습니다. 위 그림에서 객체에 대한 참조는 화살표로 표시되어 있습니다.

힙에 있는 객체들에 대한 참조는 다음 4가지 종류 중 하나입니다.

1. 힙 내의 다른 객체에 의한 참조

2. Java 스택, 즉 Java 메서드 실행 시에 사용하는 지역 변수와 파라미터들에 의한 참조

3. 네이티브 스택, 즉 `JNI(Java Native Interface)`에 의해 생성된 객체에 대한 참조

4. 메서드 영역의 정적 변수에 의한 참조

이들 중 1번의 힙 내의 다른 객체에 의한 참조를 제외한 나머지 3개가 `root set`으로, `reachability`를 판가름하는 기준이 됩니다.

**`root set`으로부터 시작한 참조 사슬에 속한 객체들은 `reachable` 객체**이고, **이 참조 사슬과 무관한 빨간색으로 표시된 객체들이 `unreachable` 객체**로 `GC` 대상 입니다. 빨간색 객체 중 오른쪽의 객체처럼 **`reachable` 객체를 참조하더라도, 다른 `reachable` 객체가 이 객체를 참조하지 않는다면 이 객체는 `unreachable` 객체**입니다.

이 그림에서 참조는 모두 `java.lang.ref` 패키지를 사용하지 않은 일반적인 참조이며, 이를 흔히 `strong reference`라 부릅니다.



### Reference

원래 `GC` 대상 여부는 `reachable`인가 `unreachable`인가로만 구분하였고 이를 사용자 코드에서는 관여할 수 없었습니다. 그러나 `java.lang.ref` 패키지를 이용하여 **`reachable` 객체들을 `strongly reachable`, `softly reachable`, `weakly reachable`, `phantomly reachable`로 더 자세히 구별하여 GC 때의 동작을 다르게 지정**할 수 있게 되었습니다. 다시 말해, `GC` 대상 여부를 판별하는 부분에 사용자 코드가 개입할 수 있게 되었다는 것입니다.

Java 스펙에서는 **`SoftReference`, `WeakReference`, `PhantomReference` 3가지 클래스에 의해 생성된 객체를 `reference object`라고 부릅니다**. 이는 흔히 `strong reference`로 표현되는 일반적인 참조나 다른 클래스의 객체와는 달리 **3가지 `Reference` 클래스의 객체에 대해서만 사용하는 용어**입니다. 또한 이들 **`reference object`에 의해 참조된 객체는 `referent`**라고 부릅니다.  Java 스펙 문서를 참조할 때 이들 용어를 명확히 알면 좀 더 이해하기 쉽습니다. 



#### strongly reachable

**`root set`으로부터 시작해서 어떤 `reference object`도 중간에 끼지 않은 상태로 참조 가능한 객체**, 다시 말해 객체까지 도달하는 여러 참조 사슬 중 `reference object`가 없는 사슬이 하나라도 있는 객체



#### softly reachable

`strongly reachable` 객체가 아닌 객체 중에서 `weak reference`, `phantom reference` 없이 `soft reference`만 통과하는 참조 사슬이 하나라도 있는 객체



#### weakly reachable

`strongly reachable` 객체도 `softly reachable` 객체도 아닌 객체 중에서 `phantom reference` 없이 `weak reference`만 통과하는 참조 사슬이 하나라도 있는 객체입니다. 

`GC`를 수행할 때마다 회수 대상이 됩니다. `GC`가 실제로 언제 객체를 회수할지는 `GC 알고리즘`에 따라 모두 다르므로, `GC`가 수행될 때마다 반드시 메모리까지 회수된다고 보장하지는 않습니다.



#### phantomly reachable

`strongly reachable` 객체, `softly reachable` 객체, `weakly reachable` 객체 모두 해당되지 않는 객체. `GC` 대상 여부를 결정하는 부분에 관여하는 `softly reachable`, `weakly reachable`과는 달리, `phantomly reachable`은 파이널라이즈와 메모리 회수 사이에 관여합니다. 

객체에 대한 참조가 `PhantomReference`만 남게 되면 해당 객체는 바로 `파이널라이즈(finalize)`됩니다. 그러나  `파이널라이즈`되었지만 아직 메모리가 회수되지 않은 상태입니다. 항상 `ReferenceQueue`를 필요로 합니다.

**PhantomReference의 `get()` 메서드는 SoftReference, WeakReference와 달리 항상 `null`을 반환**합니다. 따라서 한 번 `phantomly reachable`로 판명된 객체는 더 이상 사용될 수 없게 됩니다. 그리고 `phantomly reachable`로 판명된 객체에 대한 참조를 **GC가 자동으로 `null`로 설정하지 않으므로, 후처리 작업 후에 사용자 코드에서 명시적으로 clear() 메서드를 실행하여 null로 설정해야 메모리 회수가 진행**됩니다. 

##### ReferenceQueue

`SoftReference` 객체나 `WeakReference` 객체가 참조하는 객체가 GC 대상이 되면 `SoftReference` 객체, `WeakReference` 객체 내의 참조는 `null`로 설정되고 `SoftReference` 객체, `WeakReference` 객체 자체는 `ReferenceQueue`에 `enqueue`됩니다. (`ReferenceQueue`에 `enqueue`하는 작업은 `GC`에 의해 자동으로 수행됩니다.) 

**ReferenceQueue의 `poll()` 메서드나 `remove()` 메서드를 이용해 ReferenceQueue에 이들 `reference object`가 `enqueue`되었는지 확인하면 softly reachable 객체나 weakly reachable 객체가 GC되었는지를 파악할 수 있고, 이에 따라 관련된 리소스나 객체에 대한 후처리 작업을 할 수 있습니다.**



<img src="https://d2.naver.com/content/images/2015/06/helloworld-329631-5.png" alt="javareference5"  />



그림을 보면 **녹색으로 표시한 중간의 두 객체는 `WeakReference`로만 참조된 `weakly reachable` 객체**이고, **파란색 객체는 `strongly reachable` 객체**입니다.

**`GC`가 동작할 때, `unreachable` 객체뿐만 아니라 `weakly reachable` 객체도 가비지 객체로 간주되어 메모리에서 회수**됩니다. **`root set`으로부터 시작된 참조 사슬에 포함되어 있음에도 불구하고 `GC`가 동작할 때 회수**되므로, 참조는 가능하지만 반드시 항상 유효할 필요는 없는 **`LRU 캐시`와 같은 임시 객체들을 저장하는 구조를 쉽게 만들 수 있습니다.**

위 그림에서 **`WeakReference` 객체 자체(Weak Reference라고 명시되어 있는 부분)는 `weakly reachable` 객체가 아니라 `strongly reachable` 객체**입니다.

또한, 그림에서 A로 표시한 객체와 같이 **`WeakReference`에 의해 참조되고 있으면서 동시에 `root set`에서 시작한 참조 사슬에 포함되어 있는 경우에는 `weakly reachable` 객체가 아니라 `strongly reachable` 객체**입니다.

<u>`GC`가 동작하여 어떤 객체를 `weakly reachable` 객체로 판명하면, `GC`는 `WeakReference` 객체에 있는 `weakly reachable` 객체에 대한 참조를 `null`로 설정합니다. 이에 따라 `weakly reachable` 객체는 `unreachable` 객체와 마찬가지 상태가 되고, 가비지로 판명된 다른 객체들과 함께 메모리 회수 대상이 됩니다.</u>



[GC의 동작방식과 Reference에 대한 자세한 글](https://d2.naver.com/helloworld/329631)



---

참고 : [https://asfirstalways.tistory.com/159](https://asfirstalways.tistory.com/159)

[https://d2.naver.com/helloworld/1329](https://d2.naver.com/helloworld/1329)

[https://www.crocus.co.kr/1512](https://www.crocus.co.kr/1512)

[https://deveric.tistory.com/64](https://deveric.tistory.com/64)

[https://mirinae312.github.io/develop/2018/06/04/jvm_gc.html](https://mirinae312.github.io/develop/2018/06/04/jvm_gc.html)
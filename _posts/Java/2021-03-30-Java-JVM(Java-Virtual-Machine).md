---
title: "Java : JVM(Java Virtual Machine)"
excerpt_separator: <!--more-->
categories:
  - Java
tags:
  - Java
  - "Java : JVM(Java Virtual Machine)"
toc: true
toc_sticky: true
toc_label: 목차
---

# JVM (Java Virtual Machine)

`JVM`은 `Java Virtual Machine` 즉, **자바 가상 머신의 약자를 따서 줄여 부르는 용어**입니다. (가상머신이란 프로그램을 실행하기 위해 물리적 머신과 유사한 머신을 소프트웨어로 구현한 것입니다.) **`JVM`의 역할은 자바 어플리케이션을 클래스 로더를 통해 읽어 들여 자바 API와 함께 실행**하는 것입니다. **또, `JVM`은 Java와 OS사이에서 중계자 역할을 수행**하여 Java가 OS에 구애받지 않고 재사용을 가능하게 해줍니다. 그리고 **가장 중요한 메모리관리, Garbage Collection을 수행**합니다. ARM 아키텍쳐와 같은 하드웨어는 레지스터 기반으로 동작하는데에 반해  **`JVM`은 스택기반으로 동작**합니다.

동일한 기능의 프로그램이더라도 메모리 관리에 따라 성능이 좌우됩니다. 때문에 한정된 메모리를 효율적으로 사용하여 최고의 성능을 내기 위해 `JVM`을 알아야 할 필요가 있습니다.



## 자바프로그램 실행과정

<img src="https://user-images.githubusercontent.com/79291114/112918082-08b55200-913f-11eb-9c2c-fcb03aee8cf7.PNG" alt="자바실행과정" style="zoom: 80%;" />

1. 프로그램이 실행되면 `JVM`은 OS로부터 이 프로그램이 필요로 하는 메모리를 할당받습니다.
   - `JVM`은 이 메모리를 용도에 따라 여러 영역으로 나누어 관리합니다.
2. `자바 컴파일러(javac)`가 `자바 소스코드(.java)`를 읽어들여 `자바 바이트코드(.class)`로 변환시킵니다.
3. `Class Loader`를 통해 class파일들을 `JVM`으로 로딩합니다.
4. 로딩된 class파일들은 `Execution engine`을 통해 해석됩니다.
5. 해석된 바이트코드는 `Runtime Data Areas`에 배치되어 실질적인 수행이 이루어지게 됩니다.
   - 이러한 실행과정 속에서 `JVM`은 필요에 따라 `Thread Synchronization`과 `GC(Garbage Collection)`같은 관리작업을 수행합니다.



## JVM 구성



### Class Loader (클래스 로더)

**`JVM`내로 클래스(`.class파일`)를 로드하고, 링크를 통해 배치하는 작업을 수행하는 모듈**입니다. **Runtime 시에 동적으로 클래스를 로드**합니다. jar파일 내 저장된 클래스들을 `JVM`위에 탑재하고 사용하지 않는 클래스들은 메모리에서 삭제합니다.(**컴파일러 역할**) 자바의 동적코드는 컴파일 타임이 아니라 런타임에 참조합니다. 즉, **클래스를 처음으로 참조할 때, 해당 클래스를 로드하고 링크한다는 것**입니다. 그 역할을 `클래스 로더`가 수행합니다.

#### Class Loading 과정

##### 1. Complie

보통 Java로 코딩을 하게되면, `.java` 파일형식으로 작성을 하게되는데 해당 파일은 컴퓨터에서 바로 실행가능한 파일이 아닙니다. **실행가능하게 만들기 위해 컴파일 과정**을 거치게 되는데, 이 과정으로 통해서 `.java`파일은 `.class`파일로 컴파일됩니다.

##### 2. Load

이제 실행시킬수 있는 `.class`파일이 만들어 졌기때문에 **해당 파일을 메모리상에 올려 실행시켜야 합니다. 해당 과정을 Class loading**이라 하며, 이 과정을 클래스 로더(Class Loader)가 주체적으로 수행하게 됩니다. 클래스로더 여러 개로 구성되어 있어 유저가 작성한 클래스뿐만 아니라 목적에 따라 부트스트랩 클래스, 확장 클래스등의 로드를 함께 수행합니다. 실제적인 로더의 동작 순서는 다음과 같습니다.

**BootstrapClassLoader -> Extensions Class Loader -> System Class Loader -> User-Defined Class Loader**

##### 3.  Link

링크과정에서는 `Verification, Prepare, Resolve`하는 과정을 진행합니다.

- `Verification(확인)` : 과정을 통해 로드된 클래스의 바이트구조가 올바른지 검사해 보안을 확인합니다.
- `Prepare(준비)` : 과정을 통해 클래스 및 인터페이스의 static 필드를 생성하고 기본값으로 초기화 합니다.
- `Resolve(해석)` : 과정을 통해 인스턴스들의 실제 주소값에 대한 심볼릭 링크를 설정해줍니다.

> 심볼릭 링크 : 링크를 연결하여 원본 파일을 직접 사용하는 것과 같은 효과를 내는 링크

##### 4. Initialization

**Link단계에서 기본값으로 초기화된 static 필드들에 대해 정의된 값을 지정**해줍니다.

##### 5. Run main

 main 을 찾아 main 스레드를 실행 시켜줍니다.

#### Class Loader의 특징

1. `계층적 구조` : 위에서 클래스 로더의 실행순서를 보면 알 수 있다시피, bootstrap으로 부터 시작되는 클래스로더들이 계층적으로 구성되어 있음을 알 수 있습니다.

2. `unload 불가` : 한 번 Class Loader에 의해 로드된 클래스는 해제될 수 없습니다.

#### 로드타임 로딩 & 런타임 로딩

##### 로드타임 로딩

일반적으로 클래스를 로딩하는 로드타임에는 **해당 class및 import로 주입되어있는 클래스들을 로드**하게 됩니다.

```java
import java.xxx.xxx;

public class Example{
    public static main(String[] args){
        //something code;
    }
}
```

위와 같이 class를 로드할 때는 import되어있는 java.xxx.xx를 함께 로드하게 되는데, **타겟 클래스를 로드하는 시점에서 함께 일어나는 로드들을 로드타임 로딩**이라고 합니다.

##### 런타임 로딩

이름에서 알수 있다시피 런타임시 로딩이 일어나게 됩니다.

```java
import java.xxx.xxx;

public class Example{
    public static main(String[] args){
    //something code;
        Class dynamicClass = Class.forName("className");
        Object obj = dynamicClass.getInstance();
    }
}
```

위와 같이 코드를 작성했을 때, main 이 실행되면서 Class.forName() 호출을 통해 새로운 클래스를 참조하게 됩니다. **현재 main 함수를 동작시키는 중에 class를 로드하게되는데, 이것을 런타임 로딩**이라고 합니다.



**참고 링크**

[Class Loader와 ClassNotFound나 ClassCastException의 연관성 ](https://www.kdata.or.kr/info/info_04_view.html?dbnum=183810)





### Execution Engine (실행 엔진)

**클래스를 실행시키는 역할**입니다. **`클래스 로더`가 `JVM`내의 런타임 데이터 영역에 바이트 코드를 배치**시키고, 이것은 **실행엔진에 의해 실행**됩니다. 자바 바이트코드는 기계가 바로 수행할 수 있는 언어이기 보다는 비교적 인간이 보기 편한 형태로 기술된 것입니다. 그래서 `실행 엔진`은 이와 같은 **바이트코드를 실제로 `JVM`내부에서 기계가 실행할 수 있는 형태로 변경**합니다. (이 과정 때문에 C언어 같은 네이티브 언어에 비해 속도가 느렸지만 `JIT(Just In Time)`을 이용해 이 점을 극복하였습니다.) 실행엔진은  `Interpreter(인터프리터)`, `JIT(Just In Time)` 두가지 방식을 사용하게 됩니다.

#### Interpreter (인터프리터)

실행 엔진은 **자바 바이트 코드를 명령어 단위로 읽어서 실행**합니다. 하지만 이 방식은 **한 줄씩 수행하기 때문에 느리다**는 인터프리터 언어의 단점을 그대로 가지고 있습니다.

#### JIT (Just In Time)

인터프리터 방식의 단점을 보완하기 위해 도입된 `JIT` 컴파일러입니다. **인터프리터 방식으로 실행하다가 적절한 시점에 바이트코드 전체를 컴파일하여 네이티브 코드(CPU와 운영체제(OS)가 직접 실행할 수 있는 코드)로 변경**하고, **이후에는 더 이상 인터프리팅 하지 않고 네이티브 코드로 직접 실행하는 방식**입니다. **네이티브 코드는 캐시에 보관하기 때문에 한 번 컴파일된 코드는 빠르게 수행**하게 됩니다. 하지만 `JIT`컴파일러가 컴파일하는 과정은 바이트코드를 인터프리팅하는 것보다 훨씬 오래걸리기 때문에 한 번만 실행되는 코드라면 컴파일하지 않고 인터프리팅하는 것이 유리합니다. 따라서 `JIT` 컴파일러를 사용하는 `JVM`들은 내부적으로 해당 메서드가 얼마나 자주 수행되는지 체크하고, 일정 정도를 넘을 때에만 컴파일을 수행합니다.





### Garbage Collector

`GC(Garbage Collection)`를 수행하는 모듈





### Runtime Data Area

**프로그램을 수행하기 위해 OS에서 할당받은 메모리 공간**입니다.

<img src="https://user-images.githubusercontent.com/79291114/112918062-02bf7100-913f-11eb-8de6-bad7fc0fa3f8.PNG" alt="Runtime-Data-Area" style="zoom:80%;" />

<img src="https://miro.medium.com/max/1568/1*Zsmrw8DvVSLpRr0mvdNCuA.png" alt="img" style="zoom: 50%;" />

#### PC Register

`Thread`가 시작될 때 생성되며 `Thread`가 생성될 때마다 생성되는 공간으로 `Thread`마다 하나씩 존재합니다. **`Thread`가 어떤 부분을 어떤 명령으로 실행해야할지에 대한 기록을 하는 부분으로 현재 수행 중인 `JVM` 명령의 주소를 가집니다.**

#### JVM stack

**프로그램 실행과정에서 임시로 할당되었다가 메소드를 빠져나가면 바로 소멸되는 특성의 데이터를 저장하기 위한 영역**입니다. **각종 형태의 변수나 임시 데이터, 스레드나 메소드의 정보를 저장**합니다. 메소드 호출 시마다 각각의 스택 프레임(그 메서드만을 위한 공간)이 생성됩니다. 메서드 수행이 끝나면 프레임 별로 삭제합니다. **메소드 안에서 사용되는 값들(local variable)을 저장**합니다. 또, **호출된 메소드의 매개변수, 지역변수, 리턴 값 및 연산 시 일어나는 값들을 임시로 저장**합니다.

#### Native method stack

자바 프로그램이 컴파일되어 생성되는 바이트 코드가 아닌 **실제 실행할 수 있는 기계어로 작성된 프로그램을 실행 시키는 영역**입니다. **`Java`가 아닌 네이티브 방식을 사용하는 다른 언어(C언어 같은)로 작성된 코드를 위한 공간**입니다. `Java Native Interface`를 통해 바이트 코드로 전환하여 저장하게 됩니다. 일반 프로그램처럼 커널이 스택을 잡아 독자적으로 프로그램을 실행시키는 영역입니다. 이 부분을 통해 C code를 실행시켜 Kerneal에 접근할 수 있습니다.

##### Java Native Interface

`Java Native Interface`는 자바 가상 머신(JVM) 위에서 실행되고 있는 자바코드가 네이티브 응용 프로그램(하드웨어와 운영 체제 플랫폼에 종속된 프로그램들) 그리고 C, C++ 그리고 어샘블리 같은 다른 언어들로 작성된 라이브러리들을 호출하거나 반대로 호출되는 것을 가능하게 하는 프로그래밍 프레임워크입니다.

#### Method Area (== Class area == Static area)

**클래스 정보를 처음 메모리 공간에 올릴 때 초기화되는 대상을 저장하기 위한 메모리 공간**입니다. **올라가게 되는 메소드의 바이트 코드는 프로그램의 흐름을 구성하는 바이트 코드**입니다. **자바 프로그램은 main 메소드의 호출에서부터 계속된 메소드의 호출로 흐름을 이어가기 때문**입니다. 대부분 인스턴스의 생성도 메소드 내에서 명령하고 호출합니다. 사실상 컴파일 된 바이트코드의 대부분이 메소드 바이트코드이기 때문에 거의 모든 바이트코드가 올라간다고 봐도 무방합니다. 이 공간에는 `Runtime Constant Pool` 이라는 별도의 관리 영역도 함께 존재합니다. **`Runtime Constant Pool`은 상수 자료형의 레퍼런스만 저장하여 참조하고 중복을 막는 역할을 수행**합니다.

##### 올라가는 정보의 종류

###### Field Information

맴버변수의 이름, 데이터 타입, 접근 제어자에 대한 정보

###### Method Information

메소드의 이름, 리턴타입, 매개변수, 접근제어자에 대한 정보

###### Type Information

class인지 interface인지의 여부 저장 Type의 속성, 전체 이름, super class의 전체 이름(interface 이거나 Object인 경우 제외)

> *Method Area는 클래스 데이터를 위한 공간이라면 Heap영역이 객체를 위한 공간입니다.*
>
> *Heap과 마찬가지로 GC(Garbage Collection)의 관리 대상에 포함됩니다.*

#### Heap (힙 영역)

<img src="https://user-images.githubusercontent.com/79291114/112918063-03580780-913f-11eb-8a8e-502aad52633d.PNG" alt="Heap"  />

**객체를 저장하는 가상 메모리 공간**입니다. **new 연산자로 생성된 객체와 배열을 저장**합니다. 물론 **`Method area`영역에 올라온 클래스들만 객체로 생성**할 수 있습니다. 힙은 `Permanent Generation`, `New/Young 영역`, `Old 영역` 이렇게 세 부분으로 나눌 수 있습니다.

##### Permanent Generation

**생성된 객체정보의 주소값이 저장된 공간**입니다. **`Class loader`에 의해 load되는 Class, Method 등에 대한 Meta 정보가 저장되는 영역**이고 `JVM`에 의해 사용됩니다. `Reflection`(객체를 통해 클래스의 정보를 분석해 내는 프로그램 기법)을 사용하여 동적으로 클래스가 로딩되는 경우에 사용됩니다. 내부적으로 `Reflection` 기능을 자주 사용하는 `Spring Framework`를 이용할 경우 이영역에 대한 고려가 필요합니다.

##### New/Young 영역

- **Eden** : 객체들이 최초로 생성되는 공간
- **Survivor 0 / 1** : Eden에서 참조되는 객체들이 저장되는 공간(**Survivor 영역 중 하나는 반드시 비어 있어야 합니다.**)

**새롭게 생성한 객체의 대부분의 객체는 `Eden` 영역에 위치**합니다. **`Eden` 영역에서 `GC`가 한 번 발생한 후 살아남은 객체는 `Survivor` 영역 중 하나로 이동**합니다. 이렇게 계속 **`Survivor` 영역에 객체가 계속 쌓이다가 가득 차게 되면, 이 `Survivor` 영역 중 살아남은 객체를 다른 `Survivor`영역으로 이동**합니다. 그 후, **가득찬 `Survivor` 영역은 아무 데이터도 없는 상태가 됩니다.** 이 과정을 반복하다가 **계속해서 살아남아 있는 객체는 `Old` 영역으로 이동**하게 됩니다. 

대부분의 객체가 금방 접근 불가능 상태가 되기 때문에 매우 많은 객체가 `Young` 영역에 생성되었다가 사라집니다. **이 영역에서 객체가 사라질 때 `Minor GC`가 발생한다고 말합니다.**

##### Old 영역

**`Young` 영역에서 일정 시간 참조되고 있는, 살아남은 객체들이 저장되는 공간**입니다. **대부분 `Young` 영역보다 크게 할당**하며, 크기가 큰 만큼 `Young` 영역보다 `GC`는 적게 발생합니다. **이 영역에서 객체가 사라질 때 `Major GC(혹은 Full GC)`가 발생한다고 말합니다.**

인스턴스는 소멸 방법과 소멸 시점이 지역 변수와는 다르기에 힙이라는 별도의 영역에 할당됩니다. 자바 가상 머신은 더 이상 인스턴스의 존재 이유가 없을 때 소멸시킵니다.



글을 보다보면 `GC(Garbage Collection)`이라는 단어가 엄청 많이 나옵니다.  그럼 이 `Garbage Collection`이란 무엇이고 어떻게 실행될까요??

[Garbage Collection 이란?](https://dev-splin.github.io/java/Java-GC(Garbage-Collection)/)



---

참고 : [https://asfirstalways.tistory.com/159](https://asfirstalways.tistory.com/159)

[https://d2.naver.com/helloworld/1329](https://d2.naver.com/helloworld/1329)

[https://medium.com/@lazysoul/jvm-%EC%9D%B4%EB%9E%80-c142b01571f2](https://medium.com/@lazysoul/jvm-%EC%9D%B4%EB%9E%80-c142b01571f2)

[https://taes-k.github.io/2019/07/16/java-class-loading/](https://taes-k.github.io/2019/07/16/java-class-loading/)

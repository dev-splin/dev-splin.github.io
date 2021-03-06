---
title: "Java : 스트림, 스트림 파이프라인"
excerpt_separator: <!--more-->
categories:
  - Java
tags:
  - Java
  - "Java : Stream"
toc: true
toc_sticky: true
toc_label: 목차
---

# Stream(스트림)

- 스트림은 컬렉션(배열포함)의 요소를 하나씩 참조해서 람다식을 처리할 수 있는 반복자이다.

- 람다식으로 요소 처리 코드를 제공한다.

  - 스트림이 제공하는 대부분의 요소처리 메소드는 함수형 인터페이스의 파라미터 타입을 가진다.
  - 파라미터 값으로 람다식 또는 메소드 참조를 대입할 수 있다.

- 내부 반복자를 사용하게 되어 병렬 처리가 쉬워진다.

  - 외부 반복자 : 개발자가 코드로 직접 컬렉션 요소를 반복해서 요청하고 가져오는 코드 패턴
  - 내부 반복자 : 컬렉션 내부에서 요소들을 반복시키고 개발자는 요소당 처리해야할 코드만 제공하는 패턴

- 내부 반복자의 이점

  - 개발자는 요소 처리 코드에만 집중하게 한다.
  - 멀티 코어 CPU를 최대한 활용하기 위해 요소들을 분배시켜 병렬처리 작업을 할 수 있다.

  

## 병렬(parallel)처리

  - 한가지 작업을 서브작업으로 나누고 서브작업들을 분리된 스레드에서 병렬적으로 처리한 후 서브 작업들의 결과들을 최종 결합하는 방법
  - 자바는 ForkJoinPool이라는 프레임워크를 이용해서 병렬 처리를 한다. (JAVA API)

```java
public class Print {	// 메소드 참조를 사용하기 위한 객체
	public void print(String a) {
		System.out.println(a+",");
	}
}

public class StreamTestMain {
	public static void main(String[] args) {
    List<String> list = Arrays.asList("수지","설현","BTS");
        
		// 순차적 처리 
		Stream<String> stream = list.stream();
		stream.forEach(s -> { 
			System.out.println(s);  // 람다식을 사용하여 요소처리
		});
		
		// 병렬로 처리 
		Stream<String> parallelStream = list.parallelStream();
		parallelStream.forEach(s -> {
			System.out.println(s);
		});
    
    // 배열로 부터 스트림 얻기 
		ParallelExam p = new Print(); // 메소드 참조를 위한 객체 생성
		String[] strArray = {"홍길동1","홍길동2","홍길동3","홍길동4","홍길동5"};
		Stream<String> strStream = Arrays.stream(strArray);
		strStream.forEach(p::print);  // 메소드 참조
		
		// 숫자 범위로 부터 스트림 얻기
		// IntStream stream = IntStream.range(1, 100); // 1-99
		IntStream stream = IntStream.rangeClosed(1, 100); // 1-100
		stream.forEach(a ->{
			int sum=0;
			sum += a; 
			System.out.println("합 : "+sum);
		});
    
  }
}

```



## 스트림 파이프라인

- 파이프라인 : 여러 개의 스트림이 파이프처럼 연결되어 있는 구조
- 스트림은 중간처리와 최종처리를 파이프라인(pipelines)으로 해결한다.



### Reduction (리덕션)

- 대량의 데이터를 가공해서 축소하는 것을 말한다.
  - 합계, 평균, 카운팅, 최대값, 최소값등을 집계하는 것
- 요소가 리덕션의 결과물로 바로 집계할 수 없을 경우 중간처리가 필요하다.
- 중간처리한 요소를 최종처리해서 리덕션 결과물을 산출한다.



### 중간처리

- 중간처리 메소드는 필터링, 매핑, 정렬, 그룹핑등이 있다.
- 중간처리 메소드는 리턴타입이 스트림이다.



### 최종처리

- 최종처리 메소드는 합계, 평균, 카운팅, 최대값, 최소값등이 있다.
- 최종처리 메소드는 리턴타입이 기본타입이거나 OptionalXXX 이다.



### 파이프라인 형성 과정

1. 중간처리 메소드는 중간처리 된 스트림을 리턴한다.
1. 이 스트림에서 다시 중간처리 메소드를 호출해서 파이프 라인을 형성하게 된다.
1. 스트림소스(컬렉션, 배열, 파일) -> 오리지널(중간) -> 필터로 처리(중간) -> 매핑처리(중간) -> 집계처리(최종)

- 최종 스트림의 집계기능이 시작되기 전까지 중간처리는 지연(lazy)된다.

  - 최종 스트림이 시작하면 비로소 컬렉션에서 요소가 하나씩 중간스트림에서 처리되고 최종스트림까지 오게 된다.

  

#### 예제코드

- distict()
  - Stream에서는 equals() 메소드가 true로 나오면 동일한 객체로 판단하고 중복을 제거
  - IntStream, LongStream, DoubleStream에서는 동일 값일 경우 중복을 제거
- filter()
  - 파라미터 값으로 주어진 Predicate가 true를 리턴하는 요소만 필터링
  - Predicate : 수학에서 결과로 true 또는 false를 반환하는 함수
- mapToInt()
  - 주어진 함수를 스트림의 요소에 적용한 결과를 IntStream으로 반환한다.
- getAsDouble()
  - 이 안에 값이 있으면 값을 OptionalDouble을 반환하고 그렇지 않으면 throw NoSuchElementException을 반환한다.
  - OptionalDouble : double 값을 가지고 있는지 아닌지를 나타내는 컨테이너 클래스 (해당 객체 안에 값이 존재하면 그 값을 반환하고 없으면 other를 반환합니다.

```java
public class Member {
	public static int MALE = 0; 
	public static int FEMALE = 1;
	
	private String name; 
	private int sex; 
	private int age; 
	
	public Member(String name, int sex, int age) {
		this.name = name;
		this.sex = sex;
		this.age = age;
	}

	public int getSex() {
		return sex;
	}

	public int getAge() {
		return age;
	}
}

public static void main(String[] args) {
		List<String> names = Arrays.asList("정인선","조수아","정인선","배수지","조두산","조형기");
    
   		names.stream()
			.distinct()
			.filter(n -> n.startsWith("배"))
			.forEach(n -> System.out.println(n));
        
		List<Member> list = Arrays.asList(
			new Member("홍길동1", Member.MALE, 10),
			new Member("홍길동2", Member.MALE, 20),
			new Member("홍길동3", Member.MALE, 30),
			new Member("홍길동4", Member.MALE, 10)
		);
    
		double ageAvg = list.stream()            // 오리지널 스트림
			.filter(m -> m.getSex()==Member.MALE)// 
			.mapToInt(Member::getAge)            // 중간
			.average()                           // 최종
			.getAsDouble();
		
		System.out.println("남자들의 평균 나이 : "+ageAvg);   
	}

```


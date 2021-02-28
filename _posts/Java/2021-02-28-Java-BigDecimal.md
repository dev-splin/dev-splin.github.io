---
title: "Java : BigDecimal"
excerpt_separator: <!--more-->
categories:
  - Java
tags:
  - Java
  - "Java : BigDecimal"
toc: true
toc_sticky: true
toc_label: 목차
---

# BigDecimal

- Java언어에서 숫자를 정밀하게 저장하고 표현할 수 있는 유일한 방법입니다.
- 소수점을 저장할 수 있는 가장 크기가 큰 타입인 double은 소수점의 정밀도에 있어 한계가 있어 값이 유실될 수 있습니다.
- Java언어에서 돈과 소수점을 다룬다면 BigDecimal은 선택이 아니라 필수입니다.
- 유일한 단점은 느린 속도와 기본 타입보다 조금 불편한 사용법 뿐입니다.

- BigDecimal타입은 내부적으로 수를 십진수로 저장하여 아주 작은 수과 큰 수의 연산에 대해 거의 무한한 정밀도를 보장합니다.



## BigDecimal 기본 용어

- **precision** : 숫자를 구성하는 전체 자리수라고 생각하면 편하나, 정확하게 풀이하면 왼쪽부터 0이 아닌 수가 시작하는 위치부터, 오른쪽부터 0이 아닌 수로 끝나는 위치까지의 총 자리수입니다. **unscale**과 동의어입니다. (ex: 012345.67890의 **precision**은 11이 아닌 9입니다.)
- **scale** : 전체 소수점 자리수라고 생각하면 편하나, 정확하게 풀이하면 소수점 첫째 자리부터, 오른쪽부터 0이 아닌 수로 끝나는 위치까지의 총 소수점 자리수입니다. **fraction** 동의어입니다. (ex: 012345.67890의 **scale**은 4이다. 하지만 0.00, 0.0의 **scale**은 모두 1이다.) **BigDecimal은 32bit의 소수점 크기를 가집니다.**
- **DECIMAL128** : **IEEE 754-2008**에 의해 표준화된 부호와 소수점을 수용하며 최대 34자리까지 표현 가능한 10진수를 저장할 수 있는 형식입니다. 2018년 미국 정부의 총 부채액이 15조 7천 500억 달러로 총 14자리 임을 감안하면, 금융권에서 처리되는 대부분의 금액을 수용할 수 있는 크기입니다. Java에서는 BigDecimal 타입을 통해 공식적으로 지원합니다.



### BigDecimal 기본  상수

float, double타입과 다르게 BigDecimal 타입은 초기화가 장황한 편입니다. 그래서 자주 쓰는 0, 1, 100은 쓰기 편하게 미리 상수로 정의되어 있습니다.

```java
// 흔히 쓰이는 값은 상수로 정의
// 0
BigDecimal.ZERO

// 1
BigDecimal.ONE

// 10
BigDecimal.TEN
```



### BigDecimal 초기화

double타입으로 부터 BigDecimal 타입을 초기화하는 방법으로 가장 안전한 것은 문자열의 형태로 생성자에 전달하여 초기화하는 것입니다. double타입의 값을 그대로 전달할 경우 이진수의 근사치를 가지게 되어 예상과 다른 값을 얻을 수 있습니다.

```java
// double 타입을 그대로 초기화하면 기대값과 다른 값을 가집니다.
// 0.01000000000000000020816681711721685132943093776702880859375
new BigDecimal(0.01);

// 문자열로 초기화하면 정상 인식
// 0.01
new BigDecimal("0.01");

// 위와 동일한 결과, double#toString을 이용하여 문자열로 초기화
// 0.01
BigDecimal.valueOf(0.01);
```



### BigDecimal 비교 연산

BigDecimal은 기본 타입이 아닌 오브젝트이기 때문에 특히, 동등 비교 연산을 유의해야 합니다. double 타입을 사용하던 습관대로 무의식적으로 `==` 기호를 사용하면 예기치 않은 연산 결과를 초래할 수 있습니다.

```java
BigDecimal a = new BigDecimal("2.01");
BigDecimal b = new BigDecimal("2.010");

// 객체의 레퍼런스 주소에 대한 비교 연산자로 무의식적으로 값의 비교를 위해 사용하면 오동작
// false
a == b;

// 값의 비교를 위해 사용, 소수점 맨 끝의 0까지 완전히 값이 동일해야 true 반환
// false
a.equals(b);

// 값의 비교를 위해 사용, 소수점 맨 끝의 0을 무시하고 값이 동일하면 0, 적으면 -1, 많으면 1을 반환
// 0
a.compareTo(b);
```



### BigDecimal 사칙 연산

정확한 실수 값을 계산하려면 BigDecimal 사칙연산을 이용하는게 좋습니다. BigDecimal의 `doubleValue()` 나 `floatValue()` 를 통해 정확한 실수를 가져왔다고 하더라도 연산중에 오차범위가 생겨나기 때문입니다.

Java에서 BigDecimal 타입의 사칙 연산 방법은 아래와 같습니다. 

```java
BigDecimal a = new BigDecimal("10");
BigDecimal b = new BigDecimal("3");

// 더하기
// 13
a.add(b);

// 빼기
// 7
a.subtract(b);

// 곱하기
// 30
a.multiply(b);

// 나누기
// 3.333333...
// java.lang.ArithmeticException: Non-terminating decimal expansion; no exact representable decimal result.
a.divide(b);

// 나누기
// 3.333
a.divide(b, 3, RoundingMode.HALF_EVEN);

// 나누기 후 나머지
// 전체 자리수를 34개로 제한
// 1
a.remainder(b, MathContext.DECIMAL128);

// 절대값
// 3
new BigDecimal("-3").abs();

// 두 수 중 최소값
// 3
a.min(b);

// 두 수 중 최대값
// 10
a.max(b);
```



### BigDecimal 소수점 처리

`RoundingMode.HALF_EVEN`은 Java의 기본 반올림 정책으로 금융권에서 사용하는 `Bankers Rounding`와 동일한 알고리즘입니다. 금융권에서는 시스템 개발시 혼란을 막기 위해 요구사항에 반올림 정책을 명확히 명시하여 개발합니다.

```java
// 소수점 이하를 절사한다.
// 1
new BigDecimal("1.1234567890").setScale(0, RoundingMode.FLOOR);

// 소수점 이하를 절사하고 1을 증가시킵니다.
// 2
new BigDecimal("1.1234567890").setScale(0, RoundingMode.CEILING);

// 음수에서는 소수점 이하만 절사합니다.
// -1
new BigDecimal("-1.1234567890").setScale(0, RoundingMode.CEILING);

// 소수점 자리수에서 오른쪽의 0 부분을 제거한 값을 반환합니다.
// 0.9999
new BigDecimal("0.99990").stripTrailingZeros();

// 소수점 자리수를 재정의합니다.
// 원래 소수점 자리수보다 작은 자리수의 소수점을 설정하면 예외가 발생합니다.
// java.lang.ArithmeticException: Rounding necessary
new BigDecimal("0.1234").setScale(3);

// 반올림 정책을 명시하면 예외가 발생하지 않습니다.
// 0.123
new BigDecimal("0.1234").setScale(3, RoundingMode.HALF_EVEN);

// 소수점을 남기지 않고 반올림합니다.
// 0
new BigDecimal("0.1234").setScale(0, RoundingMode.HALF_EVEN);
// 1
new BigDecimal("0.9876").setScale(0, RoundingMode.HALF_EVEN);
```



#### BigDecimal 문자열 변환 출력

`.setScale()`을 사용하여 소수점 자리수를 제한하면 원본의 소수점 값은 상실해 버립니다. 문자열로 출력하는 것이 목적이라면 `NumberFormat` 클래스를 사용하는 것이 적합합니다.

```java
NumberFormat format = NumberFormat.getInstance();
format.setMaximumFractionDigits(6);
format.setRoundingMode(RoundingMode.HALF_EVEN);
// 0.123457
format.format(new BigDecimal("0.1234567890"));
```



### BigDecimal 나누기 처리

```java
BigDecimal b10 = new BigDecimal("10");
BigDecimal b3 = new BigDecimal("3");

// 나누기 결과가 무한으로 떨어지면 예외 발생
// java.lang.ArithmeticException: Non-terminating decimal expansion; no exact representable decimal result.
b10.divide(b3);

// 반올림 정책을 명시하면 예외가 발생하지 않음
// 3
b10.divide(b3, RoundingMode.HALF_EVEN);

// 반올림 자리값을 명시
// 3.333333
b10.divide(b3, 6, RoundingMode.HALF_EVEN);

// 3.333333333
b10.divide(b3, 9, RoundingMode.HALF_EVEN);

// 전체 자리수를 7개로 제한하고 HALF_EVEN 반올림을 적용한다.
// 3.333333
b10.divide(b3, MathContext.DECIMAL32);

// 전체 자리수를 16개로 제한하고 HALF_EVEN 반올림을 적용한다.
// 3.333333333333333
b10.divide(b3, MathContext.DECIMAL64);

// 전체 자리수를 34개로 제한하고 HALF_EVEN 반올림을 적용한다.
// 3.333333333333333333333333333333333
b10.divide(b3, MathContext.DECIMAL128);

// 전체 자리수를 제한하지 않는다.
// java.lang.ArithmeticException: Non-terminating decimal expansion; no exact representable decimal result. 예외가 발생한다.
b10.divide(b3, MathContext.UNLIMITED);
```



### BigDecimal의 다양한 활용방법

BigDeciaml의 다양한 활용 방법은 제가 참고한 [지단로보트님 블로그](https://jsonobject.tistory.com/466)에서 확인하실 수 있습니다!

---

참고 : https://jsonobject.tistory.com/466




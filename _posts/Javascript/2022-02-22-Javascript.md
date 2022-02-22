---
title: "Javscript : Javascript 알쓸신잡(알아두면 쓸모있는 신기한 잡아스크립트)"
excerpt_separator: <!--more-->
categories:
  - Javscript 
tags:
  - Javscript 
toc: true
toc_sticky: true
toc_label: 목차
---

# Javascript

저는 기존에 Java와 Spring 공부를 했었는데, 회사에 입사 후 Javascript를 다룰 일이 많아졌습니다.

또, 개인적으로 만들어 보고 싶은 서비스가 생겼는데, Javascript로 프론트 엔드 + 백 엔드를 전부 다룰 수 있다는 것이 매력적이게 다가왔습니다.

그래서 **조현영(제로초) 님의 렛츠기릿 자바스크립트**를 보면서 제가 생소한 부분들을 정리해논 글 입니다.

> 단순히 제 주관으로 판단하고 작성하는 글이기 때문에 가이드가 될 수는 없으니 주의해주시면 감사하겠습니다 :star2:





### REPL(Read Eval Print Loop)

기본적으로 Javascript는 컴퓨터가 한줄 씩 읽는 인터프리터 방식의 언어입니다.

이 Javascript는 브라우저의 콘솔에서도 사용할 수 있습니다. 이 때, 브라우저의 콘솔에서 **한 줄씩 입력(Read)**받고 받은 **입력을 평가(Eval)**한 후 **결과를 출력(Print)**합니다. 그 후 다시 프롬프트가 나타나 **새로운 입력을 기다립니다.(Loop)**

이 과정을 `REPL(Read-Eval-Print-Loop)`라고 합니다.





### Number와 parseInt/parseFloat의 차이

아래의 예시와 같이 `Number`는 안에 들어있는 값이 전부 숫자가 아니라면 `Nan(Not a Number)`를 반환하고 `parseInt/parseFloat`은 **Int와 Float으로 인식할 수 있는 위치까지 읽어 해당 타입**으로 반환합니다.

```javascript
Number('234오오오테스트324'); // Nan(Not a Number)

parseInt('234오오오테스트324'); // 234
parseInt('234.23423오오오테스트324'); //234

parseFloat('234오오오테스트324'); // 234
parseFloat('234.23423오오오테스트324'); // 234.23423
```

> 참고로 `Nan`을 `typeof`로 체크해보면 숫자라고 나옵니다.





### prompt()

값을 입력할 수 있는 시스템 창을 출력합니다.





### 빼기(-) 연산자

`1 + '0'` 처럼 더하기 연산자는 숫자가 문자열로 변환 후, 더하기 연산을 진행하는것은 대부분이 다 아는 사실일 겁니다.

하지만, 빼기 연산자는 문자열이 숫자로 바뀐 후 연산합니다. 때문에 **문자열에 숫자가 아닌 다른 문자가 들어올 경우 Nan을 반환**합니다.

즉, 위에서 말한 것 처럼 문자열이 `parseInt`보다는 `Number`가 적용되는 것이라고 볼 수 있습니다.





### NaN끼리 비교하기

값 중에서 NaN은 비교할 때 독특한 성질을 가집니다. 바로 NaN끼리 비교할 때 false 값을 가진다는 것입니다.

```javascript
> NaN == NaN;
< false
```





### 문자열 코드 보기

`charCodeAt()`이라는 메서드를 사용하면 해당 문자가 어떤 코드 번호를 가지고 있는지 알 수 있습니다.'

```javascript
> '&'.charCodeAt();
< 65286
> 'a'.charCodeAt();
< 97
> 'b'.charCodeAt();
< 98
```





### 문자와 숫자 비교

Javascript에서 문자와 숫자를 비교할 때, **문자를 숫자로 변환 후 비교**하게 됩니다. 이 때, 숫자로 변환할 수 없는 문자는 `NaN`이 됩니다.

```javascript
> '1' < 5
< true
> 'abc' < 5
< false
```



---

참고 : [[리뉴얼] 렛츠기릿 자바스크립트](https://www.inflearn.com/course/%EB%A0%88%EC%B8%A0%EA%B8%B0%EB%A6%BF-%EC%9E%90%EB%B0%94%EC%8A%A4%ED%81%AC%EB%A6%BD%ED%8A%B8/dashboard)

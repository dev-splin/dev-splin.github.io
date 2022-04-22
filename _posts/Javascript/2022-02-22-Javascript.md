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

그래서 **조현영(제로초) 님의 렛츠기릿 자바스크립트**를 보면서 제가 생소한 부분들을 정리해논 글 입니다.

> 단순히 제 주관으로 판단하고 작성하는 글이기 때문에 가이드가 될 수는 없으니 주의해주시면 감사하겠습니다 :star2:





## REPL(Read Eval Print Loop)

기본적으로 Javascript는 컴퓨터가 한줄 씩 읽는 인터프리터 방식의 언어입니다.

이 Javascript는 브라우저의 콘솔에서도 사용할 수 있습니다. 이 때, 브라우저의 콘솔에서 **한 줄씩 입력(Read)**받고 받은 **입력을 평가(Eval)**한 후 **결과를 출력(Print)**합니다. 그 후 다시 프롬프트가 나타나 **새로운 입력을 기다립니다.(Loop)**

이 과정을 `REPL(Read-Eval-Print-Loop)`라고 합니다.





## Number와 parseInt/parseFloat의 차이

아래의 예시와 같이 `Number`는 안에 들어있는 값이 전부 숫자가 아니라면 `Nan(Not a Number)`를 반환하고 `parseInt/parseFloat`은 **Int와 Float으로 인식할 수 있는 위치까지 읽어 해당 타입**으로 반환합니다.

```javascript
Number('234오오오테스트324'); // Nan(Not a Number)

parseInt('234오오오테스트324'); // 234
parseInt('234.23423오오오테스트324'); //234

parseFloat('234오오오테스트324'); // 234
parseFloat('234.23423오오오테스트324'); // 234.23423
```

> 참고로 `Nan`을 `typeof`로 체크해보면 숫자라고 나옵니다.





## prompt()

값을 입력할 수 있는 시스템 창을 출력합니다.





## 빼기(-) 연산자

`1 + '0'` 처럼 더하기 연산자는 숫자가 문자열로 변환 후, 더하기 연산을 진행하는것은 대부분이 다 아는 사실일 겁니다.

하지만, 빼기 연산자는 문자열이 숫자로 바뀐 후 연산합니다. 때문에 **문자열에 숫자가 아닌 다른 문자가 들어올 경우 Nan을 반환**합니다.

즉, 위에서 말한 것 처럼 문자열이 `parseInt`보다는 `Number`가 적용되는 것이라고 볼 수 있습니다.





## NaN끼리 비교하기

값 중에서 NaN은 비교할 때 독특한 성질을 가집니다. 바로 NaN끼리 비교할 때 false 값을 가진다는 것입니다.

```javascript
> NaN == NaN;
< false
```





## 문자열 코드 보기

`charCodeAt()`이라는 메서드를 사용하면 해당 문자가 어떤 코드 번호를 가지고 있는지 알 수 있습니다.'

```javascript
> '&'.charCodeAt();
< 65286
> 'a'.charCodeAt();
< 97
> 'b'.charCodeAt();
< 98
```





## 문자와 숫자 비교

Javascript에서 문자와 숫자를 비교할 때, **문자를 숫자로 변환 후 비교**하게 됩니다. 이 때, 숫자로 변환할 수 없는 문자는 `NaN`이 됩니다.

```javascript
> '1' < 5
< true
> 'abc' < 5
< false
```





## !!연산자

`!`가 true를 false로 false를 true로 바꿔주는 연산자이기 때문에 `!!`를 사용하면 해당 연산자를 붙인 코드가 true인지 false인지를 판단할 수 있다.

```javascript
> !!false // false -> true -> false
< false

> !!'' // false(''은 false) -> true -> false
< false

> !!0 // false(0은 false) -> true -> false
< false

> !!NaN // false(NaN은 false) -> true -> false
< false

> !!undefined // false(undefined은 false) -> true -> false
< false

> !!null // false(null은 false) -> true -> false
< false

> !!document.all // false(document.all은 false, 잘 사용하지 않음) -> true -> false
false
```

위의 경우를 제외하고 대부분이 true이기 때문에 **!!를 이용해서 해당 코드가 있는지 없는지를 판단**할 수 있습니다.





## typeof null

`typeof null`은 원래 null이 나와야 하지만 **결과값은 object**가 나옵니다. 이 현상은 **자바스크립트의 유명한 버그**입니다. 언어가 만들어진 초창기에 object가 되었는데, 기존에 typeof null을 사용하는 모든 곳에 영향을 끼치기 때문에 버그를 수정할 수가 없었다고 합니다.

이 버그를 보고 초반 설계의 중요성에 대해 생각해볼 수 있습니다.





## let let = '1' 선언했을 때 오류 메시지가 다른 이유

`var var = 3;` 처럼 예약어를 변수의 이름으로 사용했을 때, `Unexpected token 'var'`라는 에러 메시지가 나옵니다.

하지만 `let let = '1'`로 선언하게 된다면 `let is disallowed as a lexically bound name`라는 에러메시지가 나옵니다.

그 이유는 ES2015 표준 이전에는 let 선언이 없었으므로 let을 변수 이름으로 자유롭게 사용할 수 있었습니다. 변수 이름 허용을 중지하도록 언어를 변경하면 let이 완벽한 변수 이름이었던 ES2015 이전에 작성된 애플리케이션이 손상될 수 있습니다. (1995년부터 2015년까지 20년 동안!)

let은 ES2015의 새로운 기능이었기 때문에 기존 코드를 끊을 수 없었습니다. 때문에 let을 사용할 때, let을 변수 이름으로 허용하지 않음으로써 잠재적인 구문 혼란의 원인을 피할 수 있었습니다.

> 때문에 ES2015전에 사용했던 var를 **var let = 3; 같은 형식으로 사용하는 것은 올바른 문법**이 됩니다.





## 변수의 값을 변경하면 반환값이 undefined가 아니다?!

```javascript
> let test = 1; // 처음 test를 초기화할 때는 undefined를 반환하지만
< undefined
> test = 2; // 해당 변수의 값을 바꾼다면, 바꾼 값을 반환합니다.
< 2
```





## 빈 값을 넣을 때는 null을 사용하자!

undefined는 빈 값이기 때문에 특정 변수에 빈 값을 넣을 때는 null을 사용함으로써, **의도적으로 null을 넣었다는 것을 표현**해주는 것이 좋습니다.

만약, undefined를 넣었다면, 다른 개발자나 내가 추후에 다시 해당 변수를 사용해야할 때, 기본값이라고 생각하여 상의하지 않고 값을 변경할 수도 있기 때문입니다.









---

참고 : [[리뉴얼] 렛츠기릿 자바스크립트](https://www.inflearn.com/course/%EB%A0%88%EC%B8%A0%EA%B8%B0%EB%A6%BF-%EC%9E%90%EB%B0%94%EC%8A%A4%ED%81%AC%EB%A6%BD%ED%8A%B8/dashboard)

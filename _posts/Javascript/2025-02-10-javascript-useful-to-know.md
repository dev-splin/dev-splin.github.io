---
title: "[Javascript] : 알쓸신잡(알아두면 쓸모있는 신기한 잡아스크립트)"
excerpt_separator: <!--more-->
categories:
  - Javascript
  - 알쓸신잡
tags:
  - Javascript
  - 알쓸신잡
# 이미지 url (썸네일 필요한 경우 추가)
image: /assets/img/thumbnail/javascript-logo.png
# 기본 비노출 상태, 노출하고 싶은 경우 아래 옵션 제거
# published: false
---

다양한 자바스크립트 서적과 글을 보고 다양한 문제를 직면하면서 생소하게 느낀 부분들을 정리해논 글 입니다. (목차를 보고 관심있는 부부만 읽어도 좋습니다.)

> 단순히 제 주관으로 판단하고 작성하는 글이기 때문에 가이드가 될 수는 없으니 주의해주시면 감사하겠습니다 🌟
{: .prompt-warning }

## 1. REPL(Read Eval Print Loop)
기본적으로 <span class="highlighting-underline">Javascript는 컴퓨터가 한줄 씩 읽는 인터프리터 방식의 언어</span>입니다. 이 Javascript는 브라우저의 콘솔에서도 사용할 수 있습니다. 이 때, 브라우저의 콘솔에서 한 줄씩 입력(Read)받고 받은 입력을 평가(Eval)한 후 결과를 출력(Print)합니다. 그 후 다시 프롬프트가 나타나 새로운 입력을 기다립니다.(Loop)

이 과정을 `REPL(Read-Eval-Print-Loop)`라고 합니다.

## 2. Number와 parseInt/parseFloat의 차이
아래의 예시와 같이 <span class="highlighting-underline">Number는 안에 들어있는 값이 전부 숫자가 아니라면 Nan(Not a Number)를 반환</span>하고 <span class="highlighting-underline">parseInt/parseFloat은 Int와 Float으로 인식할 수 있는 위치까지 읽어 해당 타입으로 반환</span>합니다.

```javascript
Number('234오오오테스트324'); // Nan(Not a Number)
​
parseInt('234오오오테스트324'); // 234
parseInt('234.23423오오오테스트324'); //234
​
parseFloat('234오오오테스트324'); // 234
parseFloat('234.23423오오오테스트324'); // 234.23423
```

## 3. 빼기(-) 연산자
<span class="highlighting-underline">1 + '0' 처럼 더하기 연산자는 숫자가 문자열로 변환</span> 후, 더하기 연산을 진행하는것은 대부분이 다 아는 사실일 겁니다. 하지만, <span class="highlighting-underline">빼기 연산자는 문자열이 숫자로 바뀐 후 연산</span> 합니다. 때문에 문자열에 숫자가 아닌 다른 문자가 들어올 경우 Nan을 반환합니다.

즉, 위에서 말한 것 처럼 문자열이 parseInt보다는 Number가 적용되는 것이라고 볼 수 있습니다.

## 4. NaN끼리 비교하기
값 중에서 NaN은 비교할 때 독특한 성질을 가집니다. 바로 <span class="highlighting-underline">NaN끼리 비교할 때 false</span> 값을 가진다는 것입니다.

```javascript
> NaN == NaN;
< false
```

## 5. 문자열 코드 보기
`charCodeAt()`이라는 메서드를 사용하면 해당 문자가 어떤 코드 번호를 가지고 있는지 알 수 있습니다.

```javascript
> '&'.charCodeAt();
< 65286
> 'a'.charCodeAt();
< 97
> 'b'.charCodeAt();
< 98
```

## 6. 문자와 숫자 비교
Javascript에서 <span class="highlighting-underline">문자와 숫자를 비교할 때, 문자를 숫자로 변환 후 비교</span>하게 됩니다. 이 때, 숫자로 변환할 수 없는 문자는 NaN이 됩니다.

```javascript
> '1' < 5
< true
> 'abc' < 5
< false
```
 
## 7. !!연산자
`!`가 true를 false로 false를 true로 바꿔주는 연산자이기 때문에 `!!`를 사용하면 해당 연산자를 붙인 코드가 true인지 false인지를 판단할 수 있습니다.

```javascript
> !!false // false -> true -> false
< false
​
> !!'' // false(''은 false) -> true -> false
< false
​
> !!0 // false(0은 false) -> true -> false
< false
​
> !!NaN // false(NaN은 false) -> true -> false
< false
​
> !!undefined // false(undefined은 false) -> true -> false
< false
​
> !!null // false(null은 false) -> true -> false
< false
​
> !!document.all // false(document.all은 false, 잘 사용하지 않음) -> true -> false
false
```

위의 경우를 제외하고 대부분이 true이기 때문에 !!를 이용해서 해당 코드가 있는지 없는지를 판단할 수 있습니다.

## 8. typeof null

<span class="highlighting-underline">typeof null은 원래 null이 나와야 하지만 결과값은 object가 나옵니다. 이 현상은 자바스크립트의 유명한 버그</span>입니다. 언어가 만들어진 초창기에 object가 되었는데, 기존에 typeof null을 사용하는 모든 곳에 영향을 끼치기 때문에 버그를 수정할 수가 없었다고 합니다.

이 버그를 보고 초반 설계의 중요성에 대해 생각해볼 수 있습니다.

## 9. let let = '1' 선언했을 때 오류 메시지가 다른 이유
`var var = 3;` 처럼 예약어를 변수의 이름으로 사용했을 때, `Unexpected token 'var'`라는 에러 메시지가 나옵니다. 

하지만 `let let = '1'`로 선언하게 된다면 `let is disallowed as a lexically bound name`이라는 에러메시지가 나옵니다. 그 이유는 <span class="highlighting-underline">ES2015 표준 이전에는 let 선언이 없었으므로 let을 변수 이름으로 자유롭게 사용</span>할 수 있었습니다. 변수 이름 허용을 중지하도록 언어를 변경하면 let이 완벽한 변수 이름이었던 ES2015 이전에 작성된 애플리케이션이 손상될 수 있습니다. (1995년부터 2015년까지 20년 동안!)

let은 ES2015의 새로운 기능이었기 때문에 기존 코드를 끊을 수 없었습니다. 때문에 let을 사용할 때, let을 변수 이름으로 허용하지 않음으로써 잠재적인 구문 혼란의 원인을 피할 수 있었습니다. 때문에 ES2015전에 사용했던 var를 `var let = 3;` 같은 형식으로 사용하는 것은 올바른 문법이 됩니다.
 
## 10. 변수의 값을 변경하면 반환값이 undefined가 아니다?!

```javascript
> let test = 1; // 처음 test를 초기화할 때는 undefined를 반환하지만
< undefined
> test = 2; // 해당 변수의 값을 바꾼다면, 바꾼 값을 반환합니다.
< 2
```

## 11. 빈 값을 넣을 때는 null을 사용하자!
`undefined`는 빈 값이기 때문에 <span class="highlighting-underline">특정 변수에 빈 값을 넣을 때는 null을 사용함으로써, 의도적으로 null을 넣었다는 것을 표현해주는 것이 좋습니다.</span>

만약, `undefined`를 넣었다면, 다른 개발자나 내가 추후에 다시 해당 변수를 사용해야할 때, 기본값이라고 생각하여 상의하지 않고 값을 변경할 수도 있기 때문입니다.

## 12. 함수에 이름을 붙이는 방법 3가지

- `함수 선언문(function declaration statement)`: function예약어 다음에 이름을 붙이는 방법 ➡️ `function a() {}`
- `함수 표현식(function expression)`: 변수를 만들어 함수를 넣는 방법 ➡️ `const a = function() {}`
- `화살표 함수(익명 함수)`: 변수를 만들어 함수를 넣는 방법(화살표 함수는 해당 방식만 가능) ➡️ `const a = () => {}`

## 13. 함수의 parameter와 argument
가끔 `parameter`와 `argument`를 헷갈리곤 하는데, 정확히 정의하자면 아래와 같습니다.

- `parameter`: 함수를 선언할 때 받는 매개변수
- `argument`: 함수를 호출할 때 넣어주는 인수
 
만약 함수의 parameter(매개변수)와 argument(인수)가 일치하지 않는다면, 짝지어지지 않은 매개변수는 undefined가 되고 짝지어지지 않은 인수는 무시됩니다.

## 14.  객체 속성의 접근방법

```javascript
const obj = {
    bc : 'hello',
    ab : 'hi',
    dc : 'world',
    // 아래는 특수한 이름을 가진 속성(문자열로 묶어주어야 함)
    '2bc' : 'unique',
    'a b' : 'space',
}
```

위와 같은 객체가 있을 때, 자바스크립트에서 2가지 방법으로 접근할 수 있습니다.

- 온점으로 구분하는 방법 ➡️ `obj.bc`
- 대괄호로 구분하는 방법(이 때, 속성명을 문자열로 묶어주어야 한다는 것을 유의) ➡️ `obj['bc']`

이렇게 객체에 접근하는 방법이 나뉘어져 있는 이유는 '2bc', 'a b'와 같은 특수한 경우 때문입니다. <span class="highlighting-underline">속성명 앞에 숫자가 오거나 공백이 있는 경우는 obj.2bc 처럼 온점으로 구별할 수 없기 때문에 대괄호로 구분하는 방법이 있는 것</span> 입니다.

## 15.  배열과 함수도 객체다?!

자바스크립트에서 배열과 함수도 객체이기 때문에 객체와 같이 속성을 정의할 수 있습니다.

```javascript
function hello() {}
hello.a = 'really?';
​
const array = [];
array.b = 'wow';
​
console.log(hello.a);
console.log(array.b);
```

즉, <span class="highlighting-underline">배열과 함수는 객체를 각각 배열과 함수모양으로 만들어 놓은 것</span>이라고 생각하면 됩니다. 하지만, 목적에 맞게 사용하는 것이 중요합니다.

## 17. 문자열의 split 구분자를 배열에 포함시키는 방법

문자열의 특정 구분자로 문자열을 배열로 분리할 때, split을 사용합니다. 이 때, 정규 표현식을 사용하면 구분자를 배열에 포함하여 분리할 수 있습니다.

```javascript
const content = '해시태그를 뽑아 봅시다. #해시태그 #테스트#붙어있는해시';
const result = content.split(/(#[^ #]+)/); // 배열에 포함시킬 정규표현식을 괄호()로 감싸줍니다.

// ['해시태그를 뽑아 봅시다. ', '#해시태그', ' ', '#테스트', '', '#붙어있는해시', '']
console.log(result);
```

---

**참고 링크**

- [[리뉴얼] 렛츠기릿 자바스크립트](https://www.inflearn.com/course/%EB%A0%88%EC%B8%A0%EA%B8%B0%EB%A6%BF-%EC%9E%90%EB%B0%94%EC%8A%A4%ED%81%AC%EB%A6%BD%ED%8A%B8){:target="_blank"}
- [MDN](https://developer.mozilla.org/ko/){:target='_blank'}
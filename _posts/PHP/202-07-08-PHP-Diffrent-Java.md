---
title: "PHP : PHP의 주의사항"
excerpt_separator: <!--more-->
categories:
  - PHP
tags:
  - PHP
  - "PHP : Precautions"
toc: true
toc_sticky: true
toc_label: 목차
---

# PHP의 주의사항

저는 원래 주로 자바를 사용하였기 때문에 PHP의 기본을 따로 정리하지는 않고, 자바와의 다른 점(주의할 점)만 간단히 정리하겠습니다.





## 자료형



### $x와 PHP 함수의 비교

| Expression              | [gettype()](https://www.php.net/manual/en/function.gettype.php) | [empty()](https://www.php.net/manual/en/function.empty.php) | [is_null()](https://www.php.net/manual/en/function.is-null.php) | [isset()](https://www.php.net/manual/en/function.isset.php) | bool : `if($x)` |
| :---------------------- | :----------------------------------------------------------- | :---------------------------------------------------------- | :----------------------------------------------------------- | :---------------------------------------------------------- | :-------------- |
| `$x = "";`              | string                                                       | **`true`**                                                  | **`false`**                                                  | **`true`**                                                  | **`false`**     |
| `$x = null;`            | NULL                                                         | **`true`**                                                  | **`true`**                                                   | **`false`**                                                 | **`false`**     |
| `var $x;`               | NULL                                                         | **`true`**                                                  | **`true`**                                                   | **`false`**                                                 | **`false`**     |
| $x is undefined         | NULL                                                         | **`true`**                                                  | **`true`**                                                   | **`false`**                                                 | **`false`**     |
| `$x = array();`         | array                                                        | **`true`**                                                  | **`false`**                                                  | **`true`**                                                  | **`false`**     |
| `$x = array('a', 'b');` | array                                                        | **`false`**                                                 | **`false`**                                                  | **`true`**                                                  | **`true`**      |
| `$x = false;`           | bool                                                         | **`true`**                                                  | **`false`**                                                  | **`true`**                                                  | **`false`**     |
| `$x = true;`            | bool                                                         | **`false`**                                                 | **`false`**                                                  | **`true`**                                                  | **`true`**      |
| `$x = 1;`               | int                                                          | **`false`**                                                 | **`false`**                                                  | **`true`**                                                  | **`true`**      |
| `$x = 42;`              | int                                                          | **`false`**                                                 | **`false`**                                                  | **`true`**                                                  | **`true`**      |
| `$x = 0;`               | int                                                          | **`true`**                                                  | **`false`**                                                  | **`true`**                                                  | **`false`**     |
| `$x = -1;`              | int                                                          | **`false`**                                                 | **`false`**                                                  | **`true`**                                                  | **`true`**      |
| `$x = "1";`             | string                                                       | **`false`**                                                 | **`false`**                                                  | **`true`**                                                  | **`true`**      |
| `$x = "0";`             | string                                                       | **`true`**                                                  | **`false`**                                                  | **`true`**                                                  | **`false`**     |
| `$x = "-1";`            | string                                                       | **`false`**                                                 | **`false`**                                                  | **`true`**                                                  | **`true`**      |
| `$x = "php";`           | string                                                       | **`false`**                                                 | **`false`**                                                  | **`true`**                                                  | **`true`**      |
| `$x = "true";`          | string                                                       | **`false`**                                                 | **`false`**                                                  | **`true`**                                                  | **`true`**      |
| `$x = "false";`         | string                                                       | **`false`**                                                 | **`false`**                                                  | **`true`**                                                  | **`true`**      |



### ==를 이용한 느슨한 비교

|             | **`true`**  | **`false`** | `1`         | `0`         | `-1`        | `"1"`       | `"0"`       | `"-1"`      | **`null`**  | `array()`   | `"php"`     | `""`        |
| :---------- | :---------- | :---------- | :---------- | :---------- | :---------- | :---------- | :---------- | :---------- | :---------- | :---------- | :---------- | :---------- |
| **`true`**  | **`true`**  | **`false`** | **`true`**  | **`false`** | **`true`**  | **`true`**  | **`false`** | **`true`**  | **`false`** | **`false`** | **`true`**  | **`false`** |
| **`false`** | **`false`** | **`true`**  | **`false`** | **`true`**  | **`false`** | **`false`** | **`true`**  | **`false`** | **`true`**  | **`true`**  | **`false`** | **`true`**  |
| `1`         | **`true`**  | **`false`** | **`true`**  | **`false`** | **`false`** | **`true`**  | **`false`** | **`false`** | **`false`** | **`false`** | **`false`** | **`false`** |
| `0`         | **`false`** | **`true`**  | **`false`** | **`true`**  | **`false`** | **`false`** | **`true`**  | **`false`** | **`true`**  | **`false`** | **`true`**  | **`true`**  |
| `-1`        | **`true`**  | **`false`** | **`false`** | **`false`** | **`true`**  | **`false`** | **`false`** | **`true`**  | **`false`** | **`false`** | **`false`** | **`false`** |
| `"1"`       | **`true`**  | **`false`** | **`true`**  | **`false`** | **`false`** | **`true`**  | **`false`** | **`false`** | **`false`** | **`false`** | **`false`** | **`false`** |
| `"0"`       | **`false`** | **`true`**  | **`false`** | **`true`**  | **`false`** | **`false`** | **`true`**  | **`false`** | **`false`** | **`false`** | **`false`** | **`false`** |
| `"-1"`      | **`true`**  | **`false`** | **`false`** | **`false`** | **`true`**  | **`false`** | **`false`** | **`true`**  | **`false`** | **`false`** | **`false`** | **`false`** |
| **`null`**  | **`false`** | **`true`**  | **`false`** | **`true`**  | **`false`** | **`false`** | **`false`** | **`false`** | **`true`**  | **`true`**  | **`false`** | **`true`**  |
| `array()`   | **`false`** | **`true`**  | **`false`** | **`false`** | **`false`** | **`false`** | **`false`** | **`false`** | **`true`**  | **`true`**  | **`false`** | **`false`** |
| `"php"`     | **`true`**  | **`false`** | **`false`** | **`true`**  | **`false`** | **`false`** | **`false`** | **`false`** | **`false`** | **`false`** | **`true`**  | **`false`** |
| `""`        | **`false`** | **`true`**  | **`false`** | **`true`**  | **`false`** | **`false`** | **`false`** | **`false`** | **`true`**  | **`false`** | **`false`** | **`true`**  |



### ===를 이용한 엄격한 비교

|             | **`true`**  | **`false`** | `1`         | `0`         | `-1`        | `"1"`       | `"0"`       | `"-1"`      | **`null`**  | `array()`   | `"php"`     | `""`        |
| :---------- | :---------- | :---------- | :---------- | :---------- | :---------- | :---------- | :---------- | :---------- | :---------- | :---------- | :---------- | :---------- |
| **`true`**  | **`true`**  | **`false`** | **`false`** | **`false`** | **`false`** | **`false`** | **`false`** | **`false`** | **`false`** | **`false`** | **`false`** | **`false`** |
| **`false`** | **`false`** | **`true`**  | **`false`** | **`false`** | **`false`** | **`false`** | **`false`** | **`false`** | **`false`** | **`false`** | **`false`** | **`false`** |
| `1`         | **`false`** | **`false`** | **`true`**  | **`false`** | **`false`** | **`false`** | **`false`** | **`false`** | **`false`** | **`false`** | **`false`** | **`false`** |
| `0`         | **`false`** | **`false`** | **`false`** | **`true`**  | **`false`** | **`false`** | **`false`** | **`false`** | **`false`** | **`false`** | **`false`** | **`false`** |
| `-1`        | **`false`** | **`false`** | **`false`** | **`false`** | **`true`**  | **`false`** | **`false`** | **`false`** | **`false`** | **`false`** | **`false`** | **`false`** |
| `"1"`       | **`false`** | **`false`** | **`false`** | **`false`** | **`false`** | **`true`**  | **`false`** | **`false`** | **`false`** | **`false`** | **`false`** | **`false`** |
| `"0"`       | **`false`** | **`false`** | **`false`** | **`false`** | **`false`** | **`false`** | **`true`**  | **`false`** | **`false`** | **`false`** | **`false`** | **`false`** |
| `"-1"`      | **`false`** | **`false`** | **`false`** | **`false`** | **`false`** | **`false`** | **`false`** | **`true`**  | **`false`** | **`false`** | **`false`** | **`false`** |
| **`null`**  | **`false`** | **`false`** | **`false`** | **`false`** | **`false`** | **`false`** | **`false`** | **`false`** | **`true`**  | **`false`** | **`false`** | **`false`** |
| `array()`   | **`false`** | **`false`** | **`false`** | **`false`** | **`false`** | **`false`** | **`false`** | **`false`** | **`false`** | **`true`**  | **`false`** | **`false`** |
| `"php"`     | **`false`** | **`false`** | **`false`** | **`false`** | **`false`** | **`false`** | **`false`** | **`false`** | **`false`** | **`false`** | **`true`**  | **`false`** |
| `""`        | **`false`** | **`false`** | **`false`** | **`false`** | **`false`** | **`false`** | **`false`** | **`false`** | **`false`** | **`false`** | **`false`** | **`true`**  |



### Array(배열)

```php
<?php

$texts1 = Array("text1-1", "text1-2");	// 배열 선언 방법 1
$texts2 = ["text2-1", "text2-2"];	// 배열 선언 방법 2

var_dump($texts1);

echo $texts1[0];
echo $texts1[1];

echo $texts2[0];
echo $texts2[1];

?>
```



#### Associative Array (배열)

Java의 Map과 같은 느낌이라고 보면 될 것 같습니다.

Java는 배열이 고정적인 크기를 가지고 한 번 크기를 할당한 후, 크기를 바꾸려면 배열을 재선언 해야했지만, **PHP는 배열을 Java의 List 처럼 동적으로 사용**할 수 있는 것 같습니다.

```php
<?php

$texts1 = array("first"=>1, "second"=>2, "third"=>3); // 첫번 째 선언방법

$texts2 = [];	// Array()와 같은 의미 
$texts2["first"] = 1;
$texts2["second"] = 2;
$texts2["third"] = 3;

var_dump($texts1);
var_dump($texts2);

foreach ($texts1 as $key => $value)	// 이와 같은 방식의 foreach를 이용해 배열의 key, value를 가져올 수 있습니다.
    echo "key : {$key} value : {$value}<br />";

?>
```





## 연산자

- and : and 연산자 (자바에서 &&)
- or : or 연산자 (자바에서 ||)





## 메서드

### 메서드 선언 방법

```php
<?php
$test = 1;
function numbering($arg1, $arg2=100) {	// arg2는 Default 매개변수
    global $test;	// 함수 밖에 있는 변수를 global을 통해 가져 올 수 있지만 권장되지 않음
    $test = 2;
    return $arg1 + $arg2;
}
echo numbering(2, 4);
echo numbering(2);
echo "test = ".$test;	// 함수 안에서 전역변수 값을 바꿔주었기 때문에 1이 아니라 2출력
?>
```



### 메서드 소개



#### 자료형 관련

`var_dump(arg)` : arg에 해당하는 타입을 보여 줍니다. (배열은 안의 내용까지)

`ucfirst(arg)` : arg의 첫 번째 글자를 대문자로 만들어 줍니다.



#### 배열 관련

`array_push(arg1, arg2)` : arg1(배열)의 마지막에 arg2(값)을 추가합니다.

`array_pop(arg1, arg2)` : arg1(배열)의 마지막 값을 삭제하면서 삭제한 값을 반환합니다.

`array_unshift(arg1, arg2)` : arg1(배열)의 맨 앞에 arg2(값)을 추가합니다.

`array_shift(arg1, arg2)` : arg1(배열)의 맨 앞 값을 삭제하면서 삭제한 값을 반환합니다.

`sort(arg)` : arg(배열)을 정렬합니다. (기본 오름차순)

`rsort(arg)` : arg(배열)을 정렬합니다. (기본 내림차순)

[더 다양한 배열관련 함수들](https://www.php.net/manual/en/ref.array.php)





## include와 namespace

### include VS require

Java의 import와 같이 외부 파일을 로드할 수 있는데, 이 때, 로드 실패 시, include는 warning을, require는 fetal을 발생시킵니다. 즉, require가 더 강제력이 강한 것입니다.

inlcude_once와 require_once가 존재하는데, once는 중복된 로드를 방지합니다.



### namespace

파일의 수가 많아지게 되면, 각 파일의 메서드나 상수, 클래스명이 같아지는 경우가 발생합니다.
이 때, namespace를 사용하면 위와 같은 상황을 구분할 수 있습니다.

```php
// greeting_en_ns.php
<?php
namespace language\ko;	// namespace
function welcome() {
    return '안녕하세요';
}

namespace language\ko_short;	// namespace는 하나의 파일에서 여러 개를 만들 수 있습니다.
function welcome() {
    return '안녕';
}
?>

// greeting_ko_ns.php
<?php
namespace language\en;	// namespace
function welcome() {
    return 'Hello World';
}
?>

// test.php
<?php
require_once 'greeting_en_ns.php';
require_once 'greeting_ko_ns.php';

// namespace를 이용해 같은 메서드를 구분해서 호출할 수 있습니다.
echo language\ko\welcome();	// 안녕하세요
echo language\ko_short\welcome();	// 안녕
echo language\en\welcome();	// Hellow World
?>
```





## Composer

Composer는 PHP의 의존성 관리도구입니다. 필요한 확장 기능을 쉽게 설치해주는 기능도 제공하지만, 프로젝트에서 필요한 확장 기능을 통합해서 관리해주는 도구입니다.

**설치 참고 링크**

[생활코딩 Composer](https://opentutorials.org/course/62/5221)



### Composer 사용하기

Composer를 이용해서 라이브러리를 가져오려면 필요한 라이브러리를 가져와야 하는데, Composer는 [Packagist](https://packagist.org/) 사이트에서 다양한 라이브러리를 찾을 수 있습니다.

Packagist에서 필요한 라이브러리를 찾았다면, 프로젝트의 최상위 폴더에 `composer.json`이라는 파일을 만들어야 합니다.



---

참고 : 

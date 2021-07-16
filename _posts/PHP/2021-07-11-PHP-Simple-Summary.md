---
title: "PHP : PHP 간단 정리"
excerpt_separator: <!--more-->
categories:
  - PHP
tags:
  - PHP
toc: true
toc_sticky: true
toc_label: 목차
---

# PHP 간단 정리

저는 원래 주로 자바를 사용하였기 때문에 PHP의 기본을 따로 정리하지는 않고, 자바와의 다른 점(주의할 점)만 간단히 정리하겠습니다. (공통 되거나 제가 아는 부분은 생략할 수도 있습니다.)





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



### 문자열 처리

대부분 Java와 비슷하지만 다른 점 몇가지만 기재하겠습니다.



#### 큰 따옴표와 작은 따옴표

큰 따옴표는 이스케이프를 적용시키고, 작은 따옴표는 문자 그대로 출력하게 됩니다. 

큰 따옴표는 해석의 과정을 거치기 때문에 작은 따옴표보다 속도가 느립니다.

```php
<?php
echo "hello world\n";	// hello world
echo 'hello world\n';	// hello world\n
?>
```



#### 문자열 안에서 변수 사용하기

문자열 안에서 변수를 사용하려면 쌍따옴표 안에서 `중괄호({,})`를 사용하거나 `마침표(.)`를 이용해 문자열과 더 해서 사용합니다.

```php
<?php
$a = array('hello', 'world');
echo "생활코딩의 공식인사는 {$a[0]} {$a[1]}입니다";
echo '생활코딩의 공식인사는 '.$a[0].' '.$a[1].'입니다';
?>
```



#### 문자열 더 하기

문자와 문자를 더 할 때는 `마침표(.)`를 사용합니다.

```php
<?php
$a = "생활";
$b = "코딩";
echo $a.$b;
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

PHP에서 사용하는 다양한 함수들을 알아보겠습니다.

**참고 링크**

[PHP 공식 문서](https://www.php.net/manual/en/)



#### 자료형 관련

- `var_dump(arg)` : arg에 해당하는 타입을 보여 줍니다. (배열은 안의 내용까지)

- `ucfirst(arg)` : arg의 첫 번째 글자를 대문자로 만들어 줍니다.



#### 배열 관련

- `array_push(arg1, arg2)` : arg1(배열)의 마지막에 arg2(값)을 추가합니다.

- `array_pop(arg1, arg2)` : arg1(배열)의 마지막 값을 삭제하면서 삭제한 값을 반환합니다.

- `array_unshift(arg1, arg2)` : arg1(배열)의 맨 앞에 arg2(값)을 추가합니다.

- `array_shift(arg1, arg2)` : arg1(배열)의 맨 앞 값을 삭제하면서 삭제한 값을 반환합니다.

- `sort(arg)` : arg(배열)을 정렬합니다. (기본 오름차순)

- `rsort(arg)` : arg(배열)을 정렬합니다. (기본 내림차순)

[더 다양한 배열 관련 함수들](https://www.php.net/manual/en/ref.array.php)



#### 문자열 관련

- [strpos](http://docs.php.net/manual/kr/function.strpos.php)
- [implode](http://docs.php.net/manual/kr/function.implode.php)
- [explode](http://docs.php.net/manual/kr/function.explode.php)
- [htmlentities](http://docs.php.net/manual/kr/function.htmlentities.php)
- `str_replace(arg1, arg2, arg3)` : arg3(문자열)에서 arg1(찾을 문자열)을 arg2(바꿀 문자열)로 바꿉니다.



##### 정규 표현식 관련

- `preg_match(arg1, arg2, arg3)` : arg2(문자열)에서 arg1(정규 표현식)에 해당되는 부분이 있는지 검사합니다.
  - arg3에는 변수를 넣을 수 있는데, 이 변수는 따로 선언하지 않아도 사용할 수 있습니다.
- `preg_replace(arg1, arg2, arg3)` : arg3(문자열) 중에서 arg1(정규 표현식)에 해당하는 부분을 arg2(바꿀 문자열)로 교체합니다.



#### 파일 관련

- `copy(arg1, arg2)` : arg1(파일 이름)의 파일을 arg2(파일 이름)로 복사합니다.

- `unlink(arg)` : arg(파일 이름)에 해당하는 이름의 파일을 삭제합니다.

- `file_get_contents(arg)` : arg(파일 이름) 파일을 읽어서 리턴합니다. (웹 사이트도 읽을 수 있습니다.)

- `file_put_contents(arg1, arg2)` : arg1(파일 이름)의 파일에 arg2(내용)을 작성합니다. (기존의 내용이 있다면 덮어 쓰고, 없다면 파일을 만든 후 작성합니다.)
  - 간편 하지만, 파일을 세밀하게 제어하기엔 한계가 있기 때문에 fopen, fwrite등의 함수를 이용하는 것이 더 좋습니다.
- `is_readable(arg)` : arg(파일 이름) 파일이 읽기 가능한 상태인지 확인합니다.
- `is_writable(arg)` : arg(파일 이름) 파일이 쓰기 가능한 상태인지 확인합니다.
- `file_exists(arg)` : arg(파일 이름) 파일이 존재하는지 확인합니다.



##### 파일 업로드 관련

- `move_uploaded_file(arg1, arg2)` : arg1(임시 이름)에 해당하는 경로를 arg2(이동할 경로)에 해당하는 경로로 이동시킵니다.
  - PHP 내부적으로 업로드할 파일을 체크하는 로직이 들어있습니다.
- `basename(arg)` : arg(파일 경로)에서 역슬래시(`\`)와 디렉토리 명 등을 제거하여 파일 경로 중 제일 마지막의 경로만 반환합니다.

[더 다양한 파일 관련 함수들](https://www.php.net/manual/en/ref.filesystem.php)



#### 디렉토리(폴더) 관련

- `getcwd()` : 현재 디렉토리의 위치를 반환합니다.
- `chdir(arg)` : 현재 디렉토리에서 arg에 해당하는 명령어로 디렉토리 제어합니다.
- `scandir(arg1, arg2)` : arg1 위치의 디렉토리 안의 파일 이름을 반환합니다. Dfault는 오름차순(arg2가 없을 때), arg2가 1이면 내림차순
- `mkdir(arg1, arg2, arg3)` : arg1에 해당하는 디렉토리를 arg2의 권한으로 만드는데, arg3을 true를 지정하면 arg1에서 주어진 경로가 여러개의 디렉토리로 이루어져 있을 때 해당 디렉토리를 한번에 생성하게 됩니다.



#### 설정 관련

- `ini_set(arg1, arg2)` : 현재 코드에서 arg1(설정 이름)에 해당하는 설정을 arg2(on(1) or off(0))로 설정합니다.



#### 세션,쿠키 관련

- `setCookie(arg1, arg2, arg3)` : arg1(키로 사용)을 키로, arg2(값으로 사용)을 값으로 하는 쿠키를 생성합니다. arg3을 이용하면 쿠키의 유효시간을 설정할 수 있습니다.
- `session_save_path(arg)` : arg에 해당하는 경로에 세션을 저장합니다.
- `session_start()` : 세션을 사용하려면 반드시 스크립트의 최상단에 위치해야 합니다.
- `session_destroy()` : 세션 정보를 삭제합니다.



#### 시스템 관련

- `die(arg)` : arg(문자열)을 출력하고 시스템을 종료합니다.



#### 클래스 로딩 관련

- `spl_autoload_register(arg)` : 클래스 로드에 실패 시 arg(함수 이름) 함수를 실행합니다.





## 객체

PHP 객체는 Java와 비슷하지만 다른 점이 몇가지 있습니다.



### 멤버 변수

PHP는 멤버변수를 따로 만들어주지 않아도 아래와 같이 메서드 내부에서 멤버변수를 사용할 수 있습니다.

>  가독성이 좋지 않고 객체 안의 어떤 값이 있는지 알아보기 힘들기 때문에 private, public 같은 접근 제어자를 이용해 멤버 변수 선언 후 사용하는 것이 좋아보입니다.

```php
<?php
class MyFileObject{
  function isFile(){
    return is_file($this->filename);	// 멤버 변수를 선언해 준 적이 없는데, 멤버변수처럼 사용하고 있습니다.
  }
}
$file = new MyFileObject();
$file->filename = 'data.txt';
var_dump($file->isFile());
var_dump($file->filename);
 
$file2 = new MyFileObject();
$file2->filename = 'data2.txt';
var_dump($file2->isFile());
var_dump($file2->filename);
?>
```



### 생성자

PHP에서는 생성자를 __construct를 사용해서 만듭니다.

```php
<?php
class MyFileObject{
  function __construct($fname){
    $this->filename = $fname;
  }
  function isFile(){
    return is_file($this->filename);
  }
}
$file = new MyFileObject('data.txt');
// $file->filename = 'data.txt';
var_dump($file->isFile());
var_dump($file->filename);
?>
```



### Static 변수

PHP에서 멤버 변수, 메서드는 `->`를 사용하지만, static 변수, 메서드는 아래와 같이  `::`을 사용합니다.

```php
<?php
class Person{
  private static $count = 0;	// static 변수
  private $name;
  function __construct($name){
    $this->name = $name;
    self::$count = self::$count + 1;	// 객체 내에서 static 변수 사용 방법
  }
  function enter(){
    echo "<h1>Enter ".$this->name." ".self::$count."th</h1>";
  }
  static function getCount(){
    return self::$count;
  }
}
$p1 = new Person('egoing');
$p1->enter();
$p2 = new Person('leezche');
$p2->enter();
$p3 = new Person('duru');
$p3->enter();
$p4 = new Person('taiho');
$p4->enter();
echo Person::getCount();	// static 메서드 사용 방법
?>
```



### 클래스 로딩

PHP에서는 클래스를 찾지 못할 때, `spl_autoload_register(arg)` 함수를 호출하도록 되어있습니다. 이 때, arg에 해당하는 함수에 파라미터가 있다면 , 파라미터의 찾지 못한 클래스의 이름을 넣어줍니다. (함수형 프로그래밍을 지원한다는 의미)

> namespace가 있다면 , namespace도 같이 들어가기 때문에 주의해야 합니다.

즉 클래스를 찾지 못했을 때, `spl_autoload_register(arg)` 함수를 이용해 함수가 포함된 파일을 동적으로 include하거나 require 할 수 있다는 것입니다.

```php
// Hi.php
<?php
class Hi{
  function __construct(){
    echo 'hi';
  }
}
?>
```

```php
<?php
finction autoloader($path) {
    $filePath = $path.'.php';
    require_once $filePath;
}
spl_autoload_register('autoloader');
new Hi();
?>
```



### 상속

PHP는 extends를 이용해서 상속할 수 있고, 부모의 메서드를 `parent::메서드 이름`를 이용해서 호출할 수 있고, 부모의 속성은 `$this->속성 이름`으로 가져올 수 있습니다.

```php
<?php
class ParentClass{
    public $value = 'public value';

    function callMethod($param){
        echo "<h1>Parent {$param}</h1>";
    }
}
class ChildClass extends ParentClass{
    function callMethod($param){
        echo $this->value;
        parent::callMethod($param);
        echo "<h1>Child {$param}</h1>";
    }
}
$obj = new ChildClass();
$obj->callMethod('method');
?>
```



## include와 namespace

### include VS require

Java의 import와 같이 외부 파일을 로드할 수 있는데, 이 때, 로드 실패 시, include는 warning을, require는 fetal을 발생시킵니다. 즉, require가 더 강제력이 강한 것입니다.

inlcude_once와 require_once가 존재하는데, once는 중복된 로드를 방지합니다.



### namespace

파일의 수가 많아지게 되면, 각 파일의 메서드나 상수, 클래스명이 같아지는 경우가 발생합니다.
이 때, namespace를 사용하면 위와 같은 상황을 구분할 수 있습니다. (Java의 Package와 비슷하다고 볼 수 있습니다.)

```php
// greeting_ko_ns.php
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

// greeting_en_ns.php
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

// use와 as를 사용해 축약해서 사용할 수 있습니다.
use language\ko as k;
use language\ko_short as ks;
use language\en as e;

echo k\welcome();
echo ks\welcome();
echo e\Welcome();
?>
```





## Composer

Composer는 PHP의 의존성 관리도구입니다. 필요한 확장 기능을 쉽게 설치해주는 기능도 제공하지만, 프로젝트에서 필요한 확장 기능을 통합해서 관리해주는 도구입니다.

**설치 참고 링크**

[생활코딩 Composer](https://opentutorials.org/course/62/5221)



### Composer로 라이브러리 설치하기

Composer를 이용해서 라이브러리를 가져오려면 필요한 라이브러리를 가져와야 하는데, Composer는 [Packagist](https://packagist.org/) 사이트에서 다양한 라이브러리를 찾을 수 있습니다. 필요한 라이브러리를 찾았다면, 아래와 같은 방법을 이용하면 됩니다.

1. 프로젝트의 최상위 폴더에 `composer.json`이라는 파일을 만듭니다.
2. 아래와 같이 Json방식으로 `"require" : { }` 안에  `"vendor(라이브러리 제공자 이름)/라이브러리이름" : "버전"` 방식으로 라이브러리를 등록합니다.

```json
{
  "require": {
    "erusev/parsedown": "1.7.4"
  }
}
```

3. cmd를 실행한 후, 해당 프로젝트의 최상위 폴더로 이동해 `composer install` 명령어로 라이브러리를 설치합니다. 
4. 설치가 완료되면, vendor라는 폴더가 생기고 라이브러리가 설치된 것을 볼 수 있습니다.

> composer.lock에는 composer install 명령어를 사용해 라이브러리들을 설치할 당시의 버전으로 저정되기 때문에 만약 버전업을 해애한다면 composer update를 이용해 lock을 update 시켜주어야 합니다.



### Composer 사용하기

Composer를 이용해 설치한 라이브러리들은 라이브러리 이름을 정확히 명시할 필요 없이 `require 'vendor/autoload.php';`를 넣어주면 전부 자동으로 로딩됩니다.

```php
<?php
require 'vendor/autoload.php';	// 라이브러리 전부를 로딩
$Parsedown = new Parsedown();	// 위에서 설치한 라이브러리 객체
echo $Parsedown->text('Hello _Parsedown_!');	// 객체의 메서드 사용
?>
```





## 파일 업로드

PHP는 클라이언트에서 전달한 파일을 아래와 같이 `$_FILES`에 배열 형태로 넣어 놓습니다.

```php
array(2) {
  ["userfile"]=>
  array(5) {
    ["name"]=>
    string(20) "blog_teaser_mini1.jpg"
    ["type"]=>
    string(10) "image/jpeg"
    ["tmp_name"]=>
    string(48) "C:\Bitnami\wampstack-8.0.7-0\php\tmp\php2064.tmp"
    ["error"]=>
    int(0)
    ["size"]=>
    int(854)
  }
  ["userfile2"]=>
  array(5) {
    ["name"]=>
    string(20) "blog_teaser_mini2.jpg"
    ["type"]=>
    string(10) "image/jpeg"
    ["tmp_name"]=>
    string(48) "C:\Bitnami\wampstack-8.0.7-0\php\tmp\php2065.tmp"
    ["error"]=>
    int(0)
    ["size"]=>
    int(854)
  }
}

```

- **name** : 파일의 이름을 나타냅니다.
- **type** : 파일의 형식을 나타냅니다.
- **tmp_name** : 임시 이름을 나타냅니다.
  - tmp_name은 이 임시 디렉토리의 경로를 나타내는 것입니다.
  - 클라이언트에서 파일을 전송하게 되면 그 파일은 자동으로 임시 디렉토리에 무조건 들어가게 됩니다.
  - 이 임시 디렉토리를 알고 있어야지만 `move_uploaded_file()` 메서드를 이용해 원하는 디렉토리로 이동 시킬 수 있습니다.
- **error** : 오류가 발생할 시, 값이 존재하게 됩니다.
- **size** : 파일의 크기를 나타냅니다.

> 허용된 파일의 범위를 넘어서면 값이 존재하지 않을 수 있습니다.





## 정규 표현식

**PHP의 정규표현식은 구분자(delimiters)로 시작해서 구분자로 끝**을 내야합니다. 구분자는 보통 슬래쉬(`/`)를 사용하지만 꼭 그래야 하는 것은 아니고 해쉬(`#`)와 같이 알파벳과 백슬래쉬 그리고 공백이 아닌 문자를 사용하면 됩니다. 아래의 그림에서 강조표시한 부분은 모두 구분자가 될 수 있는 문자들입니다. ([참고 링크](http://www.php.net/manual/en/regexp.reference.delimiters.php))



![img](https://s3.ap-northeast-2.amazonaws.com/opentutorials-user-file/module/6/1790.gif)



닫히는 구분자 뒤에는 `Pattern Modifier`라는 옵션(Internal Option)이 위치할 수 있는데, 옵션에 따라서 정규표현식이 다르게 동작합니다. 

아래는 **대소문자를 구분하지 않는 `i`** 와 **줄바꿈 문자에 따라서 텍스트의 행을 구분하도록 하는 `m`**을 적용한 예입니다. ([참고 링크](http://www.php.net/manual/en/reference.pcre.pattern.modifiers.php))



### 캡처링(capturing)

PHP는 `preg_match()`의 3번째 파라미터인 변수에 정규 표현식을 적용한 결과를 넣게 되는데, 이 때, 괄호(`(,)`)를 이용해서 그룹핑하게 되면 그룹화된 부분도 3번째 파라미터에 들어가게 됩니다.

> 괄호 안에 (?: )와 같이  `?:`를 붙여 사용하게 되면 그룹화된 부분이 3번째 파라미터에 들어가지 않습니다. 

```php
<?php
$subject = 'coding everybody http://opentutorials.org egoing@egoing.com 010-0000-0000';
preg_match('~(http://\w+\.\w+)\s(\w+@\w+\.\w+)~', $subject, $match);
var_dump($match);
echo "homepage:".$match[1];	// 1번 째로 그룹화된 부분
echo "<br />";
echo "email:".$match[2];	// 2번 째로 그룹화된 부분
?>
```



#### 그룹 이름 설정

아래 처럼 `(?P<name>)` 나 `(?<name>)`를 사용해 연관 배열의 키로 그룹 이름을 설정해 줄 수 있습니다.

> PHP 5.2.2 이상의 버전에서 부터 `(?<name>)`를 사용할 수 있지만, 이전 버전과의 호환성을 위해 `(?P<name>)` 방식을 권장한다고 합니다.

```php
<?php
$str = 'foobar: 2008';
 
preg_match('/(?P<name>\w+): (?P<digit>\d+)/', $str, $matches);
 
print_r($matches);
 
?>
```



#### 치환

`preg_replace(arg1, arg2, arg3)`을 이용해 문자열에서 정규 표현식에 해당하는 부분을 교체할 수 있습니다. 이 때, 그룹화 한 부분을 `${1}`나 `$3` 처럼(`\3`도 가능) 인덱스(1부터 시작)로 사용할 수 있습니다. 

```php
<?php
$string = 'April 15, 2003';
$pattern = '/(\w+) (\d+), (\d+)/i';
$replacement = '${1}1,$3';	// ${1}은 첫 번째 그룹화한 부분을 의미
echo preg_replace($pattern, $replacement, $string);
?>
```



##### 배열 파라미터

`preg_replace(arg1, arg2, arg3)`에 아래와 배열을 파라미터로 넣을 수 있습니다.

patterns의 인덱스와 같은 replacements 값으로 교체됩니다. 

ex) patterns[0]에 해당하는 문자열은 replacements[0]으로 교체

```php
<?php
$string = 'The quick brown fox jumped over the lazy dog.';
$patterns = array();
$patterns[0] = '/quick/';
$patterns[1] = '/brown/';
$patterns[2] = '/fox/';
$replacements = array();
$replacements[2] = 'bear';
$replacements[1] = 'black';
$replacements[0] = 'slow';
echo preg_replace($patterns, $replacements, $string);
?>
```





## 데이터베이스

PHP에서는 PDO를 이용해서 데이터베이스를 이용할 수 있습니다. `PDO(PHP Data Objects)`란 여러가지 데이터베이스를 제어하는 방법을 표준화시킨 것입니다.

데이터베이스는 다양한 종류가 있기 때문에 종류에 따라서 서로 다른 드라이브를 사용해 왔습니다. 드라이브의 종류에 따라서 데이터베이스를 제어하기 위한 API가 달랐는데, PDO를 사용하면 동일한 방법으로 데이터베이스를 제어할 수 있습니다.

PHP의 mysql client library(mysql_connect와 같은 함수)는 PHP 5.5 버전부터 지원되지 않기 때문에 더 이상 새로운 에플리케이션의 경우 mysql client library를 사용해서는 안됩니다. 하지만 사용하는 환경이 PDO나 mysqli를 지원하지 않거나 기존의 코드가 mysql client library라면 이것의 사용법을 알아야 합니다. 이런 경우 [데이터베이스와 PHP 수업](http://opentutorials.org/course/62/5174)을 참고합니다.

또한 PHP에 대한 올바른 사용법을 전파하고 있는 PHP Right Way에서는 PDO를 사용할 것을 권하고 있습니다. 이에 대한 내용은 [PHP Right Way](http://wafe.github.io/php-the-right-way/#데이터베이스)의 데이터베이스 편을 참고하면 됩니다.



### 연결 방법

PHP에서 데이터베이스 연결 방법은 주석으로 차근차근 설명하겠습니다.

```php
<?php
// db를 연결합니다.
// PDO::MYSQL_ATTR_INIT_COMMAND는 MySQL 서버에 연결할 때 실행할 명령입니다. 재접속 시 자동으로 재실행됩니다.
$dbh = new PDO('mysql:host=localhost;dbname=opentutorials', 'root', '111111', array(PDO::MYSQL_ATTR_INIT_COMMAND => "SET NAMES utf8"));
// form에서 보내는 mode 값에 따라 다르게 처리합니다.
switch($_GET['mode']) {
    case 'insert':
    	// 데이터베이스를 연결한 dbh에서 Query를 준비합니다.
        $stmt = $dbh->prepare("INSERT INTO topic (title, description, created) VALUES (:title, :description, now())");
        // Query의 파라미터에 값을 매핑합니다.
        $stmt->bindParam(':title',$title);
        $stmt->bindParam(':description',$description);
 		
 		// 파라미터에 매핑할 변수에 form에서 받아온 값을 넣어줍니다.
        $title = $_POST['title'];
        $description = $_POST['description'];
        
        // Query를 실행합니다.
        $stmt->execute();
        // list.php로 Redirect합니다.
        header("Location: list.php"); 
        break;
    case 'delete':
        $stmt = $dbh->prepare('DELETE FROM topic WHERE id = :id');
        $stmt->bindParam(':id', $id);
 
        $id = $_POST['id'];
        $stmt->execute();
        header("Location: list.php"); 
        break;
    case 'modify':
        $stmt = $dbh->prepare('UPDATE topic SET title = :title, description = :description WHERE id = :id');
        $stmt->bindParam(':title', $title);
        $stmt->bindParam(':description', $description);
        $stmt->bindParam(':id', $id);
 
        $title = $_POST['title'];
        $description = $_POST['description'];
        $id = $_POST['id'];
        $stmt->execute();
        // list.php로 Redirect합니다.(파라미터와 같이)
        header("Location: list.php?id={$_POST['id']}");
        break;
}
?>
```





## 쿠키(Cookie)와 세션(Session)

**HTTP프로토콜은 상태 유지가 안되는 프로토콜**입니다. 이전에 무엇을 했고, 지금 무엇을 했는지에 대한 정보를 갖고 있지 않습니다. 웹 브라우저(클라이언트)의 요청에 대한 응답을 하고 나면 해당 클라이언트와의 연결을 지속하지 않습니다. 이 때, 웹에서의 상태 유지를 위해 `Cookie`와 `Session`기술이 등장합니다.



### 쿠키(Cookie)

PHP에서는 쿠키를 사용하려면, `setCookie()`로 쿠키를 설정하고  `$_COOKIE` 변수를 이용해 사용할 수 있습니다.

```php
<?php
// 쿠키를 cookie1을 키로 '생활코딩'을 값으로 쿠키를 생성합니다.
setCookie('cookie1', '생활코딩');
// 쿠키의 유효시간을 1분으로 설정합니다. 
setCookie('cookie2', time(), time()+60);
?>
```

```php
<?php
// cookie1의 값을 가져옵니다.
echo $_COOKIE['cookie1']."<br />";
// 현재시간 - cookie2의 값
echo time()-$_COOKIE['cookie2'];
?>
```



### 세션(Session)

세션은 SID(session ID)를 식별자로 서버에 데이터를 저장합니다. SID로는 쿠키나 도메인 파라미터를 사용할 수 있습니다. 데이터는 서버 내에 파일이나 DB에 저장 하고 주로 사용자 인증시에 사용합니다.

`session_start()`로 시작할 수 있고, 스크립트의 최상단에 위치해야 합니다.

세션의 값은 `$_SESSION` 변수를 이용해 사용할 수 있습니다.

```php
<?php
// session 경로에 세션을 저장합니다. (파일로 생성됨)
session_save_path('./session');
// 세션을 시작합니다.
session_start();
// title을 키, '생활코딩'이 값
$_SESSION['title'] = '생활코딩';
?>
```

```php
<?php
ini_set("display_errors", "1");
session_save_path('./session');
session_start();
// $_SESSION 변수에서 title이라는 키의 값을 가져와 출력합니다.
echo $_SESSION['title'];
// 세션 파일을 가져와 출력합니다.
echo file_get_contents('/ex.u/cep/cep/html/egoing/session/session/sess_'.session_id());
?>
```





---

참고 : [https://opentutorials.org/course/62](https://opentutorials.org/course/62)


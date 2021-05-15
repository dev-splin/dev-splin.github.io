---
title: "Spring : Given-When-Then Pattern"
excerpt_separator: <!--more-->
categories:
  - Spring
tags:
  - Spring
  - "Spring : Given-When-Then Pattern"
  - "Spring : assertThat"
  - "Spring : BDD(Behavior Driven Development)"
toc: true
toc_sticky: true
toc_label: 목차
---

# Given-When-Then Pattern

`Given When Then`은 테스트 코드를 작성할 때, 테스트코드의 라이프사이클을 정해놓은 영역을 의미합니다. 즉, `Given-When-Then Pattern`은 테스트할 준비를 하고(`Given`), 테스트할 대상을 실행하고(`When`), 검증(`Then`)하는 것입니다. 이 패턴은 `BDD(Behavior Driven Development)`에서 나온 패턴인데, **BDD는 TDD에서 테스트라는 말이 너무 추상적이어서 테스트를 행위로 바꾼 행위 주도 개발법**을 말합니다. 아주 간단한 패턴이지만 저 패턴을 잘 지키면 테스트 코드를 이해하기가 쉬워지니 지키도록 노력하는게 좋습니다.

간단하게 **SpringMVC에서 Category의 개수를 가져오는 Service를 테스트**하는 코드를 보며 설명하겠습니다.

```java
	@Test
	public void countTest() throws Exception {
		// given
		given(categoryDao.selectCount()).willReturn(1);
//		when(categoryDao.selectCount()).thenReturn(1);
		
		//when
		int result = categoryService.getCount();
		
		//then
		verify(categoryDao).selectCount();
		assertThat(result, is(1));
//		Assert.assertEquals(1, result);
	}
```



## Given

**테스트를 위해 준비하는 과정** 입니다. **테스트에 사용하는 변수, 입력 값 등을 정의하거나 Mock 객체를 정의하는 구문도 Given에 포함**합니다.

```java
		// given
		given(categoryDao.selectCount()).willReturn(1);
//		when(categoryDao.selectCount()).thenReturn(1);
```

위의 코드는 `categoryService`에서 사용하는 `categoryDao`의 `selectCount()` 메서드의 결과를 `1`로 정의합니다.



### Mockito.when

```java
import static org.mockito.Mockito.when;

	@Test
	public void countTest() throws Exception {
		when(categoryDao.selectCount()).thenReturn(1);
        ...
    }
```

`Mockito`의 `when()` 메서드는 `categoryDao`의 `selectCount()` 메서드의 결과를 정의하는 것 이기 때문에 `Given`에 포함됩니다. 하지만 **Given의 과정에 when이 나오는 것은 헷갈릴 수 있는 가능성**이 있습니다. 



### BDDMockito.given

```java
import static org.mockito.BDDMockito.given;

	@Test
	public void countTest() throws Exception {
		given(categoryDao.selectCount()).willReturn(1);
        ...
    }
```

`Mockito.when`을 사용할 때, **Given의 과정에 when이 나오는 것은 헷갈릴 수 있는 가능성**이 있는데, 이 문제를 해결하기 위해 등장한 것이 `BDDMockito` 입니다. `BDDMockito`는 `Mockito`를 상속한 클래스이고, 동작이나 사용하는 방법 또한 `Mockito`와 거의 차이가 없습니다. 즉, `BDDMockito`는 **BDD를 사용하여 테스트코드를 작성할 때, 시나리오에 맞게 테스트 코드가 읽힐 수 있도록 도와주는 (이름을 변경한) 클래스** 입니다.





## When

**실제로 테스트할 메서드를 실행하는 과정**입니다. **하나의 메서드만 수행하는 것이 바람직**하기 때문에, 일반적으로 `When`의 과정은 테스트 코드에서 가장 심플하고도 중요한 구문입니다.

```java
		//when
		int result = categoryService.getCount();
```

위의 코드는 `categoryService`의 `getCount()`메서드를 실행합니다.





## Then

**테스트를 검증하는 과정**입니다. **예상한 값, 실제 실행을 통해서 나온 값을 검증**합니다.

```java
		//then
		verify(categoryDao).selectCount();
		assertThat(result, is(1));
//		Assert.assertEquals(1, result);
```

위의 코드는 `verify()` 메서드로 `selectCount()`가 **실행된 적이 있는지 검증**하고, **result의 값에 기대하는 값이 나왔는지 확인**합니다.



### util.Assert.assertEquals()

```java
import org.junit.Assert;

	@Test
	public void countTest() throws Exception {
		...
		Assert.assertEquals(1, result);
        ...
    }
```

기대하는 값이 나오는지 확인하는 `assertEquals()` 메서드는 **첫 번째 인자로 기대값**을 받고, **두 번째 인자로 실제값**을 받습니다. 실제값이 앞에 들어가는게 자연스러워보이기 때문에 인자의 위치에 대해서 헷갈릴 수 있는 메서드입니다.



### junit.Assert.assertThat()

```java
import static org.junit.Assert.assertThat;
import static org.hamcrest.CoreMatchers.is;

import org.junit.Assert;

	@Test
	public void countTest() throws Exception {
		...
		assertThat(result, is(1));
        ...
    }
```

`assertThat()`은 `hamcrest`가 `static` 메서드로 제공하는 여러 `matcher`를 사용할 수 있고 이러한 static 메서드는 체이닝할 수 있어서 **기존 assertXXX 메서드보다 더 많은 유연성을 제공**합니다. 그 외에도 다양한 이점이 있습니다.



#### 가독성

`assertEquals()`를 사용할 때 마다 인자의 위치에 대해서 헷갈릴 때가 많기 때문에 `assertThat()`을 사용해서 작성하면 그 의미를 더 분명히 할 수 있습니다. `assertThat()`의 **첫 번째 인자는 실제값**을 받고, **두 번째 인자는 기대값**을 받습니다. 이 때, **두 번째 값에는 단순히 값만 들어가는 것이 아니고, hamcrest가 static 메서드로 제공하는  matcher**가 들어가게 됩니다. (`import static org.hamcrest.CoreMatchers.is`)



#### Failure 메세지

**assertThat()은 더 나은 에러 메시지를 제공**합니다.

**assertTrue()를 사용하는 예**

```
assertTrue(expected.contains(actual));

--- 실행 결과 ---
java.lang.AssertionError
	at org.junit.Assert.fail(Assert.java:86)
	at org.junit.Assert.assertTrue(Assert.java:41)
	at org.junit.Assert.assertTrue(Assert.java:52)
```

특정 string을 포함하는지 확인하기 위해서는 `assertStringContains()`와 같은 메서드가 없기 때문에 `contains()` 메서드를 사용하고 `assertTrue()`을 이용해 검증해야 합니다. 여기서 **assertion error는 expected 값과 actual 값에 대해 알려주지 않는다는 것**입니다.



**assertThat을 사용하는 예**

```java
assertThat(actual, containsString(expected));

--- 실행 결과 ---
Expected: a string containing "expected"
     but: was "actual"
java.lang.AssertionError: 
Expected: a string containing "expected"
     but: was "actual"
	at org.hamcrest.MatcherAssert.assertThat(MatcherAssert.java:20)
```

**expected 값과 actual 값 모두 에러 메시지에 반환**됩니다. 원인을 찾기 위해 별도의 디버깅 필요없이 에러 메시지 만으로 잘못된 부분을 바로 파악할 수 있습니다.



#### Type 안정성

**assertThat을 사용하면 Type에 대한 안정성도 얻을 수 있습니다.**

**assertEquals()를 사용하는 예**

```java
static public void assertEquals(Object expected, Object actual) {
    assertEquals(null, expected, actual);
}
```

`assertEquals()`의 구현은 위와 같이 `Object`를 인자로 받고 있기 때문에 아래의 코드에서 컴파일에는 성공하지만, 실행시 실패합니다.

```java
assertEquals("abc", 123);	// 실행시 실패
```



**assertThat()를 사용하는 예**

```java
public static <T> void assertThat(T actual, Matcher<? super T> matcher) {
    assertThat("", actual, matcher);
}
```

`assertThat()`의 구현은 위와 같이 **Generic을 사용하고 있어 Type이 체크**되기 때문에,

같은 경우에 `assertThat()`은 `assertEquals()`와 다르게  컴파일을 허용하지 않습니다.

```java
assertThat(123, is("abc")); // 컴파일 실패
```



#### 유연성

**hamcrest는 `anyOf()`와 `allOf()`와 같은 논리 matcher도 제공**합니다.

```java
assertThat("test", allOf(is("test2"), containsString("te")));
```

**allOf() matcher는 and 논리 연산자처럼 동작**합니다. 따라서 **제공된 모든 matcher에 통과**해야 합니다. 실패하는 경우 다음과 같이 실패한 matcher에 대해서 에러 메시지를 반환합니다.

```java
Expected: (a string containing "te" and is "test2")
     but: is "test2" was "test"
java.lang.AssertionError: 
Expected: (a string containing "te" and is "test2")
     but: is "test2" was "test"
	at org.hamcrest.MatcherAssert.assertThat(MatcherAssert.java:20)
```



#### Custom Matchers

**assertThat()에서 다양한 matcher 이용해 custom assert 메서드를 만들 수 있는 것처럼 다음과 같이 custom matcher를 생성**할 수 있습니다.

```
import org.hamcrest.Description;
import org.hamcrest.TypeSafeMatcher;

public static class CustomMatcher extends TypeSafeMatcher<String> {
    private final String expected;

    public CustomMatcher(String expected) {
        this.expected = expected;
    }

    @Override
    protected boolean matchesSafely(String item) {
        return expected.equals(item);
    }

    @Override
    public void describeTo(Description description) {
        description.appendValue(expected);
    }
}
```



#### Reference

`assert 메서드`와 그에 상응하는 `assertThat(hamcrest 1.3 기준) matcher`에 대해서 알아봅니다.

| assert method                                                | assertThat                                                   | static Import                                                |
| :----------------------------------------------------------- | :----------------------------------------------------------- | :----------------------------------------------------------- |
| assertEquals(“expected”, “actual”);                          | assertThat(“actual”, is(“expected”));                        | org.hamcrest.core.Is.is                                      |
| assertArrayEquals(new String[]{“test1”, “test2”}, new String[]{“test3”, “test4”}); | assertThat(new String[]{“test1”, “test2”}, is(new String[]{“test3”, “test4”})); | org.hamcrest.core.Is.is                                      |
| assertTrue(value);                                           | assertThat(actual, is(true));                                | org.hamcrest.core.Is.is                                      |
| assertFalse(value);                                          | assertThat(actual, is(false));                               | org.hamcrest.core.Is.is                                      |
| assertNull(value);                                           | assertThat(actual, nullValue);                               | org.hamcrest.core.IsNull.nullValue                           |
| assertNotNull(value);                                        | assertThat(actual, notNullValue);                            | org.hamcrest.core.IsNull.notNullValue                        |
| assertSame(expected, actual);                                | assertThat(actual, sameInstance(expected));                  | org.hamcrest.core.IsSame.sameInstance                        |
| assertNotSame(expected, actual);                             | assertThat(actual, not(sameInstance(expected)));             | org.hamcrest.core.IsNot.not, org.hamcrest.core.IsSame.sameInstance |
| assertTrue(1 > 3);                                           | assertThat(1, greaterThan(3));                               | org.hamcrest.number.OrderingComparison.greaterThan           |
| assertTrue(“abc”.contains(“d”));                             | assertThat(“abc”, containsString(“d”));                      | org.hamcrest.core.StringContains.containsString              |

이 외에도 더 많은 matcher가 있습니다.

- org.hamcrest.beans
  - HasProperty
  - HasPropertyWithValue
  - SamePropertyValuesAs
- org.hamcrest.collection
  - IsArray
  - IsArrayContaining
  - IsArrayContainingInAnyOrder
  - IsArrayContainingInOrder
  - IsArrayContainingWithSize
  - IsCollectionWithSize
  - IsEmptyCollection
  - IsEmptyIterable
  - IsIn
  - IsIterableContainingInAnyOrder
  - IsIterableContainingInOrder
  - IsIterableWithSize
  - IsMapContaining
- org.hamcrest.number
  - BigDecimalCloseTo
  - IsCloseTo
  - OrderingComparison
- org.hamcrest.object
  - HasToString
  - IsCompatibleType
  - IsEventFrom
- org.hamcrest.text
  - IsEmptyString
  - IsEqualIgnoringCase
  - IsEqualIgnoringWhiteSpace
  - StringContainsInOrder



---

참고 : [https://brunch.co.kr/@springboot/292](https://brunch.co.kr/@springboot/292)

[https://jongmin92.github.io/2020/03/31/Java/use-assertthat/](https://jongmin92.github.io/2020/03/31/Java/use-assertthat/)

[https://multifrontgarden.tistory.com/187](https://multifrontgarden.tistory.com/187)

[https://velog.io/@lxxjn0/Mockito%EC%99%80-BDDMockito%EB%8A%94-%EB%AD%90%EA%B0%80-%EB%8B%A4%EB%A5%BC%EA%B9%8C](https://velog.io/@lxxjn0/Mockito%EC%99%80-BDDMockito%EB%8A%94-%EB%AD%90%EA%B0%80-%EB%8B%A4%EB%A5%BC%EA%B9%8C)


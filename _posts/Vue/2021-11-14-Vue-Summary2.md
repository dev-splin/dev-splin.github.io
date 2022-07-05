---
title: "Vue : 정리(2)"
excerpt_separator: <!--more-->
categories:
  - Vue
tags:
  - Vue
  - "Vue : Sammary"
toc: true
toc_sticky: true
toc_label: 목차
---

# Vue 정리(2)

캡틴판교 장기효님의 `초급 ~ 실전 Vue.js로 완성하는 프론트엔드 개발자 로드맵`을 보면서 제 나름대로 정리한 글입니다. (개인적인 시간 상 전부 정리하기 보다는 몰랐던 부분을 정리합니다.)





## 뷰 라우터

뷰 라우터에 대해서는 아래의 링크에 자세히 설명되어 있습니다.

하나 참고할 점은 아래처럼 **mode에 history를 넣어주면 url의 해쉬 값(#)을 제거할 수 있다는 것**입니다.

```vue
new VueRouter({
  mode: 'history',
  routes: [
    { path: '/login', component: LoginComponent },
    { path: '/home', component: HomeComponent }
  ]
})
```



**참고 링크**

- [뷰 라우터 - Cracking Vue.js](https://joshua1988.github.io/vue-camp/vue/router.html#%E1%84%87%E1%85%B2-%E1%84%85%E1%85%A1%E1%84%8B%E1%85%AE%E1%84%90%E1%85%A5-%E1%84%89%E1%85%A5%E1%86%AF%E1%84%8E%E1%85%B5)
-  [Vue.js 라우터 네비게이션 가드 알아보기 - joshua1988(캡틴 판교)](https://joshua1988.github.io/web-development/vuejs/vue-router-navigation-guards/)



## 액시오스(Axios)

vue에서는 vue resource라는 공식 라이브러리가 있었지만, 현재는 사용하지 않기 때문에 최근 뷰에서 권고하는 HTTP 통신 라이브러리는 `액시오스(Axios)`가 되었습니다. 액시오스는 Promise 기반의 HTTP 통신 라이브러리이며 상대적으로 다른 HTTP 통신 라이브러리들에 비해 문서화가 잘되어 있고 API가 다양합니다.



**참고 링크**

- [자바스크립트 비동기 처리와 콜백 함수 - joshua1988(캡틴 판교)](https://joshua1988.github.io/web-development/javascript/javascript-asynchronous-operation/)
- [자바스크립트 Promise 쉽게 이해하기 - joshua1988(캡틴 판교)](https://joshua1988.github.io/web-development/javascript/promise-for-beginners/)
- [자바스크립트 async와 await - joshua1988(캡틴 판교)](https://joshua1988.github.io/web-development/javascript/js-async-await/)





## Computed와 Methods의 차이

참고 링크에 더 자세한 설명을 볼 수 있습니다만, 간략히 줄여보겠습니다.

Computed와 똑같은 기능을 **Methods로 사용할 경우 렌더링 할 때마다 Methods에 정의한 익명 함수의 로직이 수행**되지만, **Computed는 해당 익명 함수에 종속된 값이 바뀌기 전 까지는 로직의 결과를 캐싱해서 가지고 있기 때문에 Methods에 비해 더 빠릅니다.**

때문에 대부분이 Computed를 사용하지만, 캐싱을 사용하지 않을 때는 Methods를 사용해도 괜찮습니다.



## Computed와 watch의 차이

computed와 watch는 특정 값이 변경되면 실행되기 때문에 혼동될 수 있습니다.

하지만 둘은 조금 다른데, 우선 **Computed는 Computed의 익명 함수 속에 종속되는 값이 변경되면 실행되는 것(return 값 필수)**이고,
**watch는 메서드의 이름에 해당하는 값이 변경되면 실행되는 것(새로운 값과 이전 값을 이용할 수 있음)**입니다.

a+b로 이루어진 c의 값이 있다고 가정해보겠습니다.

```vue
computed: {
	c: function() {
		return a + b;
	}
}

watch: {
	a: function(newValue, oldValue) {
		axios.get(....)
	}
}
```

- 위의 예시처럼 인스턴스의 data에 할당된 값들 사이의 종속관계를 자동으로 세팅하고자 할때는 `computed`로 구현하는것이 좋습니다. 
  -  c의 값이 a,b에에 따라 결정되어집니다.
  - 이 종속관계가 조금이라도 복잡해질 때, `watch`로 구현할 경우 중복계산이 일어나거나 코드 복잡도가 높아질 것 입니다. 이는 오류가 발생할 가능성이 증가합니다.
- `watch`는 특정 프로퍼티의 변경시점에 특정 액션(call api, push route …)을 취하고자 할때 적합합니다.
- `computed`의 경우 종속관계가 복잡할 수록 재계산 시점을 예상하기 힘들기 때문에 종속관계의 값으로 계산된 결과를 리턴하는 것 외의 사이드 이펙트가 일어나는 코드를 지양해야합니다.
- 만약 `computed`로 구현가능한 것이라면 `watch`가 아니라 `computed`로 구현하는것이 대게의 경우 좋습니다.



**참고 링크**

- [Vue 공식 문서 - computed와 watch](https://kr.vuejs.org/v2/guide/computed.html)
- [[Vue.js] watch와 computed 의 차이와 사용법](https://framework.zend.com/manual/1.12/en/learning.quickstart.create-project.html)





## Vue CLI(Command Line Interface)

`Vue CLI(Command Line Interface)`에서 **CLI(Command Line Interface)는 명령어를 통한 특정 액션을 취하는 도구를 의미**합니다. 즉, Vue를 명령어를 통해 조작할 수 있다는 의미입니다.



**참고 링크**

- [Vue CLI 공식 사이트](https://cli.vuejs.org/)





## Vue 프로젝트 생성

1. `vue create [생성할 프로젝트이름]` 을 이용해 Vue 프로젝트를 생성
   - 이 때, 다양한 옵션을 선택할 수 있습니다. (현재는 default로)
2. 프로젝트 생성 후 `npm install(== npm i)` 명령어 실행
3. `npm install -g @vue/cli` 명령어 실행
4. `npm run serve` 명령어 실행으로 간단한 Vue 페이지를 볼 수 있습니다.



---

참고 : [[초급 ~실전] Vue.js로 완성하는 프론트엔드 개발자 로드맵 - 장기효(캡틴판교)](https://www.inflearn.com/roadmaps/3)




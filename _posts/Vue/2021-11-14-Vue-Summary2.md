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



---

참고 : [[초급 ~실전] Vue.js로 완성하는 프론트엔드 개발자 로드맵 - 장기효(캡틴판교)](https://www.inflearn.com/roadmaps/3)




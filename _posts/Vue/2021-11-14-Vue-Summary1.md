---
title: "Vue : 정리(1)"
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

# Vue 정리(1)

캡틴판교 장기효님의 `초급 ~ 실전 Vue.js로 완성하는 프론트엔드 개발자 로드맵`을 보면서 제 나름대로 정리한 글입니다.





## Plugins 추천

- Vetur
- Night Owl
- Material Icon Theme
- Live Server
- ESLint
- Prettier
- Auto Close Tag
- Atom Keymap





## Reactivity

뷰의 핵심인 Reactivity는 **데이터의 변화를 라이브러리에서 감지해서 알아서 화면에 그려주는 것**입니다.



### Object.defineProperty()

객체의 속성을 재정의 하는 함수입니다. 이 함수를 이용하여 아래의 예제처럼 동적으로 화면을 그릴 수 있습니다.

```javascript
Object.defineProperty(대상 객체, 객체의 속성, {
  // 정의할 내용                    
})
```



**예제**

```javascript
let div = document.querySelector('#app');
let viewModel = {};

Object.defineProperty(viewModel, 'str', {
    // 속성에 접근했을 때의 동작을 정의
    get: function() {
    	console.log('접근');
    },
    // 속성에 값을 할당했을 때의 동작을 정의
    set: function(newValue) {
        console.log('할당', newValue);
        div.innerHTML = newValue;
    }
})
```





## 인스턴스(Instance)

인스턴스는 뷰로 개발할 때 필수로 생성해야 하는 코드 입니다.

```vue
<body>
	<div id="app">
       
    </div>

<scirpt>
    let vm = new Vue({
        el: '#app',
        data: {
        message: 'hi'
        }
    });
</scirpt>
    
</body>
```

위의 코드는 `el`에서 정의한 `app`이라는 id를 가진 태그를 찾아서 인스턴스를 붙이겠다는 의미 입니다.

**인스턴스를 붙여줘야 Vue의 다양한 기능과 속성들을 사용할 수 있습니다.**

인스턴스를 생성하면 인스턴스는 Root 컴포넌트가 됩니다.





## 컴포넌트(Component)

![component](https://user-images.githubusercontent.com/79291114/139777640-5b6768eb-a2c2-43f2-aa70-de1925421df8.PNG)

**컴포넌트는 화면의 영역을 영역 별로 구분해서 관리하는 것**이라고 보면 됩니다. 컴포넌트의 핵심은 영역을 구분하여 재사용성을 높이는 것 입니다.



### 전역 컴포넌트와 지역 컴포넌트

전역과 지역으로 컴포넌트를 생성할 수 있습니다.

```vue
<script>
    // 전역 컴포넌트
    Vue.component('app-header', {
        template: '<h1>Header</h1>'
    });

    Vue.component('app-content', {
        template: '<div>Content</div>'
    });

    new Vue({
        el: '#app',
        // 지역 컴포넌트
        components: {
            'app-footer' : {
                template: '<footer>footer</footer>'
            }
        }
    });
</script>
```

만약 인스턴스가 여러 개가 있다고 가정했을 때, **전역 컴포넌트는 어떤 인스턴스에서도 다 사용 가능**하고 **지역컴포넌트는 해당 컴포넌트를 선언한 인스턴스에서만 사용 가능**합니다.



## 컴포넌트 통신

![components-communication](https://user-images.githubusercontent.com/79291114/141136329-71a83b9d-8b0e-4b46-b466-54a24c24c84f.PNG)

- 프롭스(props) 속성 : 상위 컴포넌트에서 하위로 데이터를 내려줄 때 사용
- 이벤트(emit) 발생 : 하위에서 상위로 이벤트를 올려줄 때 사용

데이터는 한방향으로만 움직이는 것이 아니라 유기적 으로 다양한 통신을 하는 경우가 대다수이기 때문에 컴포넌트 통신 규칙이 필요하게 됩니다.



### 프롭스(props)

상위 컴포넌트에서 하위로 데이터를 내려줄 때 사용합니다. 즉, 상위 컴포넌트에서 하위 컴포넌트를 적용할 때 값을 설정해서 사용할 수 있다는 것입니다.

```vue
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <meta http-equiv="X-UA-Compatible" content="ie=edge">
  <title>Document</title>
</head>
<body>
  <div id="app">
    <!-- <app-header v-bind:프롭스 속성 이름="상위 컴포넌트의 데이터 이름"></app-header> -->
    <!-- 하위 컴포넌트 props에 값을 설정 -->
    <app-header v-bind:propsdata="message"></app-header>
    <app-content v-bind:propsdata="num"></app-content>
  </div>

  <script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
  <script>
    var appHeader = {
      template: '<h1>{{ propsdata }}</h1>',
      props: ['propsdata']
    }
    var appContent = {
      template: '<div>{{ propsdata }}</div>',
      props: ['propsdata']
    }

    new Vue({
      el: '#app',
      components: {
        'app-header': appHeader,
        'app-content': appContent
      },
      data: {
        message: 'hi',
        num: 10
      }
    })
  </script>
</body>
</html>
```



### 이벤트(emit)

하위컴포넌트에서 상위컴포넌트로 이벤트를 올려줄 때 사용합니다. 즉, 하위 컴포넌트의 특정 부분에서 사용되는 메소드를 상위 컴포넌트에서 정의할 수 있는 것입니다.

```vue
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <meta http-equiv="X-UA-Compatible" content="ie=edge">
  <title>Document</title>
</head>
<body>
  <div id="app">
    <p>{{ num }}</p>
    <!-- <app-header v-on:하위 컴포넌트에서 발생한 이벤트 이름="상위 컴포넌트의 메서드 이름"></app-header> -->
    <!-- 하위 컴포넌트의 메소드(pass, increase)를 실행시키면 상위 컴포넌트의 메소드(logText, increaNumber)가 실행됨 -->
    <app-header v-on:pass="logText"></app-header>
    <app-content v-on:increase="increaseNumber"></app-content>
  </div>

  <script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
  <script>
    var appHeader = {
      template: '<button v-on:click="passEvent">click me</button>',
      methods: {
        passEvent: function() {
          this.$emit('pass');
        }
      }
    }
    var appContent = {
      template: '<button v-on:click="addNumber">add</button>',
      methods: {
        addNumber: function() {
          this.$emit('increase');
        }
      }
    }

    var vm = new Vue({
      el: '#app',
      components: {
        'app-header': appHeader,
        'app-content': appContent
      },
      methods: {
        logText: function() {
          console.log('hi');
        },
        increaseNumber: function() {
          this.num = this.num + 1;
        }
      },
      data: {
        num: 10
      }
    });
  </script>
</body>
</html>
```



#### this의 활용

JavaScript를 보면 `this.`가 많이 나오는데, 이 this에 대해서 정확히 알고 갈 필요가 있습니다. (Vue도 JavaScript 기반이기 때문에 알아두면 좋습니다.)

**참고 링크**

- https://www.w3schools.com/js/js_this.asp
- https://betterprogramming.pub/understanding-the-this-keyword-in-javascript-cb76d4c7c5e8



### 같은 레벨에서의 컴포넌트 통신 방법

상위에서 하위, 하위에서 상위 간의 통신이 아니라 같은 레벨에서의 컴포넌트 통신이 필요할 수도 있습니다. 이 때, 상위 컴포넌트를 통해 같은 레벨의 컴포넌트 끼리 통신하는 방법입니다.

즉, 하위 컴포넌트1과 하위 컴포넌트2가 있다고 가정할 때,
하위 컴포넌트1에서 상위 컴포넌트로 **emit으로 통신 시, 파라미터 값을 넘겨 주고** 상위 컴포넌트와 하위 컴포넌트2를 **props로 통신 시 사용하는 변수의 값을 emit을 정의한 메서드에서 변경해주는 방법**입니다.

```vue
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <meta http-equiv="X-UA-Compatible" content="ie=edge">
  <title>Document</title>
</head>
<body>
  <div id="app">
    <!-- props로 값을 넣어줌 -->
    <app-header v-bind:propsdata="num"></app-header>
    <!-- pass를 deliverNum으로 정의 -->
    <app-content v-on:pass="deliverNum"></app-content>
  </div>

  <script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
  <script>
    var appHeader = {
      template: '<div>header</div>',
      props: ['propsdata']
    }
    var appContent = {
      template: '<div>content<button v-on:click="passNum">pass</button></div>',
      // 상위 컴포넌트로 통신할 때, 파라미터를 넘겨줌
      methods: {
        passNum: function() {
          this.$emit('pass', 10);
        }
      }
    }

    new Vue({
      el: '#app',
      components: {
        'app-header': appHeader,
        'app-content': appContent
      },
      data: {
        num: 0
      },
      methods: {
        // props로 연결된 변수 값을 변경
        deliverNum: function(value) {
          this.num = value;
        }
      }
    })
  </script>
</body>
</html>
```



---

참고 : [[초급 ~실전] Vue.js로 완성하는 프론트엔드 개발자 로드맵 - 장기효(캡틴판교)](https://www.inflearn.com/roadmaps/3)




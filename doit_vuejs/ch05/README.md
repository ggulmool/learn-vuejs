# 5장. 화면을 개발하기 위한 기본 지식과 팁(템플릿 & 프로젝트 구성)

## 뷰 템플릿
### 뷰 템플릿이란?
- HTML, CSS 등의 마크업 속성과 뷰 인스턴스에서 정의한 데이터 및 로직들을 연결하여 사용자가 브라우저에서 볼수 잇는 형태의 HTML로 변환해주는 속성
- 템플릿 속성을 사용하는 방법 두가지
    - 첫번째. ES5에서 뷰 인스턴스의 template속성을 활용하는 방법
        - 라이브러리 내부적으로 template속성에서 정의한 마크업 + 뷰 데이터를 가상 돔 기반의 render()함수로 변환한다.
    ```javascript
    new Vue({
        template: '<p>Hello {{ message }}</p>'
    });
    ```
    - 두번째. 싱글 파일 컴포넌트 체계의 `<template>` 코드를 활용하는 방법
    ```HTML
    <template>
      <p>Hello {{ message }}</p>
    </template>
    ```
- 템플릿에서 사용하는 뷰의 속성과 문법 
    - 데이터 바인딩
    - 자바스크립트 표현식
    - 디렉티브
    - 이벤트 처리
    - 고급 템플릿 기법

### 데이터 바인딩
- HTML 화면 요소를 뷰 인스턴스의 데이터와 연결하는 것
- `{{ }}` 콧수염 괄호
    - 뷰 인스턴스의 데이터를 HTML 태그에 연결하는 가장 기본적인 텍스트 삽입 방식
    ```HTML
    <div id="app">
      {{ message }}
    </div>
  
    <script>
      new Vue({
         el: '#app',
         data: {
           message: 'Hello Vue.js!'  
         } 
      });
    </script>
    // data 속성의 message값이 바뀌면 뷰 반응성에 의해 화면이 자동으로 갱신.
  
    ``` 
- v-bind
    - 아이디, 클래스, 스타일 등의 HTML 속성 값에 뷰 데이터 값을 연결할 때 사용하는 데이터 연결방식
    - v-bind: 문법을 :로 간소화. ex) v-bind:id와 :id는 같은 동작
    ```html
    <div id="app">
        <p v-bind:id="idA">아이디 바인딩</p>
        <p v-bind:class="classA">클래스 바인딩</p>
        <p v-bind:style="styleA">스타일 바인딩</p>
    </div>
    <script src="https://cdn.jsdelivr.net/npm/vue@2.5.2/dist/vue.js"></script>
    <script>
        new Vue({
            el: '#app',
            data: {
                idA: 10,
                classA: 'container',
                styleA: 'color: blue'
            }
        });
    </script>
    ```
    
    [v-bind예제](./1_vbind_props.html)

### 자바스크립트 표현식
- 뷰의 템플릿에서도 자바스크립트 표현식 사용가능
```html
<div id="app">
    {{ var a = 10; }} <!-- X, 선언문은 사용 불가능 -->
    {{ if (true) {return 100} }} <!-- X, 분기 구문은 사용 불가능 -->
    {{ true ? 100 : 0 }} <!-- O, 삼항 연산자로 표현 가능 -->
    
    {{ message.split('').reverse().join('') }} <!-- X, 복잡한 연산은 인스턴스 안에서 수행 -->
    {{ reversedMessage }} <!-- O, 스크립트에서 computed 속성으로 계산 후 최종 값만 표현 -->
</div>
<script>
    new Vue({
        el: '#app',
        data: {
            message: 'Hello Vue.js!'
        },
        computed: {
            reversedMessage: function() {
                return this.message.split('').reverse().join('');
            }
        }
    });
</script>
```

[자바스크립트 표현식 주의할점 예제](./2_template_javascript_expr.html)

### 디렉티브
- HTML 태그 안에 v- 접두사를 가지는 모든 속성들을 의미
- 화면의 요소를 더 쉽게 조작하기 위해 사용하는 기능

| 디렉티브이름                      | 역할                                                                         |
|--------------------------|------------------------------------------------------------------------------|
| v-if              | 지정한 뷰 데이터 값의 참, 거짓 여부에 따라 해당 html태그를 화면에 표시 혹은 미표시. |
| v-for             | 지정한 뷰 데이터의 개수만큼 해당 html태그를 반복 출력         |
| v-show            | v-if는 해당 태그를 완전히 삭제하지만 v-show는 css효과만 display:none;으로 주어 실제 태그는 남아있고 화면상에서만 숨김.          |
| v-bind            | html 태그의 기본 속성과 뷰 데이터 속성을 연결         |
| v-on              | 화면 요소의 이벤트를 감지하여 처리할 때 사용. ex) v-on:click         |
| v-model           | 폼에 입력한 값을 뷰 인스턴스의 데이터와 즉시 동기화         |

```html
<div id="app">
  <a v-if="flag">두잇 Vue.js</a>
  <ul>
    <li v-for="system in systems">{{ system }}</li>
  </ul>
  <p v-show="flag">두잇 Vue.js</p>
  <h5 v-bind:id="uid">뷰 입문서</h5>
  <button v-on:click="popupAlert">경고 창 버튼</button>
</div>

<script>
  new Vue({
    el: '#app',
    data: {
      flag: true,
      systems: ['android', 'ios', 'window'],
      uid: 10
    },
    methods: {
      popupAlert: function() {
        return alert('경고 창 표시');
      }
    }
  });
</script>
```
[뷰 디렉티브 예제](./3_vue_directive.html)

### 이벤트 처리
- 화면에서 발생한 이벤트를 처리하기 위해 `v-on 디렉티브`와 `method 속성` 사용
```html
<!-- v-on 디렉티브 이용해 이벤트 처리하기 -->
<button v-on:click="clickBtn">클릭</button>
<script>
    methods: function() {
        alert('clicked');
    }
</script>
<!-- v-on 디렉티브 메서드 호출할 때 인자값 넘기기-->
<button v-on:click="clickBtn(10)">클릭</button>
<script>
    methods: function(num) {
        alert('clicked ' + num + ' times');
    }
</script>
<!-- event 인자를 이용해 돔 이벤트 접근하기 -->
<button v-on:click="clickBtn">클릭</button>
<script>
    methods: function(event) {
        console.log(event)
    }
</script>
```

### 고급 템플릿 기법
- computed 속성
    - 데이터를 가공하는 등의 복잡한 연산은 뷰 인스턴스 안에서 하고 HTML에는 데이터 표현만
    - data속성 값의 변화에 따라 자동으로 다시 연산
    - 캐싱: 동일한 연산을 반복해서 하지 않기 위해 연산의 결과 값을 미리 저장하고 있다가 필요할 때 불러오는 동작
- computed 속성과 methods 속성의 차이점
    - methods 속성은 호출할 때만 해당 로직이 수행
    - computed 속성은 대상 데이터의 값이 변경되면 자동적으로 수행
        - 데이터가 변경되지 않는 한 이전의 계산값을 캐싱하고 있다고 필요할 때 반환
        - methods 속성을 이용하는 것보다 성능면에서 효율적
- watch 속성
    - 데이터 변화를 감지하여 자동으로 특정 로직을 수행
    - 데이터 호출과 같이 시간이 상대적으로 많이 소모되는 비동기 처리에 적합
```html
<div id="app">
  <input v-model="message">
</div>

<script>
  new Vue({
    el: '#app',
    data: {
      message: 'Hello Vue.js!'
    },
    watch: {
      message: function(data) {
        console.log("message의 값이 바뀝니다 : ", data);
      }
    }
  });
</script>
```

## 뷰 프로젝트 구성방법
### HTML 파일에서 뷰 코드 작성시의 한계점
- template 속성에 작성된 html코드의 결과화면을 예상하기 어려움.
- `<script>`태그안에 html코드는 구문 강조가 적용되지 않기 때문에 오탈자 찾기 어렵다.
- 코드 들여쓰기도 어려워 태그의 관계를 파악하기 어렵다.

### 싱글 파일 컴포넌트 체계
- .vue 파일로 프로젝트 구조를 구성하는 방식

```html
<template>
<!-- 화면에 표시할 요소들을 정의하는 영역 ex) HTML + 뷰 데이터 바인딩 -->
</template>
<script>
export default{
// 뷰 컴포넌트의 내용을 정의하는 영역 ex) template, data, methods 등    
}
</script>
<style>
/* 템플릿에 추가한 html태그의 css스타일을 정의하는 영역 */
</style>
```

### 뷰 CLI
- 뷰 개발자들이 편하게 프로젝트를 구성할 수 있도록 뷰 코어팀에서 CLI도구 제공
    - 웹팩이나 브라우저리파이 같은 모듈 번들러를 프로젝트 자체에 포함하여 바로 사용할 수 있다.
    - .vue 파일을 HTML, CSS, 자바스크립트 파일로 변환해 주기 위한 뷰 로더를 포함한다.
- 결론적으로 .vue 파일 방식으로 애플리케이션을 개발하려면 뷰 로더와 이를 지원하는 웹팩, 브라우저리파이 같은 모듈 번들러가 필요.
- 뷰 CLI 설치
    - npm install vue-cli -g
- 뷰 CLI 명령어
    - vue init [템플릿명]
    
### 뷰로더 
- 웹팩에서 지원하는 라이브러리
    - 싱글 파일 컴포넌트 체계에서 사용하는 .vue 파일의 내용을 브라우저에서 실행 가능한 웹 페이지의 형태로 변환해준다.
```javascript
// webpack.config.js 웹팩의 뷰 로더 처리 부분
module: {
    rules: [
        {
            test: /\.vue$/, // 로더가 적용될 대상 파일을 지정
            loader: 'vue-loader',   // 로더의 종류 지정
            options: {
                loaders: {
                    
                }
            }
        }
    ]
}
```

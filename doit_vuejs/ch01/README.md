## 1장. Vue.js소개

#### Vue.js란?
- 웹 페이지 화면 개발을 위한 화면단 라이브러리이자 프레임워크
    - 프레임워크 : 개발자들의 개발 생산성을 높이기 위해 일정한 틀과 규칙에 따라 개발하도록 미리 구조를 정의해 놓은 도구
    - 라이브러리 : 자주 사용되는 기능들을 모아 재활용할 수 있도록 정리한 기술 모음집
- 뷰코어 라이브러리는 화면단 데이터 표현에 관한 기능들을 중점적으로 지원
- 라우터, 상태관리, 테스팅 등 프레임워크 기능도 제공

#### 뷰의 장점
- 배우기가 쉽다
- 리액트와 앵귤러에 비해 성능이 우수하고 빠르다
- 앵귤러의 데이터 바인딩 특성과 리액트의 가상 돔기반 렌더링 특징 모두 가짐
- 용어정리
    - 타입스크립트 : 기존 자바스크립트에 엄격한 타입 체크를 도입한 언어. 앵귤러2의 표준
    - ES6 : 자바스크립트의 최신 스펙으로, ECMAScript2015와 동일한 용어
    - 웹팩 : 웹 모듈 번들러. 최신 프론트엔드 프레임워크로서 권고하는 필수 웹 성능 개선 도구

### Vue.js 특징
- UI 화면 개발 방법 중 하나인 MVVM 패턴의 뷰 모델(ViewModel)에 해당하는 화면단 라이브러리
```
              뷰모델         
    뷰   --> 돔 리스너   -->     모델
        <-- 데이터 바인딩 <--   
  돔(DOM)    뷰(Vue.js)     자바스크립트 객체
```
- 화면의 요소들을 제어하는 코드와 데이터 제어 로직을 분리
- 컴포넌트 기반 프레임 워크
- 리액트와 앵귤러의 장점을 가진 프레임 워크
    - 빠른 화면 렌더링을 위해 리액트의 가상 돔 렌더링 방식 적용
    - 사용자의 인터랙션이 많은 요즘의 웹화면에 적합한 동작 구조
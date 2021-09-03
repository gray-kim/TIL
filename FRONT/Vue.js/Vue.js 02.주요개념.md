# Vue.js 주요 개념

## 라이프 사이클
### [Vue.js 프로그램이 라이프사이클 안에서 하는 작업]
- 이벤트와 라이프사이클 초기화 작업  
-> 데이터를 감시할 수 있도록 변경   
-> 템플릿 마운트   
-> 데이터 변경이 발생하면 템플릿 업데이트  
-> 데이터와 이벤트 감시 취소  
-> 라이프사이클 종료

- Vue.js는 프로그램 라이프사이클의 연결고리를 제어할 수 있다. 이 연결고리는 '훅'이라고 한다.
---
## 인스턴스와 컴포넌트
### [인스턴스]
- Vue 클래스 초기화
- 아래에서 생성한 vue 객체는 최상위 객체로 동작
- 이 객체를 vue 인스턴스 혹은 루트 컴포넌트라고 함
```javascript
new Vue({
    // 옵션이나 컴포넌트 설정
})
```

- 최상위 vue vm 인스턴스 생성
```javascript
var vm = new Vue({
    // 옵션이나 컴포넌트 설정
})
```

### [컴포넌트]
- 개발자가 HTML 태그 방식으로 웹 페이지 안에 추가 UI를 덧붙일 수 있는 기능을 제공하는 것
```javascript
// 전역 컴포넌트 선언
Vue.component('컴포넌트 이름', {
    // 옵션
});

// 지역 컴포넌트 선언
new Vue({
    // 옵션이나 컴포넌트 설정
});
```

- 컴포넌트 이름은 영어로 시작해야 하고, 소문자로 구성하고, 하이픈(-)을 포함할 수 있다.
- 숫자로 시작하지 않는 것에 한정해 컴포넌트 이름의 일부분으로 사용할 수 있다.
- 컴포넌트 선언 시 카멜케이스 형태로 선언하며 태그로 호출 시 스네이크 케이스 형태로 작성한다.
```html
<div id="app">
    <my-component></my-component>
</div>
<script>
    Vue.component('myComponent', {
        template: '<span>myComponent 컴포넌트는 my-component로 호출해야 합니다.</span>'
    });

    new Vue({
        el: '#app'
    });
</script>
```

---
## 템플릿
- vue 프로그램의 컴포넌트가 사용하는 HTML DOM 문서 조각이다.
- 템플릿은 머스태시 문법과 Vue.js가 제공하는 디렉티브를 사용할 수 있다.
```html
<!-- 일반적인 템플릿 형태 -->
<div>
    <dl>
        <dt>{{ title}}</dt>
        <dd v-text="description"></dd>
    </dl>
</div>
```

## 옵션, 상태, 데이터, 감시자
- vue에서 옵션 설정이나 상태, 데이터 설정 시 객체 리터럴 형태로 작성한다.  
(객체 리터럴은 중괄호({...}) 내에 0개 이상의 프로퍼티를 정의하여 값을 생성하는 표기법이다.)
- 컴포넌트 선언에 el, template이라는 옵션 설정
```javascript
// 루트 컴포넌트(Vue 인스턴스)
new Vue({
    el: '#app'
});

// 전역 컴포넌트
Vue.component('comp', {
    template: '<span>comp</span>'
});

// 지역 컴포넌트
components: {
    'sub_comp': {
        template: '<span>sub_comp</span>'
    }
}
```

- 컴포넌트에 설정한 옵션 함수
```javascript
Vue.component('comp', {
    render: function(h) {
        return h("span", "hello vue")
    }
});
```

- 인스턴스에 설정한 data 옵션 객체
```javascript
new Vue({
    data: {
        msg: "Hello"
    }
});
```

- 그 외의 옵션 : computed, watch, methods 등이 있음
```javascript
new Vue({
    data: {
        name: "soojin"
    },
    // computed 옵션값이 주어지면 컴포넌트에 읽기 전용 속성이 만들어짐
    computed: {
        merge_name: function() {
            return "kim"
        }
    },
    // 감시자
    watch: {
        name: function(new_val, old_val) {

        }
    }
});
```
- watch옵션 값을 설정하여 컴포넌트의 상태값이나 computed속성값이 변경되었음을 감시하는 감시자를 추가할 수 있다.
- 감시할 속성은 옵션 객체의 속성 이름에 기록한다.

---
## 이벤트 핸들링
- DOM에서 발생하는 이벤트와 Vue 컴포넌트에서 발생하는 이벤트를 처리


## 데이터 바인딩과 폼 입력
- 데이터 바인딩은 머스태시 문법 사용이나 v-bind 디렉티브를 사용하여 가능
- 그러나 머스태시 문법 사용 시에는 일부 서버 사이드 템플릿 라이브러리와 겹쳐서 문제가 발생할 수도 있음


## 디렉티브와 보간법
### 디렉티브
- `디렉티브`란 컴포넌트 템플릿에 사용ㅇ한 태그의 기능을 확장하는 데 사용하는 Vue.js의 특수 속성이다.
- v-bind, v-model 등이 있다.

### 보간법
- 컴포넌트 템플릿에 컴포넌트의 상태 값을 보여주거나, HTML 태그의 속성값에 컴포넌트의 상태 값을 설정하는 모든 방법을 말함
- 템플릿에 사용하는 보간법은 문자열 보간, 속성 보간으로 나뉜다.
- 문자열 보간
    + 문자열 보간은 머스태시 문법과 태그의 내용 전체를 변경하는 디렉티브를 사용
- 속성 보간
    + 속성 보간은 HTML태그 속성값으로 컴포넌트 상태 값을 설정할 때 사용한다.   
    + 속성 보간은 v-bind 디렉티브를 사용해야 한다.  
    + v-bind:속성 이름="컴포넌트 상태"와 같은 형태로 사용한다.
        * 예시 : v-bind:id="p_id"

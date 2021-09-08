# Vue.js API

## 옵션
- Vue vm을 인스턴스화할 때는 데이터, 템플릿, 마운트할 DOM 엘리먼트, 메서드, 라이프사이클 콜백 등을 포함하는 객체 리터럴을 인자로 전달해야 함

### 1. 상태 옵션
- computed, methods, watch 옵션을 뺀 주요 상태 옵션

#### 1-1. data 옵션
- 컴포넌트의 상태 값을 관리
- Vue 객체를 초기화한 이후에만 접근해서 참조 가능
```javascript
var vm = new Vue({
    data: {
        counter: 0
    }
});
```

#### 1-2. props 옵션
- 컴포넌트 태그의 속성 이름을 리터럴 배열로 정의
```javascript
Vue.component('props-simple', {
    props: ['msg', 'hello']
})
```
```html
<!-- 위에서 선언한 props 컴포넌트의 사용 -->
<props-simple msg="안녕~" hello="Hello" />
```

### 2. DOM 옵션
- Vue 컴포넌트는 기본적으로 DOM에 연결되어 동작하지만 연결되어 있지 않아도 동작함

#### 2-1. el 옵션
- Vue 인스턴스가 동작할 DOM 엘리먼트를 지정할 때 사용
- el 옵션에 지정한 엘리먼트와 Vue 인스턴스를 연결하는 것을 '마운트'한다고 말함
```javascript
var vm = new Vue({ el: "div#markup" });
```

#### 2-2. template 옵션
- Vue 인스턴스의 마크업으로 사용할 문자열을 지정
```html
<div id="markup"><h1>Hello Markup</h1></div>
```
```js
// 기존 자식 엘리먼트는 삭제되고 template에서 선언한 내용으로 대체됨
var vm = new Vue({
    el: "div#markup",
    template: "<h2>{{ markupText }}</h2>",
    data: {
        markupText: "brand new markup"
    }
});
```

#### 2-4. render 옵션
- 문자열을 전달하는 template 대신 자바스크립트의 프로그래밍 기능을 활용할 때 사용
- render 옵션 안에는 함수를 사용하며, 인자로 createElement 메서드를 전달 받음. 옵션 함수 인자 h로 전달
```js
Vue.component('render-acro', {
    render: function(h) {
        return h(
            'h1', this.$slots.default   // 태그 이름과 자식의 배열
        )
    }
})
```
- render, el, template 옵션이 모두 주어지면 render옵션이 우선순위가 제일 높다.

### 3. 애셋 옵션
- Vue 컴포넌트 생성 시 해당 인스턴스가 사용할 asset에 관한 정보를 전달할 수 있다. 
- directives, filters, compoenets를 애셋 옵션이라 함
### 4. 컴포지션 옵션
- Vue 인스턴스 생성 시 컴포넌트의 상속 관계나 기본 동작에 영향을 미치는 옵션을 설정할 수 있는데 이를 컴포지션 옵션이라 함

#### 4-1. parent 옵션
- 상위 Vue 컴포넌트를 설정하는 데 사용
```js
var parent_vm = new Vue({});
var child_vm = new Vue({
    parent: parent_vm
})
```

#### 4-2. mixins 옵션
- 동일 옵션을 사용하는 Vue 인스턴스를 생성할 때 mixins 옵션을 사용하여 코드 중복을 방지할 수 있다.
- mixins 옵션값은 객체 리터럴로 설정하며 Vue 인스턴스 옵션 및 라이프사이클 이벤트 훅을 담아둔다.
```js
// mixin 객체는 클래스가 아니므로 초기화해서 쓸 수 없다.
var mixin = {
    created: function () { console.log(1)}
};

// 이 경우 콘솔에 1 출력 후 3 출력
var vm = new Vue({
    created: function () { console.log(3)},
    mixins: [mixin] // mixin 객체 전달
});
```

### 5. 기타 옵션
#### 5-1. name 옵션
- 브라우저에 루트 vm의 이름으로 출력될 텍스트를 지정
```js
var vm = new Vue({
    name: "Vue VM Root Component",
    el: "div#hello"
});
```

#### 5-2. delimeters 옵션
- Vue 컴포넌트의 상태 값을 화면에 출력할 때 사용하는 보간기호를 바꿀 수 있다.
```js
// 이렇게 바꿀 경우 템플릿에서 
// <div>{{hello}}</div> 가 아닌 <div>${hello}</div>로 사용한다
var vm = new Vue({
    delimiters: ['${','}']
});
```

#### 5-3. model 옵션
- 사용자화 컴포넌트를 v-model 디렉티브와 함께 사용할 때 prop 속성과 컴포넌트 이벤트를 사용자화할 수 있도록 도와준다.

```이 부분은 잘 이해가 안가서 다른 책으로 봐야할 듯```

---
## 인스턴스 속성



---
## 인스턴스 이벤트 메서드

---
## 라이프사이클 이벤트 훅


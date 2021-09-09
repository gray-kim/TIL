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
```이 부분은 잘 이해가 안감```

---
## 인스턴스 속성
- Vue 인스턴스 생성 시 제공하지 않은 data 옵션의 상태는 인스턴스 생성 후에는 더 추가할 수 없다.
- data옵션에 직접 접근하는 대신 추가 관문(프록시)를 통해 개발자가 변경하는 데이터를 추적할 수 있도록 한다.
- 인스턴스 속성은 Vue 인스턴스가 메모리에 적재되어 있는 동안만 사용할 수 있는 인스턴스 고유의 값이 있는 객체이다.


| 인스턴스 속성 이름 | 특징  |
| ------| ------------ |
| vm.$data | vue 인스턴스 생성 시 전달한 data 옵션 객체 |
| vm.$props | 컴포넌트 태그 속성을 전달받은 후 이를 참조하거나 수정할 때 사용 |
| vm.$el | vue 인스턴스가 마운트한 루트 DOM 엘리먼트 객체를 참조 |
| vm.$options | vue 인스턴스 생성 시 임의로 추가한 옵션 참조할 때 사용 |
| vm.$root | vue 인스턴스의 최상위 부모 인스턴스 반환 시 사용 |
| vm.$parents | vue 인스턴스의 부모 컴포넌트에 접근할 때 사용 |
| vm.$children | vue 인스턴스의 자식 컴포넌트에 접근할 때 사용 |
| vm.$slot | 프로그램에서 접근할 수 있는 콘텐츠 슬롯에 콘텐츠를 배포할 때 사용 |
| vm.$scopedSlots | 범위가 지정된 슬롯을 다룰 때 사용 |
| vm.$refs | vue 인스턴스가 관리하는 자식 DOM 엘리먼트나 다른 컴포넌트 참조 시 사용 |
| vm.$isServer | 현재 동작 중인 vue 인스턴스가 서버에서 실행 중인지 여부를 판단 결과는 boolean |

---
## 인스턴스 이벤트 메서드
- vue.js 는 Vue vm 사이의 상태를 공유하고 관리하는 Vuex란 라이브러리를 제공
- 해당 라이브러리 대신 차선책으로 Vue vm을 사용할 수 있음
- 상태 공유로만 사용하는 Vue vm을 이벤트 버스라 한다.

| 인스턴스 이벤트 메서드 이름 | 특징  |
| ---------| ------------ |
| vm.$on | vue 인스턴스에서 발생하는 이벤트 처리 |
| vm.$once | vue 인스턴스에서 발생하는 이벤트를 한번만 처리 |
| vm.$off | vue 인스턴스의 이벤트 리스너가 더 동작하지 않게살 때 사용 |
| vm.$emit | vm에 이벤트를 발생시키는데 사용 |

---
## 라이프사이클 이벤트 훅
- vue.js 프로그램의 모든 과정을 라이프사이클이라 함
- 라이프사이클 각 단계별 이벤트가 발생하고 함수를 전달 할 수 있는데 이를 이벤트 훅이라 한다.

[lifecycle](https://kr.vuejs.org/images/lifecycle.png)
![lifecycle](https://kr.vuejs.org/images/lifecycle.png)

```js
var vm = new Vue({
    el: "div#hollo",
    data: {
        hello: 'vue.js'
    },
    beforeCreate: function() {
        // data 옵션 객체와 이벤트 초기화 하기 전 실행
        // 옵션의 라이프사이클 추적에 도움
        console.log('beforeCreate 이벤트 발생');
    },
    created: function() {
        // 이벤트 초기화 단계에서 호출
        // 엘리먼트에 마운트 하기 전에 발생하므로 vm.$el 속성을 사용할 수 없음
        console.log('created 이벤트 발생');
    },
    beforeMount: function() {
        // 초기 렌더링이 일어나기 직전
        // 템플릿이나 렌더링 함수 컴파일한 직후에 실행
        console.log('beforeMount 이벤트 발생');
    },
    mounted: function() {
        // 마운트 단계 들어가기 진전에 호출
        // vm.$el 속성 생성되어 있어 DOM 직접 수정 가능
        console.log('mounted 이벤트 발생');
    },
    beforeDestroy: function() {
        // vm이 메모리에서 소멸되기 직전에 발생
        console.log('beforeDestroy 이벤트 발생');
    },
    destroyed: function() {
        // vm이 메모리에서 완전히 제거된 이후 실행
        console.log('destroyed 이벤트 발생');
    },
    beforeUpdate: function() {
        // 상태값 감시하다 변경되면 DOM을 업데이트 하기 전에 호출
        // beforeUpdate 호출 후 가상 DOM 재렌더링과 수정을 실행
        console.log('beforeUpdate 이벤트 발생');
    },
    updated: function() {
        // 가상 DOM 재렌더링과 수정이 끝난 후 실행
        // 즉 가상 DOM을 브라우저에 반영한 후 발생
        console.log('updated 이벤트 발생');
    },
});
```

```html
<div id="hello">
    <keep-alive>
        <component :is="child1" v-if="show"></component>
    </keep-alive>
</div>

<script>
var Child = {
    template: '<div>Child activated 이벤트 훅 테스트</div>',
    activated: function() {
        // keep-alive 태그로 둘러싼 엘리먼트가 DOM에 렌더링되지 않다가 보이게 되면 호출됨
        console.log('activated 이벤트 발생');
    },
    deactivated: function() {
        // keep-alive 태그로 둘러싼 엘리먼트가 DOM에 렌더링되다가 보이지 않게 되면 호출됨
        console.log('deactivated 이벤트 발생');
    },
}

var vm = new Vue({
    el: "#hello",
    data: {
        child1: Child,
        show: false
    }
})
</script>
```

## 라이프사이클 메서드
- Vue.js 는 4개의 라이프사이클 메서드를 제공함

### 1. vm.$mount
```html
<div id="hello"></div>

<script>
var vm = new Vue({
    template: '<div>{{ msg }}</div>'
    data: {
        msg: "hello vue"
    }
});

vm.$mount();

// vm.$mount 사용 시 인자 전달하지 않으면 vue 인스턴스 내부에 vm.$el 속성을 생성함
// 아래처럼 DOM API를 사용해 화면에 추가 가능
document.querySelector("div#hello").appendChild(vm.$el);

// 인자로 DOM 엘리먼트 제공
var hello_element = document.querySelector("#hello");
vm.$mount(hello_element);

// 두번째 인자 false 시 template 옵션값으로 교체
vm.$mount("#hello", false);
</script>
```

### 2. vm.$nextTick
- 다음 DOM 업데이트 사이클 이후 실행될 콜백을 연기
```js
new Vue({
  // ...
  methods: {
    // ...
    example: function () {
      // 데이터 수정
      this.message = 'changed'
      // 아직 DOM 이 갱신되지 않음
      this.$nextTick(function () {
        // DOM이 이제 갱신됨
        // `this` 가 현재 인스턴스에 바인딩 됨
        this.doSomethingElse()
      })
    }
  }
})
```

### 3. vm.$forceUpdate
- Vue 인스턴스를 강제로 다시 렌더링

### 4. vm.$destroy
- vm을 완전히 제거
- beforeDestroy와 destroyed 훅을 호출
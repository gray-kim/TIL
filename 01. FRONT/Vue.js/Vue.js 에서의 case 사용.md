Vue.js에서는 다음과 같은 케이스를 일반적으로 사용합니다:

1. camelCase (낙타 표기법):
   camelCase는 변수, 메서드, 프로퍼티 등 식별자 이름에 주로 사용됩니다. Vue 컴포넌트의 데이터 속성, 메서드, 컴포넌트 옵션 등에 camelCase를 사용합니다. 예를 들면:

```vue
export default {
  data() {
    return {
      userName: 'John',
      userAge: 30,
    };
  },
  methods: {
    getUserDetails() {
      // ...
    },
  },
};
```

2. kebab-case (케밥 표기법):
   kebab-case는 HTML 속성, 컴포넌트 이름, CSS 클래스 등에 주로 사용됩니다. Vue 컴포넌트의 태그 이름, props 속성, HTML 템플릿에서의 속성 등에 kebab-case를 사용합니다. 예를 들면:

```vue
<template>
  <div class="user-profile">
    <input type="text" v-model="user-name" />
    <custom-component :custom-prop="some-value" />
  </div>
</template>

<script>
export default {
  name: 'user-profile',
  props: ['custom-prop'],
  data() {
    return {
      'user-name': 'John',
    };
  },
};
</script>

<style>
.user-profile {
  background-color: #f0f0f0;
}
</style>
```

3. PascalCase (파스칼 표기법):
   PascalCase는 Vue 컴포넌트 파일의 이름과 싱글톤 컴포넌트 등에 주로 사용됩니다. 컴포넌트 파일의 이름과 컴포넌트의 인스턴스화된 이름에 PascalCase를 사용합니다. 예를 들면:

```vue
// User.vue
<template>
  <div>
    <!-- ... -->
  </div>
</template>

<script>
export default {
  name: 'User',
  // ...
};
</script>
```

각 케이스는 해당하는 컨텍스트에서 가장 일반적으로 사용되는 규칙입니다. 그러나 개발 팀의 코드 스타일 가이드에 따라 케이스를 조정할 수도 있습니다.
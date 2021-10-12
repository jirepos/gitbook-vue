# Directive

Vue는 코어에 포함된 기본 디렉티브 세트(v-model과 v-show) 외에도 사용자 정의 디렉티브를 등록할 수 있다. directive를 사용하면 HTML 요소에 속성처럼 추가하여 요소에 추가적인 처리를 할 수 있다. 예를 들어 태그내에 있는 text의 글자색을 변경하는 처리를 하고 싶다고 할 때 directive를 정의하여 사용할 수 있다.

## 간단한 Directive 만들기

### HighlightDirective.js

src/directive/ 디렉터리에 HighlightDirective.js 파일을 만든다.

```javascript
export const MyHighlightDirective = {
  beforeMount(el, binding, vnode, prevVnode) {
    el.style.background = binding.value
  }
}
```

### main.js

```javascript
import { createApp } from 'vue'
import App from './App.vue'
import { MyHighlightDirective } from './directives/HighlightDirective.js'

createApp(App)
  .directive('highlight', MyHighlightDirective)
  .mount('#app')
```

### App.vue

```javascript
<template>
    <p v-highlight="'red'"> 배경색이 빨간색으로 표시된다.   </p>
</template>

<script setup>
</script>

<style>
</style>
```

## 2.x 문법

Vue 2에서 커스텀 디렉티브는 아래에 나열된 훅을 사용하여, 엘리먼트의 라이프사이클을 대상으로 생성되었습니다. 이는 모두 선택사항입니다:

* bind - 디렉티브가 엘리먼트에 바인딩되면 발생합니다. 한번만 발생합니다.
* inserted - 엘리먼트가 부모 DOM에 삽입되면 발생합니다.
* update - 이 훅은 엘리먼트가 업데이트되었지만, 자식이 아직 업데이트되지 않은 경우 호출됩니다.
* componentUpdated - 이 훅은 컴포넌트와 자식이 업데이트되면 호출됩니다.
* unbind - 이 훅은 디렉티브가 제거되면 호출됩니다. bind처럼 한번만 호출됩니다.

## hook 함수

directive 객체는 여러가지 훅 함수를 제공한다.

* created\
  new. 요소의 속성들 또는 이벤트 리스너들이 적용되기 전에 호출된다.
* bind → beforeMount
* inserted → mounted
* beforeUpdate new! 요소 자체가 갱신되기 전에 호출된다.
* update → 제거됨
* componentUpdated → updated
* beforeUnmount new! 요소가 unmounted 되기 직전에 호출된다.
* unbind → unmounted

Vue 3에서 최종 API는 다음과 같다.

```javascript
const MyDirective = {
  beforeMount(el, binding, vnode, prevVnode) {},
  mounted() {},
  beforeUpdate() {},
  updated() {},
  beforeUnmount() {}, // 신규
  unmounted() {}
}
```

디렉티브 훅은 다음을 전달인자로 사용할 수 있다.

| 파라미터     | 설명                                               |
| -------- | ------------------------------------------------ |
| el       | 디렉티브가 바인딩된 엘리먼트. 이 것을 사용하면 DOM 조작을 할 수 있다.       |
| binding  | 속성을 가진 객체                                        |
| vnode    | Vue 컴파일러가 만든 버추얼 노드. VNode API에 전체 설명이 있다        |
| oldVnode | 전의 버추얼 노드. update와 componentUpdated에서만 사용할 수 있다. |

> `el` 뿐만아니라 모든 전달인자는 읽기 전용으로 사용하여야 합니다. 절대 변경하면 안된다.

### mounted

```javascript
mounted(el, binding, vnode) {
  const vm = binding.instance
}
```

#### binding 객체

binding은 다음의 속성들을 가진다.

| 속성         | 설명                                                                                           |
| ---------- | -------------------------------------------------------------------------------------------- |
| name       | 디렉티브의 이름. v- 프리픽스가 없음                                                                        |
| value      | 디렉티브에서 전달받은 값. 예를 들어 v-my-directive="1 + 1"인 경우 value는 2.                                    |
| oldValue   | 이전 값. updated에서만 사용 가능                                                                       |
| expression | 표현식 문자열. 예를 들어 v-my-directive="1 + 1"이면, 표현식은 "1 + 1" .                                      |
| arg        | 렉티브의 전달인자. 있는 경우에만 존재함. 예를 들어 v-my-directive:foo 이면 "foo" .                                  |
| modifiers  | 포함된 수식어 객체. 있는 경우에만 존재함. . 예를 들어 v-my-directive.foo.bar이면, 수식어 객체는 { foo: true, bar: true }. |

#### Accessing the component instance

Vue 2에서는 component instance는 vnode 인자를 통해서 접근해야 했다. Vue 3에서 instance는 binding 의 일부이다.

```javascript
mounted(el, binding, vnode) {
  const vm = binding.instance
}
```

#### 객체 전달

directive에 값을 전달할 때에 자바스크립트 객체를 binding의 value값으로 전달할 수 있다.

```javascript
<div v-demo2="{ color: 'red', text: 'hello!' }"></div>
```

directive에서는 binding.value.color와 같이 사용하여 값을 꺼내올 수 있다.

```javascript
const MyHighlightDirective = {
  beforeMount(el, binding, vnode, prevVnode) {
    //el.style.background = binding.value
    el.style.background = binding.value.color
  },
}
```

## 동적으로 값을 전달

v-bind를 사용하여 동적으로 값을 전달할 수 있는 data- 속성을 사용해야한다. 내부에서 꺼낼때에는 element.getAttribute("속성명") 또는 'element.dataset.속성명'으로 속성 값을 꺼낼 수 있다. 동적 값은 updated() 훅에서만 제대로 값을 가져올 수 있다.

```javascript
const dateFormat = {
  beforeMount(el, binding, vnode, prevNode) {
    // 여기서는 el.dataset.date 값이 undefiend임 
  },
  mounted(el, binding, vnode, prevNode) {
    // 여기서는 el.dataset.date 값이 undefiend임 
  },
  updated(el, binding, vnode, prevNode) {
    let date = new Date(Number(el.dataset.date))
    el.innerHTML = date.toString()
  }
}

const app = createApp(App)
app.directive('date-format', dateFormat)
app.mount('#app')
```

```html
<template>
  <div>
    등록일: <span  :data-date="model.regDt" v-date-format></span> 
  </div>
</template>
```

## References

[Vue 2 공식사이트](https://kr.vuejs.org/v2/guide/custom-directive.html)\
[Vue 3 공식사이트](https://v3.vuejs-korea.org/ko-kr/guide/migration/custom-directives.html)

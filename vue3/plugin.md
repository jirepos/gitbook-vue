# Plugin

Plugin은 객체 또는함수를 install()메소드를 통해 제공한다. 플러그인에 대해 엄격하게 정의 된 범위는 없다.일반적으로 플러그인이 유용한 시나리오는 다음과 같다.

* 약간의 전역 메소드 또는 속성 추가
* 하나 이상의 글로벌 에셋 추가 : 디렉티브 / 필터 / 트랜지션 등.
* 글로벌 mixin으로 일부 컴포넌트 옵션 추가
* 일부 전역 인스턴스 메서드를 config.globalProperties에 연결하여 추가.
* 가지고 있는 API를 제공하면서 동시에 위의 일부 조합을 주입하는 라이브러리

## 간단한 Plugin

애플리케이션에 추가 될 때마다 install 메소드가 객체 인 경우 호출된다. function인 경우 함수 자체가 호출된다. 두 경우 모두 Vue의 createApp에서 생성 된 app객체와 사용자가 전달한 옵션 두 개의 매개 변수를 받는다. provide() 를 사용하여 노출한다.

### MyPlugin.js

```javascript
export default { 
  install: (app, options) => {

    const print = (msg) => {
      console.log(msg)
    }
    
    app.provide('print', print)

  }
}
```

### main.js

createApp()을 사용하여 Vue 앱을 초기화 한 후 use() 메서드를 호출하여 애플리케이션에 플러그인을 추가 할 수 있다.

```javascript
import { createApp } from 'vue'
import App from './App.vue'
import MyPlugin from './plugins/MyPlugin.js'

createApp(App)
  .use(MyPlugin)
  .mount('#app')
```

### App.vue

provide()를 사용했다면 inject()를 사용하여 함수를 쓸 수 있다.

```html
<template>
    <div>test</div>
</template>

<script setup>

import { inject }  from 'vue' 

const print = inject("print")
const statusChanged = (param) =>  {
  console.log(param.status)
}

print("Plugin print()")

</script>

<style>
</style>
```

## Plugin 확장

app 객체에 접근 할 수 있으므로 mixin 및 directive 사용과 같은 다른 모든 기능을 플러그인에서 사용할 수 있다.

### MyPlugin.js

```javascript
export default { 
  install: (app, options) => {

    const print = (msg) => {
      console.log(msg)
    }

    const ColorDirective = {
      beforeMount(el, binding, vnode, prevVnode) {
        el.style.background = binding.value
      }
    }

    app.directive("colordir", ColorDirective)
    app.provide('print', print)
  }
}
```

### App.vue

```html
<template>
    <p v-highlight="'red'"> 배경색이 빨간색으로 표시된다.   </p>
    <p v-colordir="'yellow'"> 배경색이 노란색으로 표시된다.   </p>
</template>

<script setup>

import { inject }  from 'vue' 

const print = inject("print")

const statusChanged = (param) =>  {
  console.log(param.status)
}

print("Plugin print()")

</script>

<style>
</style>
```

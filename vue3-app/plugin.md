# Plugin 사용

CSS, Image와 같은 정적 자원 들은 때로는 직접 서버에 요청하는 코드를 작성하겠지만 서버에 요청 파라미터를 전송하고 응답을 받을 때에는 Ajax를 사용할 것이다. Ajax 요청을 하는 공통 함수를 제공하기 위해 HttpPlugin.js라는 파일을 생성할 것이고 이것은 Vue의 PlugIn으로 개발할 것이다.

* Plugin 파일은 대문자로 시작하고 새로운 단어와 단어는 대문자로 구분한다.

## Plugin 정의

/vue/src/assets 디렉터리에 js 디렉터리를 만든다. 그 안에 HttpPlugin.js 파일을 생성한다.

```shell
📒 project_root
   📒 vue
      📒 src
         📒 assets 
            📒 js
               📄 HttpPlugin.js
```

HttpPlugin.js 파일을 다음과 같이 작성한다.

```javascript

export default {
  /**
   * 
   * @param {Object} app  createApp()에서 생성된 App 객체 
   * @param {Object} options 사용자가 전달한 옵션 
   */
  install: (app, options) => { 
    // for debug
    console.log('http plugin instellaed')
    
  }
}
```

## Plugin 사용

/vue/src/main.js 파일을 열고 HttpPlugin.js 파일을 import한다.

```
import { createApp } from 'vue'
import App from './App.vue'
import Http from './assets/js/HttpPlugin.js'
```

app.use()를 사용하여 객체를 등록한다.

```
const app = createApp(App)
app.use(Http)
app.mount('#app')
```

main.js의 전체코드는 다음과 같다.

```
import { createApp } from 'vue'
import App from './App.vue'
import Http from './assets/js/HttpPlugin.js'

const app = createApp(App)
app.use(Http)
app.mount('#app')
```

브라우저를 열고 index.html을 호출하면 콘솔에 다음과 같은 문장이 출력될 것이다.

```shell
http plugin instellaed
```

http.js에 hello라는 함수를 하나 다음과 같이 정의한다.

```javascript
const hello = () => { 
  console.log("Hello~~~~")
}
```

이 함수를 다른 컴포넌트에서 사용할 수 있도록 provie() 함수를 사용한다.

```javascript
app.provide('$hello', hello)
```

App.vue에서 http.js에서 생성한 함수를 사용할 수 있도록 inject()를 사용한다. inject를 사용하기 위해서는 inject를 import해야 한다.

```javascript
import { onMounted, inject } from 'vue'
const $hello = inject('$hello')
```

mount 함수를 다음과 같이 작성한다.

```javascript
onMounted(() => { 
  $hello()  
})
```

## Plugin 테스트

이제 브라우저로 index.html 파일을 호출하면 콘솔에 Hello\~가 출력될 것이다. 다음은 App.vue의 전체코드이다.

```
<template>
  <img alt="Vue logo" src="./assets/logo.png" />
  <HelloWorld msg="Hello Vue 3 + Vite" />
</template>

<script setup>
import HelloWorld from './components/HelloWorld.vue'
import { onMounted, inject } from 'vue'
const $hello = inject('$hello')

onMounted(() => { 
  $hello()  
})
</script>

<style>
</style>
```

### 함수에 파라미터 전달

hello() 함수에 파라미터 전달하기 hello() 함수에 파라미터를 전달하려면 hello 함수를 다음과 같이 수정한다.

```javascript
//http.js
const hello = (options) => { 
  console.log(options.name)
}
```

hello() 함수를 호출할 때 파라미터를 전달하도록 App.vue 코드를 수정한다.

```javascript
onMounted(() => { 
  $hello({name: "Latte"})  
})
```

브라우저에서 index.html을 호출하면 Latte가 콘솔에 출력될 것이다.

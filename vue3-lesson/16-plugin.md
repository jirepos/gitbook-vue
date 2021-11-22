# Plugin


플러그인은 일반적으로 전역 수준 기능을 Vue에 추가합니다. 플러그인에는 엄격하게 정의된 범위는 없습니다. 일반적으로 작성할 수있는 플러그인에는 여러 유형이 있습니다.


* 약간의 전역 메소드 또는 속성 추가
* 하나 이상의 글로벌 에셋 추가 
  

## plugins 디렉터리 

```shell
📒 project_root
   📒 vue
      📒 src
         📒 plugins 
            📄 MyPlugin.js
```               


## Plugin 생성 
```jsx
// 상수 정의
const MIMEType = {
  json: "application/json" ,
  text: "text/plain"
}


export default {
  /**
   * 
   * @param {Object} app  createApp()에서 생성된 App 객체 
   * @param {Object} options 사용자가 전달한 옵션 
   */
  install: (app, options) => { 
    // 상수 추가 
    app.provide("MIMEType", MIMEType);
  }
}

```

## Plugin 등록

main.js에 use()를 사용하여 등록한다. 

```jsx
import MyPlugin from './plugins/MyPlugin.js'
const app = createApp(App)
app.use(MyPlugin)   // plugin 등록
app.mount('#app')
```

## Plugin 사용 
```html
<template>
  <div>
    <h2>Plugin</h2>
  </div>
</template>
<script>
import { defineComponent, inject } from 'vue'

export default defineComponent({
  setup() {
    const MIMEType = inject("MIMEType") // INJECT를 사용하여 상수 가져온다
    console.log(MIMEType.json);
  },
})
</script>
```


## 함수 정의
```jsx
    app.provide("myFunc", (param) => {
      console.log(param);
    });

```
inject를 사용하여 함수 주입 후에 사용할 수 있다. 
```jsx
    const myFunc = inject("myFunc");
    myFunc("hello");
```




## directive 정의
Plugin에서는 directive를 정의하여 추가할 수 있다. 
```jsx
  // 2. 전역 에셋 추가
  app.directive('my-directive', {
    bind (el, binding, vnode, oldVnode) {
      // 필요한 로직 ...
    }
    ...
  })
```  



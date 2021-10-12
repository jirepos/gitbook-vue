# How-to

## JSON을 변수에 할당하기

```javascript
export default {
  data() {
    return {
      user: null,
    };
  },
  mounted() {
    this.$axios.get('/user.json').then( res => {
      this.user = res.data; // json을 변수에 할당
    });
  },
};
</script>
```

> 한편 자바스크립트에서도 문장의 끝에 세미콜론을 반드시 붙이지 않아도 된다. 하지만 파이썬의 방식과는 차이가 있다. 파이썬에서는 세미콜론을 붙이지 않는 것이 기본이지만, 자바스크립트에서는 세미콜론을 붙이는 것이 기본이다. 자바스크립트에서는 엄연히 (개행이 아니라) 세미콜론으로 문장의 끝을 구별한다. 하지만 실제로는 세미콜론을 붙이지 않더라도 인터프리트 과정에서 구문 오류가 발생하지는 않는다. 그것은 인터프리터가 ‘문장의 끝이라고 생각되는 지점’에 세미콜론을 자동으로 붙여주기 때문이다.

## window.open

### window.open 사용하는 방법

window.open을 사용하여 간단히 팝업을 열 수 있다.

```javascript
onYourfunction = () => { 
  window.open("http://wwww.naver.com")
}
```

### form.submit 사용하는 방법

template에서 fome 요소를 만들고 event handler 함수에서 submit()한다.

```javascript
<template>
  <form action="http://www.naver.com" target="_blank" method="GET" ref="form1"></form>
</template>
<script setup>
import { ref, onMounted } from 'vue' 

const form1 = ref(null)  // 폼 요소의 참조 구하기 

const onOpenWindow = () => { 
  form1.value.submit()  // submit할 때는 form1.value.submit()으로 사용
}
</script>
```

### window open/close

```javascript
<script setup>
let windowRef = null

const onOpenWindow = () => { 
  windowRef = window.open("http://www.naver.com") // naver를 새창으로 띄운다 
}
const onCloseWindow = () => { 
  windowRef.close()
}

</script>
```

### popup window에서 opener의 함수 호출하기

Poupup 윈도우에서 호출될 함수를 하나 만든다.

```javascript
<script setup> 
const onCalledOutside = () => { 
  console.log("this is called outside.2222")
}
</script>
```

onMounted()에서 window의 변수를 만들고 이 함수의 참조를 설정한다.

```javascript
<script setup>

onMounted(() => {
  window.outsideCall = onCalledOutside // 
})
</script>
```

Poupu에서 opener의 함수를 호춣한다.

```javascript
  <script>
    window.addEventListener('load', () => {
      let element = document.getElementById("myspan")
      console.log(element)
      element.addEventListener("click", () => {
        window.opener.outsideCall() // opener
      })
    })
  </script>
```

## Vue에서 import 경로 간단히 설정하기

Vue를 개발하다보면 아래와 같이 import 경로가 복잡해 질 수 있다.

```javascript
import Component from '../../../../components/Component.vue'
```

간단히 하기 위해 Vite를 사용하는 경우 vite.config.js 파일에 alias를 설정하여 간단히 import할 수 있다.

```javascript
import Component from '@/components/Component.vue'
```

vite.config.js 파일에 alias를 다음과 같이 설정한다. '@'을 /src에 대한 별칭으로 사용한다.

```javascript
// vite.config.js
import { defineConfig } from 'vite'
import vue from '@vitejs/plugin-vue'
import path from 'path' 

// https://vitejs.dev/config/
export default defineConfig({
  mode: 'development',   // build를 위해서는 production을 설정하고 개발시에는 development를 사용
  plugins: [vue()],
  resolve: {
    alias: {
      '@': path.resolve(__dirname, '/src')
    }
  }
})
```

Vue 컴포넌트에서 다음과 같이 import한다.

```javascript
<template>
  <div>
    <div>
      <common-vue/>
    </div>
  </div>
</template>
<script setup>
import CommonVue from '@/components/biz/common/Common.vue'
</script>
```

## References

[Vue에 경로 별칭](https://imkh.dev/vue-alias-path/)

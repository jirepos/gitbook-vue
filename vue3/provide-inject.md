# Provide와 Inject

Provide는 부모-자식 컴포넌트 구조에 파이프라인을 만들어주는 것으로 Vue.js 공식문서에는 의존성 주입이라는 표현을 쓰고 있다. provide()를 통해 opt 객체를 제공한다.

## App.vue

```html
<template>
    <p v-highlight="'red'"> 배경색이 빨간색으로 표시된다.   </p>
    <p v-colordir="'yellow'"> 배경색이 노란색으로 표시된다.   </p>
    <inject-comp></inject-comp>
</template>

<script setup>
import InjectComp from './components/InjectComp.vue'
import {  reactive,  provide  }  from 'vue' 

const opt = reactive({
  name: "Latte"
})

provide("opt", opt) 

</script>

<style>
</style>
```

## InjectComp.vue

inject()를 통해 opt를 주입하여 사용한다.

```html
<template>
  <div>
    {{opt.name}}
  </div>
</template>
<script>
import { defineComponent, inject , ref, reactive } from 'vue'

export default defineComponent({
  setup() {

    const opt = inject('opt')
    return { 
      opt 
    }
  },
})
</script>
```

기본적으로 provide/inject는 반응형을 지원하지 않는다(not reactive). 하지만 반응형을 구현할 수 있는 방법이 있다. 부모가 속성을 제공(provide)할 때 감시중인(observed) 객체를 전달하면 된다.

> provide와 inject는 주로 고급 플러그인/컴포넌트 라이브러리를 위해 제공된다. 일반 애플리케이션 코드에서는 사용하지 않는 것이 좋다.

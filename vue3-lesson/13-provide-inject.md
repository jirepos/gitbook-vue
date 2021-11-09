# Provide와 Inject

Provide는 부모-자식 컴포넌트 구조에 파이프라인을 만들어주는 것으로 Vue.js 공식문서에는 의존성 주입이라는 표현을 쓰고 있다. provide()를 통해 변수를 제공한다.

## Main.comp
```html
<template>
  <div>
    Main
    <br>
    <sub-comp></sub-comp>
    <child-comp></child-comp>
  </div>
</template>


<script>
import { defineComponent, ref,  provide } from 'vue';
import SubComp from '@/components/c13-inject/SubComp.vue';
import ChildComp  from '@/components/c13-inject/ChildComp.vue';

export default defineComponent({
  components: {
    SubComp , 
    ChildComp
  },
  setup() {
    const mainVal = ref("hello")
    provide("mainVal",  mainVal);
  },
})
</script>

```
## SubComp.vue
```html
<template>
  <div>
    <h1>Sub Component</h1>
    {{mainVal}}
  </div>
  
</template>
<script>
import { defineComponent, inject } from 'vue'

export default defineComponent({
  setup() {
    const mainVal = inject("mainVal")    
    return { 
      mainVal 
    }
  },
})
</script>
```



기본적으로 provide/inject는 반응형을 지원하지 않는다(not reactive). 하지만 반응형을 구현할 수 있는 방법이 있다. 부모가 속성을 제공(provide)할 때 감시중인(observed) 객체를 전달하면 된다.

> provide와 inject는 주로 고급 플러그인/컴포넌트 라이브러리를 위해 제공된다. 일반 애플리케이션 코드에서는 사용하지 않는 것이 좋다.

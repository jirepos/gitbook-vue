# Vue 2와 Vue3의 차이 

## Vue2에서의 Component 작성 방식(Options API)
* data() 함수에서 데이터처리 로직 작성 
* method 객채에서 함수 작성

```javascript
export default {
    props: {
      title: String
    },
    data () {
      return {
        username: '',
        password: ''
      }
    },
    methods: {
      login () {
        // login method
      }
    }
  }
```

## Vue3에서의 작성 방식(Composition API)

* Composition API 사용 
  * Composition API 작업을 시작하려면 실제로 사용할 수 있는 장소가 필요하다. Vue 컴포넌트에서 이 위치는 setup이라고 한다.
  * setup 컴포넌트 옵션은 컴포넌트가 생성되기 전에, props가 한 번 resolved 될 때 실행된다.
  * Composition API의 진입점 역할을 한다.

**Vue 3의 장점**
https://www.samsungsds.com/kr/insights/vue_js_3.html


```javascript
<template>
  <div>
  </div>
</template>
<script>

import { onMounted, ref, reactive, inject, defineComponent } from "vue";

export default defineComponent({
  components : {
  },
  props {

  },
  setup(props, { emit } ) {
    return { 
    }
  },
});
</script>
<style scoped>

</style>
```

https://v3.ko.vuejs.org/guide/typescript-support.html#vue-components-%E1%84%8C%E1%85%A5%E1%86%BC%E1%84%8B%E1%85%B4


### Composition API를 왜 쓰냐? 

* 논리적인 관점 단위로 개발을 하려고 해도 이 옵션의 규칙을 지켜야하고 더 많은 논리 주제가 추가될 수록 코드의 양이 많아져 가독성이 떨어지고 유지보수성이 낮아집니다
* 동일한 논리적 관심사와 관련있는 코드를 함께 배치할 수 있다면 더 좋을 것입니다. 이것이 바로 Composition API가 할 수 있는 일입니다.


자세한 사항은 아래의 URL을 참고한다. 


https://v3.ko.vuejs.org/guide/composition-api-introduction.html#%E1%84%8F%E1%85%A5%E1%86%B7%E1%84%91%E1%85%A9%E1%84%8C%E1%85%B5%E1%84%89%E1%85%A7%E1%86%AB-api%E1%84%80%E1%85%A1-%E1%84%91%E1%85%B5%E1%86%AF%E1%84%8B%E1%85%AD%E1%84%92%E1%85%A1%E1%86%AB-%E1%84%8B%E1%85%B5%E1%84%8B%E1%85%B2



### setup() function 


```javascript
const MyComponent = {
  setup(props, context) {
    // context.attrs    
    // context.slots    
    // context.emit    
    // context.parent    
    // context.root
  }
}
```




**context의 퍼러퍼티들**
* attrs 
컴포넌트에 전달된 non-prop 속성
* slots
모든 템플릿 슬롯의 렌더링 기능을 가진 객체 
* emit
컴포넌트가 이벤트를 방출하는 방법
* parent
* root



```javascript
const MyComponent = {
  setup(props, { attrs, slots, emit, expose }) {
    function onClick() {
      attrs.foo // automatically update latest value
    }
  }
}
```


### TypeScript 지원 

컴포넌트의 script 부분의 언어가 TypeScript로 되어있는지 확인하세요:

```javascript
<script lang="ts">
  ...
</script>
```

#### Vue Components 정의 
TypeScript에서 Vue component내의 타입을 올바르게 추론하려면, 전역 메서드 defineComponent를 통해 component를 정의 해야합니다:


```javascript 
import { defineComponent } from 'vue'

const Component = defineComponent({
  // type inference enabled
})
```



### 구조 분해 할당(Destructuring)
setup 함수의 시그네처에서 사용한 방법으로 JavaScript의 Destructuring 을 이해할 필요가 있다. 

```javascript
var o = {p: 42, q: true};
var {p, q} = o;

console.log(p); // 42
console.log(q); // true
```

자세한 사항은 아래의 MDN 사이틀흘 참고한다. 
https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Operators/Destructuring_assignment



### ref, reactive 

Vue 3.0 에서 새로운 ref 펑션을 사용하여 어디서나 변수를 반응성있도록 만들 수 있다. 

```javascript
import { ref } from 'vue'

const counter = ref(0)
```

ref는 전달인자를 받고, 반응성 변수의 값에 접근하거나 변경할 수 있는 value 속성을 가진 객체를 반환한다. 
```javascript
import { ref } from 'vue'

const counter = ref(0)

console.log(counter) // { value: 0 }
console.log(counter.value) // 0

counter.value++
console.log(counter.value) // 1
```


> 다시말해, ref 는 값에 반응형 참조를 만듭니다. 참조 작업의 개념은 Composition API 전체에서 종종 사용될 것입니다.


객체인 경우에는 reactive를 사용한다. 
```javascript
import { reactive } from 'vue'

const user = reactive({
  name: "Kim" 
})

user.name = "Park" 
```

사용방법

```html
<div>
  {{count}}  <!-- ref -->
  {{user.name}} <!-- reactive -->
</div>
```


### toRefs 

reactive() 를 사용하여 생성한 객체를 toRefs()를 사용하여 ref()를 사용한 객체로 만들 수 있다. 
```javascript
const user = reactive({ 
  name: "Kim", 
  age: 10 
})

return { 
  toRefs(user)
}
```
사용방법
```html
<div>
  {{name}} {{age}}
</div>
```





## \<script setup\> 타입
Single File Component에서 새로운 Script Type인 \<script setup\>이 있다. 아직은 실험적인 상태이며 3.x 버전을 목표로 한다.
\<script\> 태그의 최상의 레벨의 bindings들을 template에 노출한다.


https://v3.vuejs.org/api/sfc-script-setup.html




```javascript
<script setup>
  // imported components are also directly usable in template
  import Foo from './Foo.vue'
  import { ref } from 'vue'

  // write Composition API code just like in a normal setup()
  // but no need to manually return everything
  const count = ref(0)
  const inc = () => {
    count.value++
  }
</script>

<template>
  <Foo :count="count" @click="inc" />
</template>
```














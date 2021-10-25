# v-model의 원리 


v-model 속성은 v-bind와 v-on의 기능의 조합으로 동작한다. 매번 사용자가 일일이 v-bind와 v-on 속성을 다 지정해 주지 않아도 좀 더 편하게 개발할 수 있게 고안된 문법이다. 앞에서 살펴본 코드를 아래와 같이 변경하더라도 동일하게 동작한다.

* **v-model 속성은 v-bind와 v-on의 기능의 조합으로 동작**

기본적으로 컴포넌트 상의 v-model는 modelValue를 props처럼, update:modelValue를 이벤트처럼 사용한다. 

커스텀 컴포넌트에 v-model 바인딩을 해보겠다. 

* props에 modelValue 를 정의한다. 
* value에 modelValue 값을 사용한다. 
* input이벤트에 핸들러를 생성하고 
* update:modelValue 이벤트를 emit한다. 

```html
<template>  
  <div>
    <input type="text" :value="modelValue" @input="onInput">
  </div>
</template>
<script>
export default {
  props: ["modelValue"],
  emits: ['update:modelValue'],
  setup(props, { emit } ) {
    const onInput = (evt) => {
      emit('update:modelValue', evt.target.value)
    }
    return { 
      onInput
    }
  },
}
</script>
```

부모컴포넌트에서 v-model 디렉티브를 사용하여 변수와 바인딩한다. 

```html
<script setup>

import { ref } from 'vue'

import MyComp from '@/components/c06/MyComp.vue'

const title = ref('hello')

</script>

<template>
  <div>
    <my-comp v-model="title"></my-comp>
    {{title}}
  </div>
</template>

<style>
</style>
```


이 때, v-model에 전달인자를 넘겨줌으로써 이 이름(modalValue)을 변경할 수 있습니다:




```html
<my-component v-model:foo="bar"></my-component>
```
이 경우, 자식 컴포넌트는 foo를 prop으로, 동기화 이벤트에 대해서는 update:foo를 emit하도록 상정합니다:

```javascript
const app = Vue.createApp({})

app.component('my-component', {
  props: {
    foo: String
  },
  template: `
    <input
      type="text"
      :value="foo"
      @input="$emit('update:foo', $event.target.value)">
  `
})
```


### title Prop를 v-model로 바인딩 

```html
<template>  
  <div>
    <input type="text" :value="title" @input="onInput">
  </div>
</template>
<script>
export default {
  props: [ 'title' ],
  emits: ['update:title'],
  setup(props, { emit } ) {
    const onInput = (evt) => {
      emit('update:title', evt.target.value)
    }

    return { 
      onInput
    }
  },
}
</script>
```

```html
<script setup>

import { ref } from 'vue'

import MyComp from '@/components/c06/MyComp.vue'

const title = ref('hello')

</script>

<template>
  <div>
    <my-comp v-model:title="title"></my-comp>
    {{title}}
  </div>
</template>

<style>
</style>
``


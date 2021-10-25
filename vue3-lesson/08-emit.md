# 커스텀 이벤트 


컴포넌트 및 props와는 달리, 이벤트는 자동 대소문자 변환을 제공하지 않습니다. emit할 이벤트의 이름은 자동 대소문자 변환을 사용하는 것 대신 정확한 이름을 사용하여야 합니다. 예를 들어, 아래와 같이 camelCase로 작성된 이벤트를 emit하는 경우:

```javascript
this.$emit('myEvent')
```

아래와 같이 kebab-case로 이벤트를 청취하는 경우 아무런 일도 일어나지 않습니다:
```html
<!-- 작동안함 -->
<my-component @my-event="doSomething"></my-component>
```

컴포넌트 및 props와는 다르게 이벤트 이름은 자바스크립트 변수나 속성의 이름으로 사용되는 경우가 없으며, 따라서 camelCase나 PascalCase를 사용할 필요가 없습니다


## 커스텀 이벤트 정의 
emits 옵션을 사용하여 커스텀 이벤트를 정의한다. 
```html
<script>
export default {
  props: [ 'title' ],
  emits: ['update'],
  setup(props, { emit } ) {
    console.log(props.title)
  },
}
</script>
```

>만약 emits 옵션을 이용해 네이티브 이벤트(e.g., click)가 재정의 된 경우, 컴포넌트에 정의된 커스텀 이벤트가 네이티브 이벤트를 덮어씁니다.

## 이벤트 생성 
버튼 요소를 하나 생성하고 클릭 이벤트 핸들러를 정의한 다음에 사용자 이벤트 'update'를 생성해 보겠다. 

```html
<template>  
  <div>
    <button @click="onClick">Click</button>
  </div>
</template>
<script>
export default {
  props: [ 'title' ],
  emits: ['update'],
  setup(props, { emit } ) {

    const onClick = () => {
      console.log('clicked')
      emit('update')
    }
    
    return { 
      onClick
    }
  },
}
</script>
````
이벤트를 다음과 같이 작성하여 수신한다. 
```html
<script setup>
import { ref } from 'vue'
import MyComp from '@/components/c06/MyComp.vue'

const onUpdate = () => {
  console.log("updated")
}

</script>

<template>
  <div>
    <my-comp @update="onUpdate" title="hello"></my-comp>
  </div>
</template>

<style>
</style>

```


### 값전달 
다음과 같이 작성하여 값을 전달할 수 있다. 
```html
<template>  
  <div>
    <button @click="onClick">Click</button>
  </div>
</template>
<script>
export default {
  props: [ 'title' ],
  emits: ['update'],
  setup(props, { emit } ) {

    const onClick = () => {
      console.log('clicked')
      emit('update', "hello")
    }
    
    return { 
      onClick
    }
  },
}
</script>
```
이벤트 수신 핸들러 함수에서 파라미터 값을 사용할 수 있다. 
```html
<script setup>
import { ref } from 'vue'
import MyComp from '@/components/c06/MyComp.vue'

const onUpdate = (val) => {
  console.log("updated:" + val)
}
</script>

<template>
  <div>
    <my-comp @update="onUpdate" title="hello"></my-comp>
  </div>
</template>

<style>
</style>

```





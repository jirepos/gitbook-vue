# Computed 와 Watch

## Computed 
다음과 같은 형식으로 작성한다. 

```javascript
const plusOne = computed( [function])
```

## 기초 샘플 
카운트를 표시하고 카운트에 1을 더하는 간단한 Computed 변수를 작성할 것이다. 

먼저, 버튼을 클릭할 때마다 1씩 증가하는 count예제를 다음과 같이 작성한다. 
```html
<template>
  <div>
    {{count}}
    <br>
    <button @click="onClick">Click Me</button>
  </div>
</template>
<script>

import { defineComponent , ref, computed } from 'vue'

export default {

  setup(props, { emit }) {
    
    const count = ref(0) 

    const onClick = () => {
      count.value += 1 
    }

    return {
      count ,
      onClick 
    };
  },
};
</script>
```
button을 클릭할 때 마다 1씩 증가한 값을 볼 수 있다. 

이제 computed를 작성해보자. 
```html
<template>
  <div>
    {{count}}
    <br>
    <button @click="onClick">Click Me</button>
    <br>
    {{plusOne}}
  </div>
</template>
<script>

import { defineComponent , ref, computed } from 'vue'

export default {

  setup(props, { emit }) {
    
    const count = ref(0) 

    const onClick = () => {
      count.value += 1 
    }
    const plusOne = computed ( () => count.value + 1)

    return {
      count ,
      onClick ,
      plusOne
    };
  },
};
</script>
```

> 템플릿 내에 표현식을 넣으면 편리하지만, 간단한 연산을 위한 부분입니다. 템플릿 안에서 너무 많은 연산을 하면 코드가 비대해지고 유지보수가 어렵습니다. 




## Watch 
대부분의 경우 computed 속성이 더 적절하지만, 사용자 지정 감시자(watcher) 가 필요한 경우도 있습니다. 이것이 Vue 가 watch 옵션을 통해 데이터의 변경에 대응하는 방법을 제공하는 이유입니다. 이는 데이터 변경에 대한 응답으로 비동기 혹은 비용이 많이 드는 작업을 수행하려는 경우가 가장 유용합니다.

watch 형식은 다음과 같다. 
```javascript
watch( source, callback, options)
```
**source**
watcher data source는 값을 리턴하는 getter function이거나 직접적으로 ref일 수 있다.


```javascript
const count = ref(0)
watch(() => state.count, (newCount, oldCount) => {
  // do Something
})
```

### 기본 타입
ref()를 사용하는 경우 count를 그대로 넘긴다.
```javascript
// 기본타입 watch 
const count = ref(0)
watch(count, (newCount, oldCount) => {
   console.log("Old Count:" + oldCount)
   console.log("New Count"  + newCount)
})

count.value = 11  // 값이 변하면 watch  실행 
```


### 객체 타입 
reactive()를 사용하는 경우 getter function을 사용한다. state 변수를 그대로 넘기는 것이 아니라 () => state.count와 같이 넘긴다.


```javascript
// 객체 타입 watch
const state = reactive({ count: 0})
watch(() => state.count, (newCount, oldCount) => {
  console.log("Old Count:" + oldCount)
  console.log("New Count"  + newCount)
})
state.count = 13333 // 값이 변하면 watch 실행 
```
전체 코드는 다음과 같다. 
```html
<template>
  <div>
    {{ count }} <br />
  </div>
</template>
<script>
import { defineComponent, ref, computed, watch, reactive } from "vue";

export default defineComponent({
  setup() {
    const count = ref(0);
    watch(count, (newCount, oldCount) => {
      console.log("Old Count:" + oldCount);
      console.log("New Count" + newCount);
    });

    count.value = 11; // 값이 변하면 watch  실행

    const state = reactive({ count: 0 });
    watch(
      () => state.count,
      (newCount, oldCount) => {
        console.log("Old Count:" + oldCount);
        console.log("New Count" + newCount);
      }
    );
    state.count = 13333; // 값이 변하면 watch 실행

    return {
      count,
    };
  },
});
</script>
```







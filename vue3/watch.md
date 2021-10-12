# watch

## computed

템플릿 내에 표현식을 넣으면 편리하다. 하지만 간단한 연산일 때만 이용하는 것이 좋습니다. 너무 많은 연산을 템플릿 안에서 하면 코드가 비대해지고 유지보수가 어렵다. 복잡한 로직이라면 computed 속성을 이용해야 하는 이유이다.

> 그러나, 템플릿에서 표현식은 로직이 분산되므로 가급적 제한하는 것이 좋다. computed 속성의 좋은 점은 종속된 대상인 변수의 값이 변경되지 않는 한 캐싱된다.

```html
<template>
  <div>
    {{count}} <br>
    {{plusOne}}
  </div>
  
</template>
<script>
import { defineComponent , ref, computed } from 'vue'

export default defineComponent({
  setup() {
    const count = ref(1)
    const plusOne = computed(() => count.value + 1)

    return {
      count, plusOne
    }
  },
})
</script>
```

## watch

지정한 대상의 값이 변경될 때마다 정의한 함수가 실행된다.

### watch 형식

```javascript
watch( source, callback, options)
```

**source**\
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

### WatchComp.vue

전체코드는 다음과 같다.

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

## References

[Watch](https://v3.vuejs.org/guide/reactivity-computed-watchers.html#watching-a-single-source)

# jQuery 컴포넌트 사용 

jQuery의 DatePicker를 Vue 컴포넌트로 만드는 방법에 대해서 살펴보자. jQuery가 설치되어 있지 않으면 jQuery를 먼저 설치한다. '외부 라이브러리 사용' 문서를 참고한다. 


```설치
yarn add jquery-datetimepicker
```


## jQuery window 객체에 설정 
main.js에서 window 객체에 $, jQuery 변수를 추가한다. 
```jsx
// main.js 
import jQuery from 'jquery'
window.$ = window.jQuery = jQuery  // $, jQuery 사용할 수 있게 
 
createApp(App)
  .directive('highlight', MyHighlightDirective)
  //.use(router)
  .use(store)
  .use(MyPlugin)
  .mount('#app');
```


## MyDatePicker 컴포넌트 작성
MyDatePicker.vue 파일을 생성한다.  css와 모듈을 import한다.  jQuery UI Plugin은 바인딩 없이 실행만 하므로 import '모듈명' 구문을 사용한다.

```jsx
import "jquery-datetimepicker/jquery.datetimepicker.css";
// jQuery plugin은 변수명 바인딩 없이 모듈만 실행하기 때문에
// 모듈명만 명시하여 import한다. 
import 'jquery-datetimepicker'
```

컴포넌트와 v-model을 바인딩하기 위해서 props에 modelValue를 추가한다. 
```jsx
props: [ "modelValue"],
```

template에 input 요소를 추가한다.  props와 input 요소의 값을 바인딩하기 위해 modelValue prop를 사용한다. 
```html
<input id="datepicker"  type="text"  :value="modelValue" >
```
value 값이 변경되었을 때 부모 컴포넌트에 값을 전달하기 위해서 'update:modelValue' emits를 정의한다. 
```jsx
emits: [ 'update:modelValue'],
```

요소에 접근하여 핸들링하기 위해서 onMounted() 훅 함수를 정의한다. 
```jsx
onMounted( () => { 

})
```
datetimepicker를 적용하기 위해서 datetimepicker()를 호춣한다. 
```jsx
    onMounted(() => {
      jQuery( "#datepicker" )
        .datetimepicker();       // input 요소 바인딩
    })
```
main.js에서 window 객체에 jQuery 추가했기 때문에 다음과 같이 사용이 가능하다. 

> window는 전역 객체이다. 



```jsx
window.jQuery( "#datepicker" ).datetimepicker();
// or
window.$( "#datepicker" ).datetimepicker(); 
// or
$( "#datepicker" ).datetimepicker(); 
```

사용자가 달력에서 값을 선택하면 change 이벤트가 발생한다. change 이벤트 핸들러를 다음과 같이 구현한다. 값이 변경된 것을 알리기 위해서 emit() 사용한다. 
```jsx
  setup(props, { emit }) {
    onMounted(() => {
      jQuery( "#datepicker" )
        .datetimepicker()       // input 요소 바인딩
        .on('change', () => {   // change 이벤트 바인등
          let rdate = jQuery( "#datepicker" ).val()
          emit('update:modelValue', rdate)  // 외부로 이벤트 발생 
        })
    })
    return { }
  },
```


아래는 MyDatePicker.vue의 전체코드이다. 
```html
<template>
  <input id="datepicker"  type="text"  :value="modelValue" >
</template>
<script>

import "jquery-datetimepicker/jquery.datetimepicker.css";
import { defineComponent, onMounted } from 'vue'
// jQuery plugin은 변수명 바인딩 없이 모듈만 실행하기 때문에
// 모듈명만 명시하여 import한다. 
import 'jquery-datetimepicker'

export default defineComponent({
  props: [ "modelValue"],
  emits: [ 'update:modelValue'],
  setup(props, { emit }) {

    onMounted(() => {
      jQuery( "#datepicker" )
        .datetimepicker()       // input 요소 바인딩
        .on('change', () => {   // change 이벤트 바인등
          let rdate = jQuery( "#datepicker" ).val()
          emit('update:modelValue', rdate)  // 외부로 이벤트 발생 
        })
    })
    return { 
    }
  },
})
</script>
```



## 부모 컴포넌트 작성 
위에서 작성한 MyDatePicker 컴포넌트를 사용하기 위해서  vue 파일을 생성한다. MyDatePicker를 import하고 v-model 바인딩을 위해서 const를 생성한다.  my-date-picker 컴포넌트에 v-model="regDt"를 사용한다. 

```html
<template>
  <div>
     <my-date-picker v-model="regDt"></my-date-picker>
     <br>
     {{regDt}}
  </div>
</template>
<script>
import { defineComponent , ref,  onMounted } from 'vue'
import MyDatePicker   from '@/components/c17/MyDatePicker.vue'

export default defineComponent({
  components: {
    MyDatePicker 
  }, 
  setup() {
    const regDt = ref('')
    return { 
      regDt 
    }
  },
})
</script>

```




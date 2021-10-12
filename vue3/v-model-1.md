# v-model 원리

v-model이라는 속성을 사용하면 입력 값이 자동으로 뷰 데이터 속성에 연결된다.

```javascript
<template>
  <input v-model="inputText"/>
</template>
<script setup>
import { ref, reactive } from 'vue' 

const  inputText = ref("a")
</script>
```

## v-on의 동작방식

v-model 속성은 v-bind와 v-on의 기능의 조합으로 동작한다. 매번 사용자가 일일이 v-bind와 v-on 속성을 다 지정해 주지 않아도 좀 더 편하게 개발할 수 있게 고안된 문법이다. 앞에서 살펴본 코드를 아래와 같이 변경하더라도 동일하게 동작한다.

```html
<template>
  <input type="text" :value="inputText" @input="updateInput">
</template>
<script>
import { defineComponent , ref } from 'vue'

export default defineComponent({
  setup() {

    const inputText = ref('')
    const updateInput = (evt) => {
      inputText.value = evt.target.value 
      console.log(inputText)
    }
    return {
      inputText, updateInput 
    }
  },
})
</script>
```

* v-bind 속성은 뷰 인스턴스의 데이터 속성을 해당 HTML 요소에 연결할 때 사용한다.
* v-on 속성은 해당 HTML 요소의 이벤트를 뷰 인스턴스의 로직과 연결할 때 사용한다.
* 사용자 이벤트에 의해 실행된 뷰 메서드(methods) 함수의 첫 번째 인자에는 해당 이벤트(event)가 들어온다

빠르게 기능을 구현하고 프로토타이핑 해나갈 때는 v-model을 사용해도 상관없다. 다만, 현재 시점에서는 IME 입력(한국어, 일본어, 중국어)에 대해서 아래와 같은 한계점이 있다.

한글 입력의 경우 한 글자에 대한 입력이 끝나야지만 inputText 데이터가 inputText 박스의 값과 동기화 된다.

위와 같은 v-model의 한계점 때문에 뷰 공식 문서에서는 한국어 입력을 다룰 때 v-bind:value와 v-on:input를 직접 연결해서 사용하는 것을 권고하고 있다.

이렇게 매번 한국어 입력을 처리할 때 v-model 대신에 직접 이벤트와 값을 조합해서 바인딩 하는 것이 귀찮게 느껴질 수 있다. 인풋 컴포넌트를 별도의 컴포넌트로 분리하면 v-model로 편하게 처리할 수 있다.

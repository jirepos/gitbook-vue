# Composition API Quick Start

## Primiteive Type 사용

\<template>에 message 변수의 값을 출력하기 위해 {{message}}를 사용한다. tmp에 message 변수를 사용하여 메시지를 설정한다. 메시지 변수를 객체로 반환하기 위해서 ref()를 사용한다.

```javascript
<template>
  <div>{{message}}</div>
</template>

<script>

import { ref, computed, toRefs } from 'vue'

const tmp = () => { 
  const message = ref("Composition")  // Primitive type을 객체로 변환하기 위해 ref() 사용 
  return { message }   // { variable } 형식으로 반환 
}

export default  {
  setup() {
    const { message } = tmp()   // const { variable } 형식으로 상수 선언 
    console.log(message.value)  // ref()를 사용해서 생성한 변수는  object.value 형태로 값을 참조
    return {
      message: message  // message 변수에 const message 할당 
    }
  }
}
</script>
```

ref() 사용하지 않으면 다음과 같이 message 값을 코드에서 조회할 수 있다.

```javascript
const { message } = tmp()
console.log(message) 
```

## 객체 타입 사용

객체는 reactive() 함수로 감싼 다음에 반환한다.

```javascript
<template>
  <div>{{message}}</div><br>
  <div>{{user.name}}</div>

  </template>

  <script>

  import { ref, computed, toRefs, reactive } from 'vue'

  const tmp = () => {
  const message = ref("Composition")  // Primitive type을 객체로 변환하기 위해 ref() 사용 
  const user = reactive( {   // 객체는 reactive() 함수로 감싼다
  name: "Hong Gildong"
})
  return { message, user }   // { variable } 형식으로 반환 
}


  export default  {
  setup() {
  const { message , user} = tmp()   // const { variable } 형식으로 상수 선언 
  console.log(message.value)  // ref()를 사용해서 생성한 변수는  object.value 형태로 값을 참조
  return {
  message: message,  // message 변수에 const message 할당 
  user: user
}
}
}
  </script>
```

## 함수사용

콘솔에 로그를 출력하는 함사 greet()를 생성하고 return { } 구문에 추가한다. 변수와 마찬가지로 **setup() 함수의 return { } 구문에 정의**한다.

```javascript
<template>
  <div>{{message}}</div><br>
  <div>{{user.name}}</div><br>
  <span @click="greet">콘솔에 로그 출력</span>
</template>

<script>

import { ref, computed, toRefs, reactive } from 'vue'

const tmp = () => { 
  const message = ref("Composition")  // Primitive type을 객체로 변환하기 위해 ref() 사용 
  const user = reactive( {   // 객체는 reactive() 함수로 감싼다
    name: "Hong Gildong"
  })
  const greet = () => { console.log("안녕하세요.") }
  return { message, user, greet }   // { variable } 형식으로 반환 
}


export default  {
  setup() {
    const { message , user, greet } = tmp()   // const { variable } 형식으로 상수 선언 
    console.log(message.value)  // ref()를 사용해서 생성한 변수는  object.value 형태로 값을 참조
    greet()  // 내부에서 함수 사용
    return {
      message: message,  // message 변수에 const message 할당 
      user: user , 
      greet: greet   // greet() 함수 설정
    }
  }
}
</script>
```

## Props의 사용

Composition을 사용하는 컴포넌트를 임포트하는 App.vue에서 props를 사용하여 값을 전달한다.

```html
    <composition-ex-1 my-param="전달된 값"/>
```

Props로 전달받을 변수를 정의하고 Props의 변수를 사용할 때에는 Vue3에서는 props.myParam과 같이 사용한다.

```javascript
<template>
  <div>{{message}}</div><br>
  <div>{{user.name}}</div><br>
  <span @click="greet">콘솔에 로그 출력</span><br>
  <span>Props:{{param}}</span>  
</template>

<script>

import { ref, computed, toRefs, reactive } from 'vue'

const tmp = () => { 
  const message = ref("Composition")  // Primitive type을 객체로 변환하기 위해 ref() 사용 
  const user = reactive( {   // 객체는 reactive() 함수로 감싼다
    name: "Hong Gildong"
  })
  const greet = () => { console.log("안녕하세요.") }
  return { message, user, greet }   // { variable } 형식으로 반환 
}

export default  {
  props: {
    myParam: String
  },
  setup(props) {
    const { message , user, greet } = tmp()   // const { variable } 형식으로 상수 선언 
    console.log(message.value)  // ref()를 사용해서 생성한 변수는  object.value 형태로 값을 참조
    greet()  // 내부에서 함수 사용
    return {
      message: message,  // message 변수에 const message 할당 
      user: user , 
      greet: greet,   // greet() 함수 설정
      param: props.myParam
    }
  }
}
</script>
```

## 사용자 이벤트 emit 사용

아래는 사용자 이벤트에 대한 참조 링크이다. 방문하여 읽어 보는 것이 좋을 것이다.

[A Guide to Vue Event Handling - with Vue3 Updates](https://learnvue.co/2020/01/a-vue-event-handling-cheatsheet-the-essentials/)

부모 컴폰전트에서 @update='afterLogin' 과 같이 이벤트를 잡을 수 있도록 핸들러를 등록한다.

```html
<composition-ex-1 my-param="전달된 값" @update='afterLogin'/>
```

methods에 afterLogin() 함수를 정의한다.

```javascript
  methods: {
    afterLogin() {
      //alert("An user logined.")
      console.log("afterLogin")
    },
  },
```

setup() 함수의 파라미터를 다음과 같이 정의한다.

```
setup(props, { emit }) {

}
```

이벤트를 발생시킬 함수를 작성한다.

```javascript
    const handleUpdate = () => { 
      emit('update', {
        username: 'user1'
      })
    }
```

setup() 함수가 실행될 때 바로 이벤트를 발생시킬 수도 있다.

```javascript
handleUpdate()
```

컴포넌트 내부에서 사용자가 클릭했을 때 이벤트를 발생시키려면 template에 다음과 같이 정의한다.

```html
<span @click="handleUpdate">이벤트 발생</span>
```

전체 코드는 다음과 같다.

```javascript
<template>
  <div>{{message}}</div><br>
  <div>{{user.name}}</div><br>
  <span @click="greet">콘솔에 로그 출력</span><br>
  <span>Props:{{param}}</span>  <br>
  <span @click="handleUpdate">이벤트 발생</span>
</template>

<script>

import { ref, computed, toRefs, reactive } from 'vue'

const tmp = () => { 
  const message = ref("Composition")  // Primitive type을 객체로 변환하기 위해 ref() 사용 
  const user = reactive( {   // 객체는 reactive() 함수로 감싼다
    name: "Hong Gildong"
  })
  const greet = () => { console.log("안녕하세요.") }
  return { message, user, greet }   // { variable } 형식으로 반환 
}


export default  {
  props: {
    myParam: String
  }, 
  setup(props, { emit }) { // 파라미터로  { emit }를 넘긴다. 
    const { message , user, greet } = tmp()   // const { variable } 형식으로 상수 선언 
    console.log(message.value)  // ref()를 사용해서 생성한 변수는  object.value 형태로 값을 참조
    greet()  // 내부에서 함수 사용

    const handleUpdate = () => { 
      console.log(">> handleUpdate")
      emit('update', {
        username: 'user1'
      })
    }
    handleUpdate() // setup() 실행시 이벤트를 바로 발생시킨다.

    return {
      message: message,  // message 변수에 const message 할당 
      user: user , 
      greet: greet,   // greet() 함수 설정
      param: props.myParam,  // Props 사용
      handleUpdate,  // 함수 반환. handleUpdate: handleUpdate와 같이 사용하지 않아도 된다. 
    }
  }
}
</script>
```

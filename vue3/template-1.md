# Template 기초 샘플

## App.vue

```javascript
<template>
  <model-comp></model-comp>
</template>

<script setup>
import BasicComp from './components/BasicComp.vue'

const statusChanged = (param) =>  {
  console.log(param.status)
}

</script>

<style>
</style>
```

## BasicComp.vue

```html
<template>
  <div>

    <!-- 변수와 템플릿 -->
    <div>
      <h1>변수와 템플릿</h1>
      <div>
        <h3>반응형 변수(ref)</h3>
        {{message}}
        <h3>반응형 변수(reactive)</h3>
        {{user.name}}
        <h3>계산 필드(computed)</h3>
        {{count}}
      </div>
    </div>

    <!-- Directive -->
    <div>
      <h1>Directive</h1>
      <div>
        <h3>v-html</h3>
        <div v-html="rawHtml"></div>
        <h3>v-bind:href</h3>
        <a :href="url">Click Me</a>
        <h3>v-bind:src</h3>
        <img :src="imgSrc" alt="" class="src">
        <h3>v-if="seen"</h3>
        <p v-if="seen">이제 저를 볼 수 있어요.</p>
        <h3>동적인자</h3>
        <a v-bind:[attributeName]="url">동적 인자</a>
      </div>
    </div>
    <!-- Event --> 
    <div>
      <h1>Event</h1>
      <input type="button" @click="buttonClick" value="Click this button">
      <input type="button" @click="buttonEmit" value="Emit"> <br>
      <input type="text" @keyup.enter="onEnter" value="">
    </div>
    <!-- CSS -->
    <div>
      <h1>CSS</h1>
      <h3>style</h3>
      <div :style="styleObject">Style Div</div>
      <h3>class</h3>
      <div :class="{active: isActive}">CSS Div</div>
      <h3>Multi-CSS</h3>
      <div :class="{active: isActive, 'text-danger': hasError}">Multi Class</div>
      <h3>Class Object</h3>
      <div :class="classObject">classObject</div>
    </div>

  </div>
</template>
<script>
import { defineComponent, ref , reactive , onMounted , computed } from 'vue'


// 객체를 반환하는 함수를  외부에서 함수 정의 
const gfunc = () => {
  //  반영형 변수 
  const msg = ref('Composition')
  // 반응형 객체 
  const emp = reactive( {
    name: 'Latte'
  })
  // 함수 
  const greet = () => { console.log("Hello, I'm greet.")}
  return { msg, emp, greet }  // 함수 내부에서 정의한 변수를 모아서 객체로 반환 
}

// 외부 함수 
const add = (a,b) => {
  return a + b
}


export default defineComponent({

  // Props 
  props: {
    title: String
  },
  
  setup(props, { emit }) {


    //  반응형 변수
    const message = ref('')
    message.value = "hello"
    // 반응형 객체 변수 
    const user = reactive({
      name: 'Latte'
    })
    
    // 계산된(computed) 속성 
    const cnt = ref(0)
    const count = computed( () => cnt.value  +2 )

    // props 사용 
    console.log(props.title);

    // 객체 분해(Destructuring)
    // setup() 밖에서 정의된 함수가 반환한 객체를 변수로 변경 
    const { msg, emp, greet } =  gfunc()
    console.log(msg.value)
    console.log(emp.name)
    greet()

    // 외부 함수 
    console.log(add(1,2))

   // html
   const rawHtml = ref('')
   rawHtml.value = '<strong>strong</strong>'


    // directive 
    const url = ref("http://www.naver.com")
    const imgSrc = ref("/public/logo.png")
    const seen = ref(false)
    const attributeName = ref("href")

    // event 
    const buttonClick = () => {
      console.log("Button clicked")
    }

    const onEnter = () => {
      console.log("Enter pressed")
    }

    // emit
    const buttonEmit = () => {
      console.log("buttonEmit clicked")
       // 이벤트 발생 
       emit('update', {
          status: 'OK'
       })
    }

    // STYLE 
    // style 
    const styleObject = {
      color: 'red', 
      fontSize: '14px',
      'background-color': 'yellow'
    }
    // css
    const isActive = ref(true) 
    const hasError = ref(true) 
    // multi-class, class object
    const classObject = reactive( {
      active: true, 
      'text-danger': false
    })
    
    // Lifecycle Hook  
    onMounted( () => {
      console.log("mounted")
      console.log(props.title)
    })

    return { message, user, count, rawHtml, seen, url, onEnter,  attributeName, imgSrc, buttonClick
     , buttonEmit, styleObject, isActive, hasError, classObject}
  },
})
</script>
```

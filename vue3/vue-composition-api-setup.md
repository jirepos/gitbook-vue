# Vue Composition API Setup

## Vue setup() function

setup()은 컴포넌트의 인스턴스가 생성될 때 호출된다.

```javascript
const MyComponent = {
  props: {
    name: String
  },
  setup(props, context) {
    console.log(props.name);
    // context.attrs
    // context.slots
    // context.emit
    // context.parent
    // context.root
  }
}
```

위 함수는 두 개의 인자를 가진다. context는 프러퍼티들을 가진다(attrs, slots, emit, parent, root).이것들은 this.$attrs, this.$slots, this.$emit, this.$parent, this.$root에 대응한다.

우리는 또한 분해할 수 있다.

```javascript
const MyComponent = {
  setup(props, { attrs }) {
    
    function onClick() {
      attrs.foo // automatically update latest value
    }
  }
}
```

**this 키워드는 setup() 함수애내에서 쓸 수 없다.** 그래서 this.$emit는 아래와 같이 쓸 수 없다.

```javascript
setup() {
  function onClick() {
    this.$emit // not available
  }
}
```

## \<script setup=""> 타입

Single File Component에서 새로운 Script Type인 \<script setup>이 있다. 아직은 실험적인 상태이며 3.x 버전을 목표로 한다.

\<script> 태그의 최상의 레벨의 bindings들을 template에 노출한다.

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

위의 코드에서 최상위 수준의 함수인 inc와 count는 template에 노출된다. setup() 함수 내부에서 정의하고 리턴할 필요가 없다. setup() 함수를 사용하는 것보다는 편하게 사용할 수 있다.

위 코드가 컴파일되면 다음과 동일하다.

```javascript
<script setup>
  import Foo from './Foo.vue'
  import { ref } from 'vue'

  export default {
    setup() {
      const count = ref(1)
      const inc = () => {
        count.value++
      }

      return {
        Foo, // see note below
        count,
        inc,
      }
    },
  }
</script>

<template>
  <Foo :count="count" @click="inc" />
</template>
```

### props, emits

props는 defineProps()를 사용해서 정의하고 emit는 defineEmit()를 사용해서 정의한다.

```javascript
<script setup>
  import { defineProps, defineEmit } from 'vue'

  // expects props options
  const props = defineProps({
    foo: String,
  })
  // expects emits options
  const emit = defineEmits(['update', 'delete'])
</script>
```

구글링을 해 보면 \<script setup="props">와 같이 setup에 props를 정의하라는 문서들을 많이 볼 수 있다 하지만 다음과 같이 작성해도 정상적으로 동작한다.

```javascript
<script setup="props, { emit }">
</script>
```

### 예제

\<script setup>을 사용한 예제를 살펴 보겠다.

#### App.vue

Setup 컴포넌트를 template에 추가하고 contents prop의 값을 전달했다. Setup 컴포넌트에서 발생한 사용자 이벤트를 처리하기 위해서 @my-event 사용자 이벤트를 추가하고 이벤트를 추가할 handler 함수인 updateChange를 설정한다.

```javascript
<template>
  <SetupTest  @my-event="updateChange" contents="부모에서 값전달"/>
</template>

<script setup>
import SetupTest from './components/example/SetupTest.vue'
// 하위 컴포넌트의 사용자 이벤트 처리
const updateChange = (param) =>  {
  console.log(param) 
}
</script>

<style>
</style>
```

#### SetupTest.vue

SetupTest.vue에서 props는 defineProps()를 사용하여 정의한다.

```javascript
const props = defineProps( {
   contents:String, 
   default: ""
})
```

template에서는 props.contents와 같이 사용한다.

```javascript
<div>{{props.contents}}</div>
```

template에 사용자 정보를 출력할 요소를 다음과 같이 정의한다. 데이터를 반응형으로 만들기 위해서는 reactive()를 사용한다.

```javascript
<script setup>
const data = reactive({
  user: { 
    name: "초기이름",
    age: 0 
  }
</script>
```

주의할 것은 reactive() 사용하지 않으면 data의 값이 변경이 되어도 template에 반영이 되지 않는다. 그리고 reactive()를 사용하여 data에 객체({ })를 설정했는데 이 객체는 다음과 같이 해도 변경이 되지 않는다.

```javascript
data = otherObject 
```

서버에서 응답으로 받을 데이터는 다음과 같다.

```json
{
  "name": "홍길동",
  "age": 88
}
```

응답으로 받은 user 객체로 data.user 객체를 다음과 같이 변경할 수 있다.

```javascript
<script setup>
data.user = res  
</script>
```

사용자가 요소를 클릭했을 때 서버에서 사용자 정보를 받아올 함수에 대한 바인딩을 다음과 같이 작성한다.

```javascript
<span @click="getUserInfo">Click Me!!!!</span><br />
```

template에 있는 요소를 클릭했을 때 이벤트를 처리할 event handler 함수를 작성한다.

```javascript
<script setup>
// event handler section
const getUserInfo = () => {
  let options = {
    method: "GET",
  };

  ajax(urls.URL_GET_USER, options)
    .then((res) => {
      data.user = res  // 여기서 user 객체를 변경한다. 
    })
    .catch((error) => {
      console.log("Error occured.");
      console.log(error); // 오류 발생시 출력
    });
};
</script>
```

아래는 SetupTest.vue 전체 코드이다.

```javascript
<template>
  <span @click="getUserInfo">Click Me!!!!</span><br />
  <span>User's Name: {{ data.user.name }}</span><br>
  <div>{{props.contents}}</div>
</template>

<script setup>
// <script setup>을 사용하면 setup() 함수 내부에서 코드를 작성하는 것과 같다. 
// setup() 함수내부에서는 this 키워드를 사용할 수 없다. 
// import section
import { inject, ref, toRefs, reactive, defineProps , defineEmit} from "vue";

// plugins
// plugin은 injext() 사용하여 주입하여 사용 
let ajax = inject("ajax");  // injects the plugin ajax 


// props section 
// 부모 컴퍼넌트에서 전달하는 props는 defineProps()를 사용해서 생성 
// template에서는 props.contents로 사용 
const props = defineProps( {
   contents:String, 
   default: ""
})
 
// emit section 
const emit = defineEmit(['myEvent'])  // 사용자 이벤트는 defineEmit()을 사용하여 정의

// variable section
const urls = {
  URL_GET_USER: "/user.json", // 사용자 정보 조회
};

// data section

// template에 출력되는 값이 자동으로 변경하려면 reactive( {} ) 형태로 사용해야 함. 
// { } 는 다른 객체로 대체가 불가능하기 때문에   { user: { } } 와 같은 형태로 사용해야 함. 
// template에서는 {{ data.user.name}}과 같은 형식으로 사용 
const data = reactive({
  user: { 
    name: "초기이름",
    age: 0 
  }
})


// event handler section
const getUserInfo = () => {
  let options = {
    method: "GET",
  };

  // ajax() 함수를 사용해서 서버에 요청 
  // ajax() 함수는 Promise 객체를 반화하므로 .then()을 사용하여 응답 결과를 처리 
  // 에러는 .catch()를 사용하여 처리 
  ajax(urls.URL_GET_USER, options)
    .then((res) => {
      // ajax plugin에서 json()을 호출하여 객체로 변환해서 넘어 온다. 
      data.user = res
      emit('myEvent', 'kim')  // 사용자 이벤트 발생
    })
    .catch((error) => {
      console.log("Error occured.");
      console.log(error); // 오류 발생시 출력
    });
};

// function section
</script>
```

## Lifecycle Hooks

4개의 주요한 main events가 있다.

* Creation — runs on your component’s creation
* Mounting — runs when the DOM is mounted
* Updates — runs when reactive data is modified
* Destruction — runs right before your element is destroyed.

## Vue3에서 Lifecycle hooks을 사용하는 방법

Composition API에서 lifecycle hooks를 사용하기 위해서는 그것들을 사용하기 전에 import해야 한다.

```javascript
import { onMounted } from 'vue'
```

setup method에서 접근할 수 있는 9개의 라이프사이클 훅이 있다.

* onBeforeMount – called before mounting begins
* onMounted – called when component is mounted
* onBeforeUpdate – called when reactive data changes and before re-render
* onUpdated – called after re-render
* onBeforeUnmount – called before the Vue instance is destroyed
* onUnmounted – called after the instance is destroyed
* onActivated – called when a kept-alive component is activated
* onDeactivated – called when a kept-alive component is deactivated
* onErrorCaptured – called when an error is captured from a child component

이것들을 import 하면 코드에서 다음과 같이 보일 것이다.

```javascript
<script>
import { onMounted } from 'vue'

export default {
   setup () {
     onMounted(() => {
       console.log('mounted in the composition api!')
     })
   }
}
</script>
```

## HTML 요소에 접근하기

this.$refs는 setup context object에는 포함되지 않았다. 그러면 composition API에서 어떻게 template refs에 접근하는지 살펴보자.

생각보다 간단 할 수 있다. Vue.js는 refs의 개념을 통합한다. 즉, 템플릿 ref를 선언하려면 반응 상태 변수를 선언하기 위해 이미 알고있는 ref () 함수를 사용해야한다.

```javascript
<template>
  <div>
    <h2 ref="titleRef">{{ formattedMoney }}</h2>
    <input v-model="delta" type="number" />
    <button @click="add">Add</button>
  </div>
</template>
```

h2 태그에 titleRef를 설정한다. 그것이 template 수준에서 해야 할 모든 것이다. 이제 setup function에서 같은 titleRef 이름으로 ref를 선언해야 한다. instance를 위해 null을 초기화 한다.

```javascript
export default {
  setup(props) {
    // Refs
    const titleRef = ref(null);

    // Hooks
    onMounted(() => {
      console.log("titleRef", titleRef.value);
    });

    return {
      titleRef
      // ...
    };
  }
};
```

.value property에 접근함으로써 다른 reactive ref와 같이 ref value에 를 접근한다.

### html 요소의 value 사용

value가 단순히 값은 아님. HTML 요소인 것 같음.

```javascript
const titleRef = ref(null);
onMounted( () => { 
  let eleHtml = titleRef.value.innerHTML
  let className= titleRef.value.className 
  
})
```

## References

[Vue3 Composition API](https://bezkoder.com/vue-function-api-example/)\
[VUe3 Script setup](https://github.com/vuejs/rfcs/blob/script-setup-2/active-rfcs/0000-script-setup.md)\
[Vue Lifecycle Hook](https://learnvue.co/2020/12/how-to-use-lifecycle-hooks-in-vue3/)\
[Access Template Refs](https://vuedose.tips/access-template-refs-in-composition-api-in-vuejs-3/)

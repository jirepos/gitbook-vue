# Vue3 Cheet Sheet

## import

\<script setup>을 사용하면 setup() 함수 내부에서 코드를 작성하는 것과 같다. setup() 함수내부에서는 this 키워드를 사용할 수 없다.

```javascript
<script setup>
import { ref,reactive } from 'vue' 
</script>
```

## 반응형 데이터

컴포지션 API에서는 두 가지 유형(reactive, ref)의 변경 가능한 반응형 데이터를 만들 수 있다. 반응형 데이터는 값이 변경됨에 따라 이를 감지하고 해당 값에 종속된 작업이 수행된다. 예를 들면 위의 message 값이 변경되면, DOM에 존재하는 div의 내부 텍스트가 알아서 변경되는 경우를 의미한다.

```javascript
// 1.reactive
const reactiveValue = reactive( {name: 'tom'  } )
reactiveValue.name // tom 

// 2. ref
const refValue = ref(10)
refValue.value // 10 
```

## 변수할당

let { increase, decrease } = userCount() 코드에서는 userCount 함수를 실행하고 함수가 반환하는 객체의 내부 함수들을 각 increase, decrease 변수에 할당한다.

```javascript
let userCount = () => {
  let increase = () => {
    console.log('increase')
  }
  let decrease = () => {
    console.log('decrease')
  }
  return {increase, decrease}
}

let { increase, decrease } = userCount()
increase()
decrease()
```

## 계산된(computed) 속성

computed는 getter 함수가 반환하는 값에 대해 변경 불가능한 반응형 데이터를 반환한다. getter 함수는 위 코드에서 computed의 인자로 전달되는 () => myValue \* 2 에 해당된다. value2x는 myValue에 2를 곱한 값을 제공한다.

```javascript
export default { 
  setup(){
    const myValue = ref(10)
    const value2x = computed( () => myValue * 2)
    return {
      value2x
    }
  }
}
```

```javascript
<script setup>
const myValue = ref(10)
const value2x = computed( () => myValue * 2)
</script>
```

## Props and Emit

props는 defineProps()를 사용해서 정의하고 emit는 defineEmit()를 사용해서 정의한다.

```javascript
<script setup>
  import { defineProps, defineEmit } from 'vue'
  // expects props options
  const props = defineProps({
    foo: String,
  })
  // expects emits options
  const emit = defineEmits(['myEvent'],['delete'])
</script>
```

부모 컴퍼넌트에서 전달하는 props는 defineProps()를 사용해서 생성하고 template에서는 props.contents로 사용한다.

```
<template>
  <div>{{props.contents}}</div>
</template>  
```

부모 컴포넌트에서 props의 값을 전달한다.

```javascript
<template>
  <SetupTest  @my-event="updateChange" contents="부모에서 값전달"/>
</template>  
```

emit는 defineEmits으로 생성하고 다음과 같이 이벤트 이름으로 사용한다.

```javascript
emit('myEvent', 'kim')  // 사용자 이벤트 발생  
```

부모 컨테이너에서는 다음과 같이 사용한다.

```javascript
<template>
  <SetupTest  @my-event="updateChange" contents="부모에서 값전달"/>
</template>
```

## HTML 요소에 접근(ref())

HTML 요소에 ref속성을 사용하여 정의한다.

```javascript
  <template>
  <div>
    <h2 ref="titleRef">{{ formattedMoney }}</h2>
  </div>
</template>
```

ref(null) ㅎ마수를 사용하여 요소를 참조한다. instance를 위해 null로 초기화한다.

```javascript
const titleRef = ref(null);  
```

## 자식 컴포넌트 메서드 접근

자식 컴포넌트의 메서드를 호출하려면 ref 속성을 사용한다. 먼저 template에 ref 속성을 사용하여 이름을 설정한다.

```
<template>
  <editor ref='child"></editor>
</template>
```

secript setup에서 ref()를 사용하여 참조를 구한다. 참조 변수의 value 속성을 사용하여 함수를 호출한다.

```javascript
<script setup>
const editor = ref(null)
const callMethod = () => { 
  editor.value.callMethod()
}
</script>
```

그런데 callMethod 정의가 되어 있었지만 callMethod가 없다고 오류가 발생했다. 정확한 원인을 알 수 없지만 **\<script setup>는 아직 실험적이라서 버그가 있는 것 같다. setup() 함수를 사용하여 하위 컴포넌트를 수정**하고 나서야 정상적으로 메서드가 호출되었다.

```javascript
<script>  // setup은 삭제했다
export default { 
  setup() {
    const callMethod = () => { 
      console.log("callMethod")
    }
    return { 
      callMethod 
    }
  }
}
</script>
```

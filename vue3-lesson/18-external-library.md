# 외부 라이브러리 사용 

Vue는 가상의 DOM을 사용하고 jQuery 플러그인이나 Toast UI Editor를 직접 사용하하는 경우에는 실제 DOM을 사용하기 때문에 jQuery로 직접 요소를 조작하면 문제가 발생할 수 있고 마찬가지로 Toast UI Editor도 직접 조작하면 문제가 발생할 수 있다.


따라서, 

* HTML 요소를 처리하기 위해서는 실제 요소가 DOM에 붙을 때(mounted) 처리해야 한다. 





## Lifecycle Hook 함수 

* onBeforeMount -장착이 시작되기 전에 호출 됨
* onMounted -컴포넌트가 마운트 될 때 호출됩니다.
* onBeforeUpdate -반응 형 데이터가 변경 될 때와 다시 렌더링하기 전에 호출됩니다.
* onUpdated -다시 렌더링 후 호출
* onBeforeUnmount -Vue 인스턴스가 파괴되기 전에 호출됩니다.
* onUnmounted -인스턴스가 파괴 된 후 호출됩니다.
* onActivated -연결 유지 구성 요소가 활성화되면 호출됩니다.
* onDeactivated -연결 유지 구성 요소가 비활성화 될 때 호출됩니다.
* onErrorCaptured -하위 구성 요소에서 오류가 캡처 될 때 호출됩니다.


## 요소에 대한 접근 방법

ref="myInput"과 같이 ref 속성에 요소의 ref 속성명을 설정한다. 
```html
<input  ref="myInput" type="text" value="" placeholder="input your  text" >
```
setup() 함수에서 ref(null)을 사용하여 요소의 참조를 구할 수 있다. 이 때 const 이름은 ref="myInput"에 정의한 것과 같은 이름을 정의한다. 

```jsx
const myInput = ref(null);  // template의 ref 값과 동일한 이름의 변수 선언 
```
myInput을 사용하려면 다른 변수들과 마찬가지로 setup() 함수에서 return 해야 한다. 
```jsx
export default defineComponent({
  setup() {
    const myInput = ref(null);  // template의 ref 값과 동일한 이름의 변수 선언 
    return { 
      myInput
    }
  },
})

```

요소에 접근하려면 요소거 DOM에 붙은 이후에 접근이 가능하기 때문에 onMounted() 훅 함수에서 접근해야 한다. 
```jsx
    onMounted( () => {
      console.log(myInput.value)
      console.log(myInput.value.type)
    })
```
위에서는 .value를 사용하여 요소를 참조하고 있고 .value.type을 사용하여 요소의 속성에 접근하고 있다. 

아래는 전체 코드이다. 

```html
<template>
  <div>
    <input  ref="myInput" type="text" value="" placeholder="input your  text" >
  </div>
  
</template>
<script>
import { defineComponent , ref,  onMounted } from 'vue'

export default defineComponent({
  setup() {
    
    const myInput = ref(null);  // template의 ref 값과 동일한 이름의 변수 선언 

    onMounted( () => {
      console.log(myInput.value)
      console.log(myInput.value.type)
    })

    return { 
      myInput
    }
    
  },
})
</script>

```

## jQuery 사용 
### 설치
```shell
yarn add jquery
```
package.json 확인
```json
  "dependencies": {
    "jquery": "^3.6.0",
    "vue": "^3.2.6",
    "vue-router": "^4.0.12",
    "vuex": "^4.0.2"
  },
```

### jQuery 사용

jQUery를 import한다. 
```jsx
import $ from 'jquery'
```
template에 다음과 같이 요소를 정의한다. 
```html
<input  id="myInput" ref="myInput" type="text" value="ddd" placeholder="input your  text" >
```
onMounted() 혹 함수에서 사용한다. 
```jsx
  setup() {
    const myInput = ref(null);  // template의 ref 값과 동일한 이름의 변수 선언 
    onMounted( () => {

      console.log(myInput.value)  // ref를 사용한 요소 접근
      console.log(myInput.value.type)  // ref를 사용하여 요소의 속성에 접근
      console.log($(myInput.value)[0]);

      let ele  = $(myInput.value)  // jQuery를 사용하여 요소 접근
      console.log(ele[0].value)  // 요소의 속성에 접근
      ele[0].value = "Park"  // 요소의 값 설정
      console.log(typeof $("#myInput"))  // jQuery가 반환한 값의 typ 확인, object
      $(myInput.value)[0].value = "Jo"  // 요소의 값 설정
      $("#myInput").on("click", () => {  // 요소에 이벤트 바인딩
        console.log("Clicked")
      })
      $("#myInput").val('원하는 값');  // 요소의  값 설정
    })

    return {
      myInput
    }
  }
```

**전역 변수 사용**

jQuery를 전역에서 사용하고 싶으면 다음과 같이 window 변수로 설정한다.
```jsx
import jQuery from 'jquery'  // jQuery import
window.$ = jQuery    // $를 전역 변수로 설정
```
다음과 같이 $ 변수를 사용할 수 있다. 
```jsx
 window.$("#myInput").val('원하는 값2');
```






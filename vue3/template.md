# Template 기초

이 글에서는 템플릿에 데이터를 표시하고, DOM 요소와 상호작용하는 방법에 대해서 알아 볼 것이다.

> 내부적으로 Vue는 템플릿을 가상 DOM 렌더링 함수로 컴파일한다. 반응형 시스템과 결합된 Vue는 앱 상태가 변경될 때 다시 렌더링할 최소한의 컴포넌트를 지능적으로 파악하여 DOM 조작을 최소화한다.

## 템플릿 문법

데이터 바인딩의 가장 기본 형태는 “Mustache”(이중 중괄호 구문)기법을 사용한 문자열 보간법(text interpolation)이다.

### 문자열

vue/src/components 디렉토리에 lesson03.vue 파일을 생성한다. script에 변수 msg를 생성한다.

```javascript
<script setup>
let msg = "hello"
</script>
```

변수를 화면에 표시하기 위해 template에 span 태그를 추가한다.

```html
<template>
  <div>
    <span>{{msg}}</span>
  </div>
</template>
```

화면에 hello가 표시되는 것을 확인할 수 있다.

Mustache 태그는 해당 컴포넌트 인스턴스의 msg 속성 값으로 대체된다. 또한 msg 속성이 변경될 때 마다 갱신된다.

script에 msg 변수의 값을 다른 것으로 할당을 해보자.

```javascript
<script setup>
let msg = "hello"
msg = "hello2"
</script>
```

브라우저에 출력되는 값이 hello2가 될 것으로 예상하지만 여전히 hello로 보이게 될 것이다.

지금 설명하는 방식은 \<script setup\ >방식인데, 이것은 script tag내의 자바스크립트들은 Vue 콤포넌트의 setup() 함수내에서 작성하는 방식과 동일하기 때문이다.

setup() 함수내에서 선언된 변수는 반응형이 아니다. 반응형으로 만들기 위해서는 ref() 함수를 사용하여 반응형으로 값을 할당해야 한다.

import를 사용해서 ref()을 import해야 한다.

```javascript
import { ref } from "vue";
```

msg를 ref()를 사용해서 값을 할당하고 다시 msg 변수에 값을 할당해 보자.

```javascript
let msg = ref("hello")
msg = "hello2"
```

다시 실행하면 이번에는 hello2로 보일 것이다. ref()에 대해서는 다른 문서에서 다시 설명을 할 것이다.

### 원시 HTML

이중 중괄호는 데이터를 HTML이 아닌 일반 텍스트로 해석한다. 실제 HTML을 출력하려면 v-html 디렉티브를 사용해야 한다.

template에 div를 추가한다.

```html
<div v-html="rawHtml"></div>
```

script에 rawHtml 변수를 추가한다.

```javascript
let rawHtml = "<h1>raw html</h1>"
```

div의 내용은 일반 HTML로 해석되는 rawHtml 속성값으로 대체된다. 이 때 데이터 바인딩은 무시된다. rawHtml의 값이 변경되어도 변경된 값으로 대체되지 않는다는 의미이다.

### directive

디렉티브는 v-로 시작하는 특수한 속성이다. 디렉티브 속성 값은 단일 JavaScript 표현식이 된다. (나중에 설명할 v-for와 v-on은 예외이다.) 디렉티브의 역할은 표현식의 값이 변경될 때 발생하는 부수 효과(side effects)들을 반응적으로 DOM에 적용하는 것이다.

```html
<p v-if="seen">이제 저를 볼 수 있어요.</p>
```

여기서 v-if 디렉티브는 표현식 seen 값의 참, 거짓 여부를 바탕으로

엘리먼트를 삽입하거나 제거한다.

```javascript
<script setup>
let seen = false 
</script>
```

#### 전달인자

일부 디렉티브는 디렉티브명 뒤에 **콜론(:)으로 표기되는 전달인자**를 가질 수 있다. 예를 들어, v-bind 디렉티브는 반응적으로 HTML 속성을 갱신하는 데 사용한다.

```html
<a v-bind:href="url"> ... </a>
```

```javascript
<script setup>
let url = "http://www.naver.com"
</script>
```

#### 동적 전달인자

JavaScript 표현식을 대괄호로 묶어 디렉티브 전달인자로 사용할 수도 있다.

```html
<!--
후술된 "동적 전달인자 형식의 제약" 부분에서 설명한 바와 같이 전달인자 형식에 몇 가지 제약이 있다는 점에 주의해 주세요.
-->
<a v-bind:[attributeName]="url"> ... </a>
```

```javascript
<script setup>
let seen = true 
let url = "http://www.naver.com"
let attributeName = "href"
</script>
```

여기서 attributeName은 JavaScript 표현식으로 동적 변환되며, 변환된 결과는 전달인자의 최종값으로 사용된다. 예를 들어, 여러분의 컴포넌트 인스턴스에 값이 "href"인 데이터 속성 attributeName이 있다면, 이 바인딩은 v-bind:href와 같다.

이와 유사하게, 동적인 이벤트명에 핸들러를 바인딩할 때 동적 전달인자를 활용할 수 있다.

```html
<a v-on:[eventName]="doSomething"> ... </a>
```

### 이벤트 수식어

수식어는 점으로 표기되는 특수 접미사로, 디렉티브를 특별한 방법으로 바인딩해야 함을 나타낸다. 예를 들어, .prevent 수식어는 v-on 디렉티브가 트리거된 이벤트에서 event.preventDefault()를 호출하도록 지시한다.

```html
<a v-on:click.prevent="myevent">myevent 실행</a>
```

```javascript
const myevent = () => {
  console.log("myevent가 실행") 
}
```

**.prevent**\
기본이벤트의 자동 실행을 중단 시킴. preventDefault()와 동일한 기능이다. **.stop**\
이벤트가 전파되는 것을 중단 시킴(Bubbling 중단). stopPropagation()과 동일한 기능이다.

**.capture**\
포착 단계에서만 이벤트를 발생시키다. 내부 엘리먼트를 대상으로 하는 이벤트가 해당 엘리먼트에서 처리되기 전에 여기서 처리한다.

**.self**\
발생 단계에서만 이벤트를 발생시킨다. event.target이 엘리먼트 자체인 경우에만 트리거를 처리, 자식 엘리먼트에서는 실행안된다. **once**\
이벤트를 한번만 실행 시킨다. passive 기본 이벤트를 취소할 수 없게 한다. 있을지 모를 .preventDefault()를 실행 안되게 한다.

#### 키 수식어

키에 대한 수식ㅇ어는 숫자를 이용하는 것이었으나 사람이 숫자를 기억하기 힘들기 때문에 키 값에 해당하는 수식어를 별도의 이름으로 지정하여 활용한다.

```html
  <input @keyup.enter="enterevent">
```

```javascript
const enterevent = () => {
  console.log("enter가 눌림")
}
```

| 키 수식어   | 고유키 값 | 비고                             |
| ------- | ----- | ------------------------------ |
| .enter  | 13    |                                |
| .tab    | 9     |                                |
| .delete | 8     | “Delete” 와 “Backspace” 키 모두 해당 |
| .esc    | 27    |                                |
| .space  | 32    |                                |
| .up     | 33    |                                |
| .down   | 34    |                                |
| .left   | 37    |                                |
| .right  | 39    |                                |
| .ctrl   | 17    |                                |
| .alt    | 18    |                                |
| .shfit  | 16    |                                |
| .meta   |       |                                |

#### 마우스 수식어

마우스 왼쪽 버튼이 클릭되었을 때 이벤트가 실행되게 하려면 다음과 같이 마우스 수식어를 사용하면 된다.

```html
  <div @mousedown.left="mouseleft">
    Click Me
  </div>
```

```javascript
const mouseleft = () => { 
  console.log("mouse left button clicked.")
}
```

| 키 수식어   | 비고            |
| ------- | ------------- |
| .left   | 마우스 왼쪽 버튼 클릭  |
| .right  | 마우스 오른쪽 버튼 클릭 |
| .middle | 마우스 가운데 휠 클릭  |

### v-for 루프

v-for 디렉티브를 사용하여 배열을 기반으로 리스트를 렌더링 할 수 있다. v-for 디렉티브는 item in items 형태로 특별한 문법이 필요하다. 여기서 items는 원본 데이터 배열이고 item은 반복되는 배열 엘리먼트의 별칭이다.

속성에 item의 값을 설정하려면 v-bind를 사용한다.

```javascript
<template>
  <ul>
    <li v-for="item in menuIds" :data-menu-id="item.menuId" @click="onMenuClicked(item.menuId)">
    </li>
  </ul>
</template>

<script setup>
let menuIds = ref( [
  { menuId: "A01", menuName: "전체메시지" },
  { menuId: "A02", menuName: "받은메시지" },
  { menuId: "A03", menuName: "보낸메시지" },
]) 

// event seciton 
const onMenuClicked = menuId => {
  console.log(menuId)
}
</script>
```

### v-on 이벤트

v-on 디렉티브는 @ 기호로 DOM 이벤트를 듣고 트리거 될 때와 JavaScript를 실행할 때 사용한다. v-on:click="methodName"나 줄여서 @click="methodName"으로 사용한다.

#### 이벤트 청취

간단히 자바스크립트를 실행할 수 있다.

```javascript
<template>
  <div id="basic-event">
    <button @click="counter += 1">Add 1</button>
    <p>The button above has been clicked {{ counter }} times.</p>
  </div>
</template>
<script setup>
let counter = ref(0) 
</script>
```

#### 메소드 이벤트 핸들러

많은 이벤트 핸들러의 로직은 더 복잡할 것이므로, JavaScript를v-on 속성 값으로 보관하는 것은 간단하지 않다. 그게 v-on이 호출하고자 하는 메소드의 이름을 받는 이유이다.

인자 없이 메소드를 호출한다. 이벤트 핸듣러 함수에서 event는 native DOM 이벤트이다.

```javascript
<template>
   <button @click="greet">Greet</button>
</template>
<script setup>
const greet = event => { 
  console.log(event.target.tagName) 
}
</script>
```

#### 인라인 메소드 핸들러

메소드 이름을 직접 바인딩 하는 대신 인라인 JavaScript 구문에 메소드를 사용할 수도 있다.

```javascript
<template>
  <div id="example-3">
    <button v-on:click="say('hi')">Say hi</button>
    <button v-on:click="say('what')">Say what</button>
  </div>
</template>
<script setup>
const say = message => { 
  console.log(message)
}
</script>
```

때로 인라인 명령문 핸들러에서 원본 DOM 이벤트에 액세스 해야할 수도 있다. 특별한 $event 변수를 사용해 메소드에 전달할 수도 있다.

```javascript
<template>
<button v-on:click="warn('Form cannot be submitted yet.', $event)">
  Submit
</button>
</template>
<script setup>
const warn = (message, event) => {
  // 이제 네이티브 이벤트에 액세스 할 수 있다
  if (event) event.preventDefault()
    alert(message)
}
</script>
```

#### 이벤트 수식어

아래 참조 URL에서 확인한다.

### References

[이벤트 핸들링](https://v3.vuejs-korea.org/guide/events.html#%E1%84%8B%E1%85%B5%E1%84%87%E1%85%A6%E1%86%AB%E1%84%90%E1%85%B3-%E1%84%8E%E1%85%A5%E1%86%BC%E1%84%8E%E1%85%B1)

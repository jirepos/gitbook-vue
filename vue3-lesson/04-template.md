이 글에서는 템플릿에 데이터를 표시하고, DOM 요소와 상호작용하는 방법에 대해서 알아 볼 것이다.

> 내부적으로 Vue는 템플릿을 가상 DOM 렌더링 함수로 컴파일한다. 반응형 시스템과 결합된 Vue는 앱 상태가 변경될 때 다시 렌더링할 최소한의 컴포넌트를 지능적으로 파악하여 DOM 조작을 최소화한다.



# 템플릿 문법
데이터 바인딩의 가장 기본 형태는 “Mustache”(이중 중괄호 구문)기법을 사용한 문자열 보간법(text interpolation)이다.

##  문자열
```javascript
<script setup>
let msg = "hello"
</script>
```

template
```html
<template>
  <div>
    <span>{{msg}}</span>
  </div>
</template>
```

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
const msg = ref("Hello");
msg.value = "Hello2";
```


## 원시 HTML 
이중 중괄호는 데이터를 HTML이 아닌 일반 텍스트로 해석한다. 실제 HTML을 출력하려면 v-html 디렉티브를 사용해야 한다.

```javascript
let rawHtml = "<h1>raw html</h1>"
```
```html
<div v-html="rawHtml"></div>
```
div의 내용은 일반 HTML로 해석되는 rawHtml 속성값으로 대체된다. 이 때 데이터 바인딩은 무시된다. rawHtml의 값이 변경되어도 변경된 값으로 대체되지 않는다는 의미이다.



## directive 
디렉티브는 v-로 시작하는 특수한 속성이다. 디렉티브 속성 값은 단일 JavaScript 표현식이 된다. (나중에 설명할 v-for와 v-on은 예외이다.) 디렉티브의 역할은 표현식의 값이 변경될 때 발생하는 부수 효과(side effects)들을 반응적으로 DOM에 적용하는 것이다

```javascript
<script setup>
let seen = false 
</script>
```

```html
<p v-if="seen">이제 저를 볼 수 있어요.</p>
```
나중에 directive를 직접 만들어 볼 것이다. 




## 전달인자 

일부 디렉티브는 디렉티브명 뒤에 콜론(:)으로 표기되는 전달인자를 가질 수 있다. 예를 들어, v-bind 디렉티브는 반응적으로 HTML 속성을 갱신하는 데 사용한다.


```html
<a v-bind:href="url"> ... </a>
```
```javascript
<script setup>
let url = "http://www.naver.com"
</script>
```

## 동적 전달인자 
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



## v-on 이벤트
v-on 디렉티브는 @ 기호로 DOM 이벤트를 듣고 트리거 될 때와 JavaScript를 실행할 때 사용한다. v-on:click="methodName"나 줄여서 @click="methodName"으로 사용한다

### 이벤트 청취
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

### 메소드 이벤트 핸들러
많은 이벤트 핸들러의 로직은 더 복잡할 것이므로, JavaScript를v-on 속성 값으로 보관하는 것은 간단하지 않다. 그게 v-on이 호출하고자 하는 메소드의 이름을 받는 이유이다.
인자 없이 메소드를 호출한다. 이벤트 핸듣러 함수에서 event는 native DOM 이벤트이다.


```html
<template>
   <button @click="greet">Greet</button>
</template>
<script setup>
const greet = event => { 
  console.log(event.target.tagName) 
}
</script>
```

이벤트 핸들링을 위한 자세한 내용은 다음의 URL을 참고한다. 

https://v3.vuejs-korea.org/guide/events.html#%E1%84%8B%E1%85%B5%E1%84%87%E1%85%A6%E1%86%AB%E1%84%90%E1%85%B3-%E1%84%8E%E1%85%A5%E1%86%BC%E1%84%8E%E1%85%B1



# 조건부 렌더링
## v-if
v-if, v-else , v-else-if 세가지 사용할 수 있다. 
```html
<script>
import { defineComponent , ref, reactive   } from 'vue'

export default defineComponent({
  setup(props, context) {

    const ctype = ref("C")

    return { 
      ctype 
    }

  },


})
</script>
<template>
  <div>
    <div  v-if="ctype === 'A'">A 입니다.  </div>
    <div  v-else-if="ctype === 'B'">B 입니다.  </div>
    <div  v-else="ctype === 'C'">C 입니다.  </div>
  </div>
</template>
```

boolean 값을 사용해도 된다. 
```html
<script>
  const okay = ref(true); 
  return { 
    okay
  }
</script>
<template>
  <div>
    <div v-if="okay">A 입니다.  </div>
    <div v-elseC 입니다.  </div>
  </div>
</template>
```

## v-show 
v-if 와 결과는 같지만 동작하는 방식이 다른 v-show 라는 디렉티브가 있습니다. v-if 의 경우 조건이 성립하지 않을 경우 태그 자체를 렌더링 하지 않지만, v-show 는 렌더링은 하되 display: none 처리를 합니다.




## v-for 

v-for 지시문을 사용하여 배열을 기반으로 항목 목록을 렌더링 할 수 있습니다

```html
<ul id="array-rendering">
  <li v-for="item in items">
    {{ item.message }}
  </li>
</ul>
```
```javascript
const items = reractive([
  { message: 'Foo'}.
  { message: 'Bar'}
])
```


v-for 블록 내부에서는 부모 범위 속성에 대한 모든 액세스 권한이 있습니다. v-for 는 현재 항목의 인덱스에 대한 선택적 두 번째 인수도 지원합니다.
```html
<ul id="array-with-index">
  <li v-for="(item, index) in items">
    {{ parentMessage }} - {{ index }} - {{ item.message }}
  </li>
</ul>
```


### v-for in 
```javascript
  const myObject = reactive( {
        title: '제목',
        author: '나',
        publishedAt: '2020-03-22'
  })
```
```html
<ul id="v-for-object" class="demo">
  <li v-for="(value, name, index) in myObject">
    {{ index }}. {{ name }}: {{ value }}
  </li>
</ul>
```
결과 
```shell
0. title: 제목
1. author: 나
2. publishedAt: 2020-03-22
```


### v-for key 
각 노드의 id를 추적하여, 재사용하거나 순서를 변경하는등의 작업을 위해 각 아이템에 유일한 key 속성을 주어, vue에게 힌트를 줄수 있습니다
```html
<ul>
  <li v-for="item in items" :key="item.id">...</li>
</ul>
```

## v-if와 v-for
  v-if 보다  v-for 이 높은 우선순위를 가지고 있음. 같이 사용하지 않도록 한다. 


https://v3.ko.vuejs.org/guide/conditional.html#v-show



# 연습문제 
세 개의 서브 컴포넌트. 레이아웃을 가지는 메인 컴포넌트. 
```html
<div>
  <child1></child1>
  <child2></child2>
  <child3></child3>
</div>
````

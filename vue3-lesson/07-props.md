# Props 


## 기초 
마치 HTML 요소의 속성인 것처럼 사용할 수 있어 부모 컴포넌트에서 자식 컴포넌트에 값을 전달 할 수 있다. 

아래는 title prop에 값을 전달하고 있다. 
```html
<script setup>
import MyComp from '@/components/c06/MyComp.vue'
</script>

<template>
  <div>
    <my-comp title="hello"></my-comp>
  </div>
</template>
```

props 속성을 이용하여 다음과 같이 정의한다. 

```html
<script>
export default {
  props: [ 'title' ],
  setup(props, { emit } ) {
  },
}
</script>
```

props도 일반 변수와 같이 {{}}를 이용하여 사용할 수 있다.  전체코드는 다음과 같다. 

```html
<template>  
  <div>
    {{title}}
  </div>
  
</template>
<script>
export default {
  props: [ 'title' ],
  setup(props, { emit } ) {
  },
}
</script>
```


## 여러개의 props 사용
다음과 같이 여러개를 정의할 수 있다. 
```javascript
  props: [ 'title' , 'blogId'],
```
그러면 다음과 같이 사용할 수 있다. 
```html
<my-comp title="hello" blogId="1111"></my-comp>
```

## prop 타입 설정
일반적으로 모든 prop가 특정 유형의 값이 되기를 원할 것입니다. 이 경우 속성의 이름과 값에 각각 prop 이름과 타입이 포함된 객체로 prop를 나열할 수 있습니다.

```javascript
props: {
  title: String,
  likes: Number,
  isPublished: Boolean,
  commentIds: Array,
  author: Object,
  callback: Function,
  contactsPromise: Promise // 또는 다른 생성자
}
```

### 타입 
type은 다음 기본 생성자(native constructor) 중 하나일 수 있습니다:
* String
* Number
* Boolean
* Array
* Object
* Date
* Function
* Symbol


자세한 사용방법은 다음의 URL을 참고한다. 
https://v3.ko.vuejs.org/guide/component-props.html#prop-%E1%84%90%E1%85%A1%E1%84%8B%E1%85%B5%E1%86%B8




## 단방향 데이터 흐름
모든 props는 자식 속성과 부모 속성 사이에 아래로 단방향 바인딩(one-way-down binding)을 형성합니다. 부모 속성이 업데이트되면 자식으로 흐르지만 반대 방향은 아닙니다. 이렇게하면 하위 컴포넌트가 실수로 앱의 데이터 흐름을 이해하기 힘들게 만드는 상위 컴포넌트 상태 변경을 방지할 수 있습니다.




## Prop 유효성 검사 
```javascript
props: {
    // 기본 타입 체크 (`null`과 `undefined`는 모든 타입 유효성 검사를 통과합니다.)
    propA: Number,
    // 여러 타입 허용
    propB: [String, Number],
    // 문자열 필수
    propC: {
      type: String,
      required: true
    },
    // 기본 값을 갖는 숫자형
    propD: {
      type: Number,
      default: 100
    },
    // 기본 값을 갖는 객체 타입
    propE: {
      type: Object,
      // 객체나 배열의 기본 값은 항상 팩토리 함수로부터 반환되어야 합니다.
      default: function() {
        return { message: 'hello' }
      }
    },
    // 커스텀 유효성 검사 함수
    propF: {
      validator: function(value) {
        // 값이 꼭 아래 세 문자열 중 하나와 일치해야 합니다.
        return ['success', 'warning', 'danger'].indexOf(value) !== -1
      }
    },
    // 기본 값을 갖는 함수
    propG: {
      type: Function,
      // 객체나 배열과 달리 아래 표현은 팩토리 함수가 아닙니다. 기본 값으로 사용되는 함수입니다.
      default: function() {
        return 'Default function'
      }
    }
```


## Prop 대소문자 구분(camelCase vs kebab-case)
HTML 속성명은 대소문자를 구분하지 않으므로, 브라우저는 모든 대문자를 소문자로 해석합니다. 즉, DOM내 템플릿을 사용할 때 camelCase된 prop명은 kebab-case(하이픈으로 구분)된 해당 항목을 사용해야 합니다:

```javascript
const app = Vue.createApp({})

app.component('blog-post', {
  // JavaScript에서의 camelCase
  props: ['postTitle'],
  template: '<h3>{{ postTitle }}</h3>'
})
```
```html
<!-- HTML에서의 kebab-case -->
<blog-post post-title="hello!"></blog-post>
```

## 정적/동적 Prop 전달 
지금까지는 정적으로 prop 값을 전달했다. 
```html
<blog-post title="Vue와 떠나는 여행"></blog-post>
```
또는 다음과 같이 v-bind나 약어인 : 문자를 사용하여 prop에 동적인 값을 전달한다. 
```html
<!-- 변수의 값을 동적으로 할당 -->
<blog-post :title="post.title"></blog-post>

<!-- 복잡한 표현을 포함한 동적 값 할당 -->
<blog-post :title="post.title + ' by ' + post.author.name"></blog-post>
```

위의 두 가지 예시에서 문자열 값을 전달하지만 실제로 어떤(any) 타입의 값도 prop에 전달할 수 있습니다



# setup 함수 내에서서의 Prop 사용 
setup() 함수의 첫번째 매개변수인 props를 이용하여 Prop를 사용할 수 있다. 
```html
<template>  
  <div>
    {{title}}
  </div>
  
</template>
<script>
export default {
  props: [ 'title' ],
  setup(props, { emit } ) {
    console.log(props.title)
  },
}
</script>
````


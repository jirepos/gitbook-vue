# Vue에 스타일 적용

## \<style> 태그 내부에 선언

vue style에서 scoped 속성을 가지고 있으면 css는 현재 컴포넌트의 엘리먼트에만 적용된다.

```html
<style scoped>
  body { background-color: bisque;}
</style>
```

자식 컴포넌트에 접근하기 위해서는 scoped를 제거하여 사용하거나 deep selector를 사용하면 접근할 수 있다.

> deep selector는 나중에 정리하겠다.

## inline 방식으로 선언하기

인라인 방식은 태그에 직접적용하는 방식이다.

```html
  <div style="display:none">이거 안 보인다.</div>
```

v-bind:style을 사용하여 스타일을 바인딩할 수 있다. 더블 쿼드 안에 { }를 사용한다. style 속성 이름에 이름에 camelCase와 kebab-case (따옴표를 함께 사용해야 함)를 사용할 수 있다.

```html
<div v-bind:style="{ color: activeColor, fontSize: fontSize + 'px' }"></div>
```

backColor 변수를 선언하고 값을 설정한다.

```javascript
data: {
  activeColor: 'red',
  fontSize: 30
}
```

거의 css 처럼 보이지만 자바스크립트 객체이다.

스타일 객체에 직접 바인딩 하여 템플릿이 더 간결하도록 만드는 것이 좋다.

```html
<div v-bind:style="styleObject"></div>
```

```javascript
data: {
  styleObject: {
    color: 'red',
    fontSize: '13px'
  }
}
```

v-bind:style에 대한 배열 구문은 같은 스타일의 엘리먼트에 여러 개의 스타일 객체를 사용할 수 있게 한다.

```html
<div v-bind:style="[baseStyles, overridingStyles]"></div>
```

## HTML 클래스 바인딩

클래스를 동적으로 토글하기 위해 v-bind:class에 객체를 전달할 수 있다.

```html
<div v-bind:class="{ active: isActive }"></div>
```

위 구문은 active 클래스의 존재 여부가 데이터 속성 isActive 의 참 속성에 의해 결정되는 것을 의미한다. 객체에 필드가 더 있으면 여러 클래스를 토글 할 수 있다. 또한v-bind:class 디렉티브는 일반 class 속성과 공존할 수 있다. 그래서 다음과 같은 템플릿이 가능하다:

```html
<div
 class="static"
 v-bind:class="{ active: isActive, 'text-danger': hasError }"
></div>
```

그리고 데이터는:

```javascript
data: {
  isActive: true,
  hasError: false
}
```

아래와 같이 렌더링 된다.

```html
<div class="static active"></div>
```

바인딩 된 객체는 인라인 일 필요는 없다.

```html
<div v-bind:class="classObject"></div>
```

```javascript
data: {
  classObject: {
    active: true,
    'text-danger': false
  }
}
```

같은 결과로 렌더링 된다. 또한 객체를 반환하는 계산된 속성에도 바인딩 할 수 있다. 이것은 일반적이며 강력한 패턴이다.

```html
<div v-bind:class="classObject"></div>
```

```javascript
data: {
  isActive: true,
  error: null
},
computed: {
  classObject: function () {
    return {
      active: this.isActive && !this.error,
      'text-danger': this.error && this.error.type === 'fatal'
    }
  }
}
```

우리는 배열을 v-bind:class 에 전달하여 클래스 목록을 지정할 수 있다.

```html
<div v-bind:class="[activeClass, errorClass]"></div>
```

```javascript
data: {
  activeClass: 'active',
  errorClass: 'text-danger'
}
```

아래와 같이 렌더링 된다.

```html
<div class="active text-danger"></div>
```

목록에 있는 클래스를 조건부 토글하려면 삼항 연산자를 이용할 수 있다.

```html
<div v-bind:class="[isActive ? activeClass : '', errorClass]"></div>
```

이것은 항상 errorClass를 적용하고 isActive가 true일 때만 activeClass를 적용한다. 그러나 여러 조건부 클래스가 있는 경우 장황해질 수 있다. 그래서 배열 구문 내에서 객체 구문을 사용할 수 있다.

```html
<div v-bind:class="[{ active: isActive }, errorClass]"></div>
```

## Vue 컴포넌트에서 외부 스타일 가져오기

\<style> 태그 내에 import 구문을 사용한다.

```html
<style>
  @import "https://cdn-css-file.css";
</style>
```

### CSS import하기

assets 디렉토리에 main.css 파일을 만든다.

```css
/* global */
body { 
  margin: 0;
  padding: 0;
  font-family: 'applesdgothicneo-ultralight', 'Noto Sans', '나눔고딕', 'NanumGothic', '맑은 고딕', 'Malgun Gothic', '돋움', Dotum, Arial, sans-serif;
  font-size: 16px;
  line-height: 28px;
}
```

main.js에서 css를 import한다.

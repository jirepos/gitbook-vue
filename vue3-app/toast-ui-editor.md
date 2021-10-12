# Toast UI Editor 붙이기

글 작성할 때 사용할 Editor는 Toast UI Editor를 사용하기로 했는데 문제는 아직 Vue 3를 지원하지 않는다. 그래서 Jquery Plugin을 Vue에 붙일 때 사용했던 방법으로 붙이기로 했다.

Vue는 가상의 DOM을 사용하고 jQuery 플러그인이나 Toast UI Editor를 직접 사용하하는 경우에는 실제 DOM을 사용하기 때문에 jQuery로 직접 요소를 조작하면 문제가 발생할 수 있고 마찬가지로 Toast UI Editor도 직접 조작하면 문제가 발생할 수 있다.

SFC(Single File Component)를 사용하여 컴포넌트를 만들고 jQuery 플러그인이나 Toast UI Editor를 컴포넌트로 만들어야 한다. 요소를 처리하기 위해서는 실제 요소가 DOM에 붙을 때(mounted) 처리해야 한다. setup 함수에서 hook 함수를 정의해야 한다.

우선 전체 코드를 보자.

```javascript
<template>
  <div   ref="refEditor">2222</div>
</template>
<script setup>
import "@toast-ui/editor/dist/toastui-editor.css"; // Editor's Style

import Editor from "@toast-ui/editor";
import { ref, onMounted } from "vue";

const refEditor = ref(null); // template의 ref의 값과 동일한 변수 선언

onMounted( () => { 
   console.log("onMounted")
   
   console.log("====" + refEditor.value)
   const xeditor = new Editor({
    el: refEditor.value,
    height: "500px",
    initialEditType: "markdown",
    previewStyle: "vertical",
  });
})
</script>
```

## 설치

먼저 toast UI editor를 설치한다.

```shell
$ npm install --save @toast-ui/editor # Latest Version
```

## 모듈 임포트

CSS와 JavaScript를 임포트한다.

```
<script setup>
import "@toast-ui/editor/dist/toastui-editor.css"; // Editor's Style
import Editor from "@toast-ui/editor";
</script>
```

## 컨테이너 요소 생성

template에 editor를 생성할 container element를 생성한다.

```html
<template>
  <div   ref="refEditor">2222</div>
</template>
```

## 요소 참조 변수 생성

container element의 참조를 저장할 변수를 생성한다. 변수의 이름은 template의 ref속성에 정의된 값과 동일한 변수명을 정의한다.

```javascript
const refEditor = ref(null); // template의 ref의 값과 동일한 변수 선언
```

ref(null)과 같이 ref() 함수에 null을 파라미터로 설정한다.

> ref 값을 직접 참조할 땐 value 속성으로 접근했었지만, 템플릿에 사용하는 경우에는 value 속성 접근없이 ref 값 자체를 사용한다. 또한 setup 훅에서 값을 반환할 때 속성명을 변경하여 사용할 수 있다.

## mount하기

onMounted() 함수를 사용하여 edior를 마운트 시킨다. el 속성에 값을 설정할 때에ㅡ는 refEditor.value와 같이 value 속성을 사용한다.

```javascript
onMounted( () => { 
   const xeditor = new Editor({
    el: refEditor.value,
    height: "500px",
    initialEditType: "markdown",
    previewStyle: "vertical",
  });
})
```

## 옵션

**height** 문자열. 높이. ex) 300px | auto **initialEditType** 보여줄 초기 타입. ex) markdown | wysiwyg **previewStyle** Markdown 모드의 preview 스타일. tab | vertical **usageStatistics** hostname을 알려달라. Editor를 사용하는지 알고 싶다. true | false **initialValue** 초기값

## syntax highlighter

설치

```shell
npm install @toast-ui/editor-plugin-code-syntax-highlight
```

import

```javascript
import 'prismjs/themes/prism.css';
import '@toast-ui/editor-plugin-code-syntax-highlight/dist/toastui-editor-plugin-code-syntax-highlight.css';

import codeSyntaxHighlight from '@toast-ui/editor-plugin-code-syntax-highlight';
```

설정

```javascript
const editor = new Editor({
  // ...
  plugins: [[codeSyntaxHighlight, { highlighter: Prism }]]
});
```

## Color Syntax Plugin

설치

```shell
npm install @toast-ui/editor-plugin-color-syntax
```

import

```javascript
import 'tui-color-picker/dist/tui-color-picker.css';
import '@toast-ui/editor-plugin-color-syntax/dist/toastui-editor-plugin-color-syntax.css';

import colorSyntax from '@toast-ui/editor-plugin-color-syntax';
```

사용

```javascript
const editor = new Editor({
  // ...
  plugins: [colorSyntax]
});
```

## 참조

[Toast UI Editor](https://www.npmjs.com/package/@toast-ui/editor)

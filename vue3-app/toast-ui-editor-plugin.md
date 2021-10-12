# Toast UI Editor Plugin 개발

* Toast UI Editor는 Editor와 Viewer를 제공한다.
* Viewer로도 Editor로 동작하다록 컴포넌트를 만든다.
* Viewer와 Editor는 스위칭할 수 없다.

Viewer로 사용할 때

```javascript
<template>
  <toast-editor  viewer "></toast-editor>
</template>
```

Editor로 사용할 때

```javascript
<template>
  <toast-editor  "></toast-editor>
</template>
```

## 이미지 업로드

## Props

| property        | type    | description | default  |
| --------------- | ------- | ----------- | -------- |
| height          | string  | 에디터 높이      | auto     |
| initialEditType | string  | 에디터 유형      | markdown |
| previewStyle    | string  | 미리보기 유형     | vertical |
| initialValue    | string  | 초기값         | ""       |
| viewer          | boolean | 뷰어 여부       | false    |

## Method

| Method      | return or Parameter Type | description      |
| ----------- | ------------------------ | ---------------- |
| getHTML     | string                   | HTML을 반환         |
| setHTML     | string                   | HTML로 값을 채움      |
| getMarkdown | string                   | markdown을 반환     |
| setMarkdown | string                   | markdown으로 값을 채움 |

## 참조

[플러그인](https://github.com/nhn/tui.editor/blob/master/docs/ko/plugin.md)

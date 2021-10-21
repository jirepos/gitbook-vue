# 시작하기 

어플리케이션의 진입점인 index.html을 생성하고 컴포넌트들을 정의하고 컴포넌트에서 다른 컴포넌트를 임포트(import)한다. 

```shell
🗂️vue/
  🗂️public/
  🗂️src/
    🗂️assets/
    🗂️components/  
      📄SubComponent.vue   // 하위 컴퍼넌트
    📄App.vue  // Main Component 
    📄main.js  // Main Script
  📄.gitignore
  📄index.html   // Entry Point 
  📄package.json
  📄vite.config.js
```

# 어플리케이션 진입점

Vite를 사용하여 Vue를 개발할 때 index.html은 프로그램의 진입점이다


* 개발하는 중에 Vite는 서버이고 index.html은 애플리케이션에 대한 진입점이다
* Vite는 index.html을 module graph의 일부이고 source code로서 취급한다
* 그것은 JavaScript source code를 참조하는 \<script type=”module” src=”…”\>을 해결한다
* static http server들과 비슷하게, Vite는 root directory라는 개념을 가진다.
* 나머지 문서들 전체에서 \<root\>로서 참조되는 것을 볼 수 있다.



HMR(Hot Module Replacement) 기능이있는 프레임 워크는 API를 활용하여 페이지를 다시로드하거나 애플리케이션 상태를 날리지 않고 즉각적이고 정확한 업데이트를 제공 할 수 있습니다. Vite는 Vue Single File Component와 React Fast Refresh을 위한 HMR 통합을 제공한다.

# index.html 
프로젝트 루트에 index.html을 생성한다. 메인 컴포넌트가 마운트 될 \<div\> 태그를 정의한다.

```html
<div id="app"></div>
```

메인 스크립트인 main.js를 로드할 \<script\> 태그를 추가한다. main.js는 /src 디렉토리에 생성해야 한다
```html
 <script type="module" src="/src/main.js"></script>
```


# 컴포넌트 
브라우저 환경에서 Vue를 정의하여 사용할 수는 있지만 복잡한 앱은 vue 확장자를 가지는 Single File Component 방식으로 개발한다. 아래는 SFC의 기본 구조이다.

```html
<template>
</template>

<script setup>
</script>

<style>
</style>
```





\<template\> 태그는 화면이 레이아웃을 구성하기 위해 사용한다. 화면 레이아웃을 제어할 JavaScript 코드는 \<script\> 태그 안에 작성한다. 각 컴포넌트에만 적용할 CSS는 \<style\> 태그 안에 정의한다.


# 하위 컴포넌트 

Vue에서는 모든 것이 컴포넌트이다. 화면을 기능을 중심으로 컴포넌트로 나눈다. 이 튜토리얼에서는 정의된 기능이 없기 때문에 간단히 하위 컴포넌트를 하나 만든다.

> 컴포넌트들은 보통 src/components 디렉토리 하위에 둔다.


src/components 디렉터리에 e02 디렉터리를 생성한다. 그 안에 ChildComp.vue 파일을 생성한다. \<template\> 태그에는 하나의 DIV 요소만 정의한다. Vue 3 버전에서는 루트에 다수의 요소를 정의하는 것이 가능하지만, 여기서는 간단히 하나만 정의한다.


```html
<template>
  <div> hello </div> 
</template>
<script setup>
</script>
<style scoped>
</style>
```

# 메인 컴포넌트 

메인 컴포넌트인 App.vue파일을 src 폴더에 생성한다. App.vue는 Single File Component 형태로 작성한다. 

이제 생성한 하위 컴포넌트를 임포트(import)해야 한다. App.vue에 컴포넌트를 import한다.

```html
<script setup>
import SubComponent from './components/e02/ChildComp.vue'
</script>
```


template에 컴포넌트를 추가한다. 임포트한 컴포넌트를 Template에 배치한다.

```html
<template>
   <child-comp/>
</template>
```
컴포넌트의 이름은 다음 규칙을 따른다.

* 영문 소문자만 사용
* 하이픈 이용(여러 단어를 하이픈으로 연결해 사용)


컴포넌트를 스트링 템플릿이나 단일 파일 컴포넌트로 정의하는 경우 두 가지 방식으로 정의할 수 있다.




**kebab-case로 정의하기**

```javascript
app.component('my-component-name', {
  /* ... */
})
```


**파스칼케이스로 정의하기**

```javascript
app.component('MyComponentName', {
  /* ... */
})
```



# Main Script
모든 Vue 어플리케이션은 createApp 함수를 사용하여 새로운 어플리케이션 인스턴스를 생성하여 시작한다.

```javascript
const app = Vue.createApp({ /* options */ })
```

인스턴스가 생성되면, mount 메소드에 컨테이너를 전달하여 mount할 수 있다. 예를 들어 \<div id=”app”\>\<div\>에 Vue 어플리케이션을 마운트 시키고 싶다면 #app를 전달해야 한다.

createApp에 전달된 옵션은 루트 컴포넌트를 구성하는데 사용된다. 이 컴포넌트는 어플리케이션을 mount할 때, 렌더링의 시작점으로 사용된다.

메인 스크립트를 src/ 디렉토리에 생성하고 다음과 코드를 작성한다. 아래 코드에서 루트 컴포넌트인 App 콤포넌트를 임포하하고 생성한다.

```javascript
import { createApp } from 'vue'
import App from './App.vue'

createApp(App).mount('#app')
```


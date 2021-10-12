# Vite

Vite (opens new window)는 네이티브 ES 모듈을 가져오는 방식으로 코드의 빠른 제공을 가능하게 하는 Web 개발 빌드 도구이다.

Vue 프로젝트에서 터미널에 다음 명령을 실행하여 Vite로 빠르게 설정할 수 있다.

**npm**:

```shell
$ npm init vite-app <project-name>
$ cd <project-name>
$ npm install
$ npm run dev
```

**Yarn**:

```shell
$ yarn create vite-app <project-name>
$ cd <project-name>
$ yarn
$ yarn dev
```

위에서 설명한 방식으로 프로젝트를 생성하면 vite가 1.x 버전이 설치된다. vite.config.js 파일을 설정할 때 Vite 사이트에서는 2.x 버전으로 설명을 하고 있다. 따라서 2.x 버전을 사용하려면 다음과 같이 설치한다.

```shell
# npm 6.x
npm init @vitejs/app my-vue-app --template vue

# npm 7+, extra double-dash is needed:
npm init @vitejs/app my-vue-app -- --template vue

# yarn
yarn create @vitejs/app my-vue-app --template vue
```

vuex 최신버전을 설치한다

```shell
yarn add vuex@next --save
```

## 알아야 할 몇가지 Vue Vite 특징들

### Bundling your project for production

Vite에 대한 목표는 Vue 개발 및 프러덕션을 가능한 쉽게하기 위한 것이다. 개발 중에 번들링이 없지만, 프러덕션을 위해 프로젝트를 번들링하는 것은 매우 쉽다.

당신이 해야 하는 것은 오직 npm run build를 실행하는 것이다.

```shell
npm run build
or
npx build 
```

package.json을 보면, vite build를 호출하고 있는 것을 볼 수 있다. 그것은 다른 빌드 프로세스들과 마찬가지로 Vue project를 번들할 것이고 저장될 dist folder를 준비할 것이다.

### Managing asset URLS

다른 Vue project setups과 마찬가지로, Vite는 assets을 참조하는 두 가지 방식을 제공한다.

* Absolutely - public folder를 사용한다. 이러한 assets들은 /file.extension으로 참조되고 프로젝트가 빌드 될 때 dist folder의 root에 복사된다.
* Relatively - 예를 들어, src/assets folder에 있는 파일들은 그 폴더의 파일 구조에 근거하여 상대적으로 접근된다. 이것은 프로젝트가 빌디 될 때 dist folder에 \__assets으로써 전체 폴더가 위치된다.

### Built-in Typescript support

Vue3에서 가장 큰 변화 중 하나는 Typescript를 사용하여 core library를 재작성했다는 것이다. type checking과 더 나은 오류 메시지는 허용한느 사용하는 IDE에 달렸다.

Vue Vite는 .ts 파일과 SFC의 \<script lang = "ts"> 모두에 대해 내장 Typescript 지원을 제공하여 Vue 3와 원활하게 통합된다.

## Getting Started

Vite(발음은 /vit/)는 build tool이다. 두가지 주요한 파트가 있다.

* 매우 빠른 Hot Module Replacement(HMR)과 같은 풍부한 기능 향상을 제공하는 dev server
* Rollup을 사용한 코드를 번들링하는 build command

### Browser Support

* 개발을 위해서는 native ESM dynamic import support가 요구된다.
* production을 위해서는 native ESM via scripts tags를 지원하는 브라우저들을 타겟으로 한다.

### Scaffolding Your First Vite Project

> Vite는 Node.js 12 이상의 버전을 요구한다.

**NPM**

```shell
 npm init @vitejs/app
```

**Yarn**

```shell
 yarn create @vitejs/app
```

직접적으로 프로젝트 이름을 명시할 수 있다.

```shell
# npm 6.x
npm init @vitejs/app my-vue-app --template vue

# npm 7+, extra double-dash is needed:
npm init @vitejs/app my-vue-app --template vue

# yarn
yarn create @vitejs/app my-vue-app --template vue
```

Supported template presets include:

* vanilla
* vue
* vue-ts
* react
* react-ts
* preact
* preact-ts
* lit-element
* lit-element-ts

### index.html and Project Root

index.html은 public 내부에 숨겨지는 것 대신에 전면 중앙에 있다. 개발하는 중에 Vite는 서버이고 index.html은 애플리케이션에 대한 진입점이다. Vite는 index.html을 module graph의 일부이고 source code로서 취급한다. 그것은 JavaScript source code를 참조하는 \<script type="module" src="...">을 해결한다.

심지어 inline \<script type="module">와 \<link href>을 통해 참조되는 CSS들 조차도 Vite-specific features을 사용할 수 있다. 게다가, index.html 내부의 URL들은 자동적으로 rebase된다.

따라서 특별한 %PUBLIC_URL% phacleholder들은 필요하지 않다.

static http server들과 비슷하게, Vite는 root directory라는 개념을 가진다. 나머지 문서들 전체에서 \<root>로서 참조되는 것을 볼 수 있다. 애플리케이션에서 절대 URL들은 base로서 project root를 사용하여 파악될 것이다. 따라서, 마치 보통의 static file server에서 작업하는 것처럼 코드를 작성할 수 있다.

Vite는 또한 루트를 벗어난 file system의 위치를 처리할 의존성을 다둘 수 있다. 그것은 monorepo-based seup에서도 사용할 수 있게 한다.

**대체 Root 명시하기** vite를 실행하는 것은 root로서 현재 작업 디렉토리를 사용하여 dev server를 시작한다. vite server som/sub/dir으로 대체 root를 명시할 수 있다.

### Command Line Interface

Vite가 설치된 프로젝트에서 npm script에 vite binary를 사용할 수 있다. npx vite를 사용하여 직접적으로 그것을 실행할 수 있다.

```json
{
  "scripts": {
    "dev": "vite", // start dev server
    "build": "vite build", // build for production
    "serve": "vite preview" // locally preview production build
  }
}
```

추가적으로 --port 또는 --https와 같은 CLI 옵션들을 명시할 수 있다. npx vite --help를 실행한다.

## Fetures

### NPM Dependency Resolving and Pre-Bundling

Native ES imports는 아래와 같은 bare moudle imports를 지원하지 않는다.

```javascript
import { someMethod } from 'my-dep'
```

위의 코드는 브라우저에서 에러를 던질 것이다.

Vite 는 제공되는 모든 소스 파일에서 bare module imports를 발견할 것이고 다음을 수행할 것이다.

* page loading speed를 향상하기 위해서 Pre-bundle할 것이고 CommonJS/UMD modules들을 ESM으로 변환할 것이다. Pre-bundling 단계는 esbuid로 수행될 것이고 Vite의 cold start time을 JavaScript 기반의 bundler보다 명확히 빠르게 할 것이다.
* 브라우저가 그것들을 적절히 import할 수 있도록 imports를 /node_modules/.vite/my-dep.js?v-f3sf2ebd와 같은 유효한 URL로 다시 작성할 것이다.

Vite는 HTTP header를 통해서 의존 요청들을 캐쉬할 것이다. 그래서 지역적으로 의존성을 edit/debug하고 싶으면 , [여기](https://vitejs.dev/guide/dep-pre-bundling.html#file-system-cache)를 따라가라.

### Hot Module Replacement

HMR 기능이있는 프레임 워크는 API를 활용하여 페이지를 다시로드하거나 애플리케이션 상태를 날리지 않고 즉각적이고 정확한 업데이트를 제공 할 수 있습니다.

Vite는 Vue Single File Component와 React Fast Refresh을 위한 HMR 통합을 제공한다. 또한 @prefresh/vite를 통한 Preact에 대한 공식적인 통합이 있다.

### TypeScript

Vite는 .ts 파일들에 대한 importing을 지원한다. Vite는 오직 .ts 에 대한 transpilation을 수행한다. type checking은 수행하지 않는다.

### Client Types

Vite의 디폴트 types은 그것의 Node.js API를 위한 것이다. Vite에 클라이언트 사이드의 코드 환경을 넣기 위해서는 vite/client를 tsconfig의 compilerOptions.types에 추가한다.

```json
{
  "compilerOptions": {
    "types": ["vite/client"]
  }
}
```

이것은 다음의 type 추가를 제공할 것이다.

* Asset imports (e.g. importing an .svg file)
* Types for the Vite-injected env variables on import.meta.env
* ypes for the HMR API on import.meta.hot

### Vue

Vite는 최고의 Vue 지원을 제공한다.

* Vue 3 SFC support via @vitejs/plugin-vue
* Vue 3 JSX support via @vitejs/plugin-vue-jsx
* Vue 2 support via underfin/vite-plugin-vue2

### CSS

.css를 임포트하는 것은 HMR 지원을 가지고 \<style> tag를 사용하여 page에 그것의 컨텐트를 주입할 것이다.

**@import Inlining and Rebasing** pstcss-import를 통하여 CSS @import inlining을 지원하기 위해 미리 정의되어 있다. Vite aliases는 또한 CSS @import에 대해서도 존중된다. 게다가, 심지어 imported 된 파일들도 다른 디렉토리들에 있다하여도 모든 CSS url() 참조들은 자동적으로 rebase된다.

@import 별칭들과 URL rebasing은 또한 Saas와 Less files을 위해 지원된다.

### Static Assets

정적 자산들이 importing하는 것은 처리된 공개된 URL로 반환될 것이다.

```javascript
import imgUrl from './img.png'
document.getElementById('hero-img').src = imgUrl
```

특별한 질의들은 자산들이 어떻게 로드될지를 수정할 수 있다.

```javascript
// Explicitly load assets as URL
import assetAsURL from './asset.js?url'
```

```javascript
// Load assets as strings
import assetAsString from './shader.glsl?raw'
```

```javascript
// Load Web Workers
import Worker from './worker.js?worker'
```

```javascript
// Web Workers inlined as base64 strings at build time
import InlineWorker from './worker.js?worker&inline'
```

### JSON

JSON 파일들은 또한 직접적으로 import될 수 있다. named import 또한 지원된다.

```javascript
// import the entire object
import json from './example.json'
// import a root field as named exports - helps with treeshaking!
import { field } from './example.json'
```

## References

[Vite](https://vitejs.dev/guide/#scaffolding-your-first-vite-project)\
[Vite Getting Started](https://vitejs.dev/guide/#using-unreleased-commits)\
[Vue Vite build](https://learnvue.co/2020/12/setting-up-your-first-vue3-project-vue-3-0-release/)

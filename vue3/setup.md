# Setup

Vue.js Version 3를 사용하고 빌드 도구로는 Vite v2.x를 사용할 것이다.

## 먼저 설치할 것들

Vue는 Node.js와 yarn을 필요로한다. yarn은 필수는 아니다.

* Node.js를 먼저 설치한다.
* Yarn을 설치한다.

## vue 프로젝트 생성

Vue 프로젝트를 생성하고 빌드하기 위해 Vite를 사용할 것이다. vite는 네이티브 ES 모듈을 가져오는 방식으로 코드의 빠른 제공을 가능하게 하는 Web 개발 빌드 도구이다.

Vue 프로젝트에서 터미널에 다음 명령을 실행하여 Vite로 빠르게 설정할 수 있다.

```shell
yarn create @vitejs/app vue --template vue
```

## 프로젝트 구조

명령어를 실행한 폴더 하위에 vue 폴더가 생성되고 그 구조는 다음과 같다.

```
🗂️vue/
  🗂️public/
  🗂️src/
    🗂️assets/
    🗂️components/
    📄App.vue
    📄main.js
  📄.gitignore
  📄index.html
  📄package.json
  📄vite.config.js
```

**src**\
Vue 소스들을 여기에 둔다.

**public**\
html과 javascsript와 같은 정적 파일을 여기에 둔다. 이 폴더에는 단히 복사되는 정적 자원들을 둔다. 이 폴더에 있는 정적 파일들은 webpack과 같은 번들러에서 처리하지 않는다.

**assets**\
css, image 등의 정적 자원들을 이 폴더에 둔다. 이 폴더에 있는 파일들은 webpack과 같은 번들러들이 처리하고 의존성을 관리한다.

**components**\
Vue의 Single File Component 소스 파일들을 이 폴더에 둔다.

**index.html**\
Vite로 개발할 때에는 이 파일이 entry point이다. 이 파일을 호출하여 앱을 실행한다.

**App.vue**\
Vue의 메인 컴포넌트이다.

**vite.config.js**\
Vite가 사용하는 설정파일이다.

## 의존성 설치

프로젝트를 생성하고 프로젝트 구조를 이해했으면 이제 생성한 디렉토리로 이동한다. VSCode의 Terminal 창을 열고 작업한다.

```shell
cd vue
```

package.json에 설정된 의존성을 설치한다. 단순히 yarn을 입력하여 실행한다.

```shell
yarn
```

vue 프로젝트에 node_modules 폴더가 생성되고 그 안에 프로젝트가 의존하는 패키지들이 설치된다.

```
vue/
  node_modules/
```

## 프로젝트 빌드

Vite에 대한 목표는 Vue 개발 및 프러덕션을 가능한 쉽게하기 위한 것이다. 개발 중에 번들링이 없지만, 프러덕션을 위해 프로젝트를 번들링하는 것은 매우 쉽다.

우리가 해야 하는 것은 오직 npm run build를 실행하는 것이다.

```shell
npm run build
or
npx build 
```

빌드가 완료되면 프로젝트 루트에 dist라는 폴더가 생성되고 번들링된 파일들이 이 안에 놓인다.

```shell
🗂️vue/
  🗂️dist/
    🗂️assets/
    📄index.html
```

## 실행하기

Vite로 개발할 때에는 vite가 제공하는 서버를 사용하여 테스트한다. 다음과 같이 입력한다.

```shell
npm run dev
or 
npx vite 
```

빌드한 이후에 빌드한 파일을 테스트하려면 다음과 같이 빌드하고 preview 옵션으로 실행한다.

```shell
npm run build
or
npx vite preview
```

실행하면 다음과 같이 터미널에 출력될 것이다.

```shell
> npx vite preview
  vite v2.3.2 build preview server running at:

  > Local: http://localhost:5000/
  > Network: use `--host` to expose
```

URL을 Ctrl + Click하면 브라우저가 열리고 다음과 같은 화면을 볼 수 있다.

중지 시키고 싶으면 Ctrl + C 를 누른다.

> git에서 프로젝트를 clone한 경우에는 package.json에서 의존하는 모듈들이 설치되어 있지 않다. 'yarn install'을 실행하여 설치한다.

## References

[Vue 프로젝트 구조](https://developer.mozilla.org/ko/docs/Learn/Tools_and_testing/Client-side_JavaScript_frameworks/Vue_getting_started)

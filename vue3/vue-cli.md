# Vue CLI

프로젝트를 생성할 때 Vite를 사용할 것이다. Vue CLI를 사용하면 Vite 버전이 1.x 버전이 설치된다. 현재 시점으로 Vite 2.x를 사용하여 프로젝트를 생성할 것이므로 Vue CLI는 참고용으로만 작성해 놓았다.

## Getting Started

**install**

```shell
npm install -g @vue/cli
# OR
yarn global add @vue/cli
```

**Create a project**

어떤 preset을 선택할지를 물어본다. Vue 3 Preview를 선택한다.

Yarn을 사용할지 NPM을 사용할지를 물어본다. Yarn을 선택한다.

프로젝트 생성이 완됴되면 다음과 같은 결과를 볼 수 있다.

## Installation

> **이전 버전에 관련된 경고**
>
> global하게 설치된 이전 버전을 삭제하려면 npm uninstall vue-cli -g 또는 yarn global remove vue-cli을 사용한다.

> **Node Version Requirement**
>
> Node.js 8.9. 이상을 요구하는데 10 이상이 추천된다.

**설치**

```shell
npm install -g @vue/cli
# OR
yarn global add @vue/cli
```

**업그레이드**

```shell
npm update -g @vue/cli
```

**OR**

```shell
yarn global upgrade --latest @vue/cli
```

## Creating Project

새로운 프로젝트를 생성하기 위해서는 다음과 같이 실행한다.

```shell
vue create hello-world
```

## Using the GUI

graphical interface를 사용하여 프로젝트를 생성하거나 관리할 수 있다.

```shell
vue ui
```

위 명령어어로 GUI가 있는 윈도우를 오픈할 수 있다. 

## ClI Service

Vue CLI project 내부에 @vue/cli-service가 vue-cli-service라는 이름의 바이너리를 설치한다. 직접 vue-cli-service로 바이너리에 접근할 수 있다.

디폴트 preset을 사용한 프로젝틔 package.json에서 아래의 내용을 볼 수 있다.

```json
{
  "scripts": {
    "serve": "vue-cli-service serve",
    "build": "vue-cli-service build"
  }
}
```

npm이나 yarn을 사용하여 이 스크립트들을 실행할 수 있다.

```shell
npm run serve
# OR
yarn serve
```

npx를 사용할 수 있다면, 바이너리를 직접적으로 실행할 수 있다.

```shell
npx vue-cli-service serve
```

## Vue CLI Project Structure

디폴트 디렉토리 스트럭처를 살펴보자.

```
your_app/
   node_modules/     # Vue를 빌드하기 위해 요구되는 의존성과 라이브러리들 
   public/           # webpack을 통해 관리되지 않는 정적 자원들 
     index.html      
   src/              # Vue applicaiton의 소스 코드 
     assets/         # 이미지나 font 같은 정적 자원들 
     components/     # 애플리케이션에서 사용되는 컴포넌트들
     router/         # Router 설정들을 모아 두는 곳
     store/          # Vuex 설정파일들을 모아 두는 곳 
     views/          # application의 다른 뷰 또는 페이지에 대한 파일들 
     App.vue         # 최상위 root 컴포넌트 
     main.js         # 어플리케이션에 필요한 요소들을 load하여 렌더링한 후 DOM에 마운트
   .gitignore        # git viersion control 파일 
   babel.config.js   # babel 설정 파일
   package.json      # npm 설정 파일 
```

**public/index.html** App 컴포넌트가 마운트될 div 영역을 볼 수 있다.

```html
<div id="app"></div>
<!-- built files will be auto injected -->
```

**Build Process** 다음의 명령어를 실행하여 app을 빌드한다.

```shell
npm run build
```

## References

[Vue Cli](https://cli.vuejs.org)\
[Vue Cli Installation](https://cli.vuejs.org/guide/installation.html)\
[CLI Service](https://cli.vuejs.org/guide/cli-service.html#checking-all-available-commands)\
[Vue CLI Project](https://5balloons.info/project-tour-of-vue-cli-app/)

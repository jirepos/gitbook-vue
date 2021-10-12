# 프로젝트 생성

Vue 3 기반 Web App 프로젝트를 진행할 것이다. Vue.js의 기초적인 내용은 다루지 않을 것이지만 필요에 따라 설명을 할 수도 있다. 프로젝트 생성부터 개발 표준 수립 및 공통 기능 개발 등의 Web App 개발 시에 고려해야할 사항들을 모두 다룰 것이다. 그러나 어느 범위까지 정리할지는 결정하지 못했다. 이 프로젝트는 경험을 축적하기 위한 용도이므로 프로젝트를 완료하고 나면 아마도 생각이 바뀔 수도 있을 것이다. 이 글은 수시로 변경될 수도 있다. 그리고 서버에 대한 부분은 자세히 다루지 않는다.

이 글에서는 Spring Boot 프로젝트에서 Font-end 개발을 위해 프로젝트를 생성하고 개발환경에서 서버를 실행한 다음에 브라우저를 열고 HelloWorld 화면을 볼 것이다.

## project 생성

Spring Boot App에서 Vue3를 사용하려고 한다. Spring Boot App 프로젝트 루트에서 다음을 입력한다.

```shell
yarn create @vitejs/app vue --template vue
```

다음과 같이 디렉터리가 생성된다.

```shell
🗂️ project_root
  🗂️ src
     🗂️ main
        🗂️ java
  🗂️ vue
```

## node_modules 설치

vue 디렉터리로 이동한다.

```shell
cd vue
```

vue 디렉터리에서 다음을 입력하여 필요한 Node 모듈들을 설치한다.

```shell
yarn install
```

## 프로젝트 테스트

yarn vite를 사용하여 앱을 실행한다.

```shell
yarn dev
```

실행하면 터미널에 다음과 같이 메시지가 출력된다.

```shell

  vite v2.4.1 dev server running at:

  > Local: http://localhost:3000/
  > Network: use `--host` to expose

  ready in 448ms.
```

브라우저를 열고 다음을 입력한다.

```shell
http://localhost:3000/
```

다음과 같이 화면이 표시될 것이다. ![](https://images.velog.io/images/latte_h/post/ec1a90f6-cbb8-48f2-beb5-032d50a35a23/image.png)

### 프로젝트 구조

```shell
🗂️project_root
  🗂️src         // Java Maven Project Strucutre
     🗂️main
        🗂️java  // Java Source
        🗂️resources // Static Resources 
           🗂️mapper
             🗂️oracle
                🗂️ blog  // blog 모듈 매퍼 
                    📄BlogMapper.xml 
           🗂️translation  // 다국어 리소스 
              📄trans-message.yml  
              📄trans-message_ko.yml.yml                    
              📄trans-message_en.yml.yml                    
           🗂️public  // 개발과 관련없는 static resources
              📄help.html
           🗂️static  // JavaScipt, CSS, Image 등 
              // Vue를 사용하기 때문에 거의 사용할 일이 없을 듯 
              🗂️resources // 정적 리소스 
                🗂️css   // CSS 
                🗂️js    // Javascript
                   🗂️lib // 외부 Javascript Library
                   🗂️api // 모듈 Api
                🗂️image // Image
           🗂️templates  // Jsp와 같은 Dynamic Resource 
     🗂️test    
        🗂️java  // Test Case
          🗂️com
             🗂️sogood 
                🗂️ core // src/main/java package 구조와 동일한 패키지 구조 
                    📄CoreUtilTest.java   // com/soogd/core/CoreUtil.java의 테스트 케이스 
  🗂️vue // Vue Project Root
     🗂️public
     🗂️src // Vue source
        🗂️api        // API for business logic 
          🗂️core     // 공통 및 업무와 무관한 Javascript
          🗂️biz      // 모듈 JavaScript
            🗂️blog   // 블로그 모듈 , Typescript와 Javascript 사용
              🗂️usecase // UseCase, java의 Service와 같음 
                 📄BlogManagerUseCase.ts  // 모듈명 접두사, UseCase 접미사 사용
                 📄BlogPostUseCase.ts
              🗂️repository // Java의 DAO 또는 Mapper와 같음           
                📄BlogMangerRepository.ts  // 모듈명 접두사, Repository 접미사 사용
                📄BlogPostRepository.ts
        🗂️assets     // for static resources. eg. image, css, icons, fonts, style
           🗂️image
           🗂️css 
        🗂️components // components
           🗂️common   // 의미별로 묶어서 디렉터리 구성
              📄Calendar.vue
              📄DatePicker.vue         
        🗂️plugins    // Vue plugins
           📄FetchPlugin.js
        🗂️routes     // routers
        🗂️store      // Vuex
        🗂️views      // views  
          🗂️blog
             📄BlogMain.vue
             📄BlogEdit.vue
             📄BlogList.vue
     📄.gitignore
     📄index.html
     📄package.json
     📄vite.config.js
```

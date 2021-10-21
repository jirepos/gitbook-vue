# 프로젝트 생성 
template를 이용해서 vue 프로젝트를 생성한다. 
```shell
yarn create @vitejs/app vue --template vue
```

디렉터릴 이동 
```shell
cd vue
```

의존성을 설치한다. 
```shell
yarn
```

# 프로젝트 실행
디레터리로 이동한다. 
```shell
cd vue
```
yarn vite를 이용하여 프로젝트를 실행한다. 
```shell
yarn vite
```



# 프로젝트 구조


```shell
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



**src**
Vue 소스들을 여기에 둔다.

**public**
html과 javascsript와 같은 정적 파일을 여기에 둔다. 이 폴더에는 단히 복사되는 정적 자원들을 둔다. 이 폴더에 있는 정적 파일들은 webpack과 같은 번들러에서 처리하지 않는다.

**assets**
css, image 등의 정적 자원들을 이 폴더에 둔다. 이 폴더에 있는 파일들은 webpack과 같은 번들러들이 처리하고 의존성을 관리한다.

**components**
Vue의 Single File Component 소스 파일들을 이 폴더에 둔다.

**index.html**
Vite로 개발할 때에는 이 파일이 entry point이다. 이 파일을 호출하여 앱을 실행한다.

**App.vue**
Vue의 메인 컴포넌트이다.

**vite.config.js**
Vite가 사용하는 설정파일이다.



# 프로젝트 빌드 
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

# 실행하기
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


# Vite
Vite (opens new window)는 네이티브 ES 모듈을 가져오는 방식으로 코드의 빠른 제공을 가능하게 하는 Web 개발 빌드 도구이다. Vite에 대해서 알아야 할 몇가지 특징을 살펴보자. 

**Bundling your project for production**
Vite에 대한 목표는 Vue 개발 및 프러덕션을 가능한 쉽게하기 위한 것이다. 개발 중에 번들링이 없지만, 프러덕션을 위해 프로젝트를 번들링하는 것은 매우 쉽다.

당신이 해야 하는 것은 오직 npm run build를 실행하는 것이다.

```shell
npm run build
or
npx build 
```

package.json을 보면, vite build를 호출하고 있는 것을 볼 수 있다. 그것은 다른 빌드 프로세스들과 마찬가지로 Vue project를 번들할 것이고 저장될 dist folder를 준비할 것이다.




**Managing asset URLSPermalink**
다른 Vue project setups과 마찬가지로, Vite는 assets을 참조하는 두 가지 방식을 제공한다.

* Absolutely - public folder를 사용한다. 이러한 assets들은 /file.extension으로 참조되고 프로젝트가 빌드 될 때 dist folder의 root에 복사된다.
* Relatively - 예를 들어, src/assets folder에 있는 파일들은 그 폴더의 파일 구조에 근거하여 상대적으로 접근된다. 이것은 프로젝트가 빌디 될 때 dist folder에 __assets으로써 전체 폴더가 위치된다.



**Built-in Typescript support**


Vue3에서 가장 큰 변화 중 하나는 Typescript를 사용하여 core library를 재작성했다는 것이다. type checking과 더 나은 오류 메시지는 허용한느 사용하는 IDE에 달렸다.

Vue Vite는 .ts 파일과 SFC의 \<script lang = “ts”\> 모두에 대해 내장 Typescript 지원을 제공하여 Vue 3와 원활하게 통합된다.


## vite.config.js
모듈을 임포트할 때 상대경로를 사용하면 매우 복잡하다. 절대 경로를 사용할 수 있도록 다음과 같이 vite.confg.js 파일을 수정한다. 

```shell
import { defineConfig } from 'vite'
import vue from '@vitejs/plugin-vue'
import path from 'path'

// https://vitejs.dev/config/
export default defineConfig({
  plugins: [vue()],
  resolve: {
    alias: {
      '@': path.resolve(__dirname, '/src')
    }
}
})
```


이제 다음과 같이 import할 수 있다. 
 ```shell
 import User from "@/api/biz/User";
 ```

 
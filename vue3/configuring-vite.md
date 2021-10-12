# Configuring Vite

> "npm init vite-app "을 사용하여 vue 프로젝트를 생성하면 "^1.0.0-rc.13" 버전이 설치된다. https://github.com/vitejs/vite에 보면 현재 버전은 2.1.5이다. vite 사이트에서 outDir의 경로를 바꾸기 위해서 build { outDir: ""}과 같이 설정라고 나오는데 실제로는 동작하지 않았다. 문서에서 설명하는 방식은 2.x 버전을 기준으로 한다.

**빌드는 다양한 build config options을 통해 커서터마이징 될 수 있다. 특별히 build.rollupOptions을 통해 내재되어 있는 Rollup options을 조정할 수 있다.**

```javascript
// vite.config.js
module.exports = {
  build: {
    rollupOptions: {
      // https://rollupjs.org/guide/en/#big-list-of-options
    }
  }
}
```

## Config File

### Config File Resolving

명령어 라인으로부터 vite를 실행할 때, Vite는 자동적으로 프로젝트 루트에서 vite.config.js 라는 이름의 파일을 찾으려고 한다.

가장 기본적인 config file은 아래와 같다.

```javascript
// vite.config.js
export default {
  // config options
}
```

type:module을 통해 native Node ESM을 사용하지 않더라도 config file에서 ES modules syntax를 사용하는 것을 지원한다. 이 경우에 로드되기 전에 config 파일은 자동 pre-processed이다. 또한 명확히 config 파일을 명시할 수 있다.

```shell
vite --config my-config.js
```

### Config Intellisense

Vite는 TypeScript 타이핑과 함께 제공되므로 jsdoc 유형 힌트를 사용하여 IDE의 지능을 활용할 수 있다.

```javascript
/**
 * @type {import('vite').UserConfig}
 */
const config = {
  // ...
}

export default config
```

또는 jsdoc 어노테이션 없이도 intellisense를 제공해야하는 defineConfig 도우미를 사용할 수 있다.

```javascript
import { defineConfig } from 'vite'

export default defineConfig({
  // ...
})
```

Vite는 TS 구성 파일도 직접 지원합니다. defineConfig 도우미와 함께 vite.config.ts를 사용할 수도 있다.

### Conditional Config

구성이 명령 (serve 또는 build) 또는 사용중인 모드에 따라 옵션을 조건부로 결정해야하는 경우 대신 함수를 내보낼 수 있다.

```javascript
export default ({ command, mode }) => {
  if (command === 'serve') {
    return {
      // serve specific config
    }
  } else {
    return {
      // build specific config
    }
  }
}
```

## Shared Options

옵션에 대한 자세한 사항은 [여기](https://vitejs.dev/config/#json-namedexports)를 참조한다.

### root

프로젝트의 root directory는 절대 경로일 수 있고 상대 경로일 수도 있다. index.html이 있는 곳을 의미한다.

### base

개발 또는 프러덕션에서 서비스 될 때 기본 공개 경로이다.

* 절대 URL 경로이름, 예. /foo/
* Full URL, 예. https://foo.com/
* 빈 문자열 또는 ./

```javascript
export default  {
   base: '/ekp', 
}
```

위와 같이 base에 /ekp를 설정하면 다음과 같이 index.html이 /ekp에 추가된다.

```html
<head>
<script type="module" src="/ekp/_assets/index.10fb19ed.js"></script>
<link rel="stylesheet" href="/ekp/_assets/style.0637ccc5.css">
</head>
```

### publicDir

보통의 static assets을 제공하기 위한 디렉토리이다. 기본값은 "public"이다. 이 디렉토리에 있는 파일들은 개발중에 / 에서 제공되고 outDir의 root에 복사된다. 항상 변형없이 현재 그대로 복사된다. 값은 프로젝트 루트의 절대 경로일 수도 있고 상대 경로일 수도 있다.

## Build Options

### build.outDir

기본값은 "dist"이다. 아웃풋 디렉토리를 명시한다. project root에 상대적이다.

Vite 버전이 2.x를 사용한다면 다음곽 같이 설정한다.

```javascript
export default  {
   build: {
      outDir: 'dist3'
   }
}
```

Vite 버전이 1.x를 사용하면 아래와 같이 작성한다.

```javascript
export default  {
   outDir: 'dist3'
}
```

### build.rolupOptions

## References

[Vite 설정하기](https://vitejs.dev/config/)

# Vite 사용 

Vite 사용하여 vanilla js 프로젝트를 다루는 방법을 정리한다. 

## 프로젝트 생성 
프로젝트 루트에서 다음을 입력한다. 
```javascript
 yarn create vite
```

TypeScript를 사용하기 위해 프로젝트 타입으로 vanilla ts를 선택했다.  다음 명령들을 입력하여 필요한 것을 설치한다. 


```shell
cd vite
yarn
```

다음을 입력하여 로컬 서버를 띄운다. 
```
yarn dev
```
터미털에 다음이 출력된다. 
```
  vite v2.6.13 dev server running at:

  > Local: http://localhost:3000/
  > Network: use `--host` to expose

  ready in 275ms.
```

브라우저를 띄워서 들어가 보자. 

## 생성된 프로젝트 구조 
```shell
📁project_root
  📁src
     📄main.ts
     📄style.css
  📄index.html
  📄package.json
  📄tsconfig.json
```

### index.html
```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <link rel="icon" type="image/svg+xml" href="favicon.svg" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Vite App</title>
  </head>
  <body>
    <div id="app"></div>
    <script type="module" src="/src/main.ts"></script>
  </body>
</html>
```



### package.json
```json
{
  "name": "vite",
  "version": "0.0.0",
  "scripts": {
    "dev": "vite",
    "build": "tsc && vite build",
    "serve": "vite preview"
  },
  "devDependencies": {
    "typescript": "^4.3.2",
    "vite": "^2.6.4"
  }
}
```

### main.ts
```javascript
import './style.css'

const app = document.querySelector<HTMLDivElement>('#app')!

app.innerHTML = `
  <h1>Hello Vite!</h1>
  <a href="https://vitejs.dev/guide/features.html" target="_blank">Documentation</a>
`
```


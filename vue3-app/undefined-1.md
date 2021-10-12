# 플러그인에 객체 생성

서버와 데이터를 주고 받을 때 서버와 주고 받는 컨텐트 타입에 대한 MIME Type을 정의하는 객체를 플러그인에 생성한다.

## 플러그인에 객체 생성

HttpPlugin.js의 install() 함수에 MIMEType 객체를 다음과 같이 정의한다.

```javascript
const MIMEType = {
  text: ["text/plain"], // 특정 서브 타입이 없는 텍스트 문서들에 대해서는 text/plain이 사용되어야 한다. 
  xml:  ["text/xml"],
  html: ["text/html"],
  css:  ["text/css"],
  json: ["application/json"],
  javascript: ["text/javascript"],
  video: [
    "video/webm"
  ],
  audio: [
    "audio/mpeg"
  ],
  // 오직 널리 알려지고 웹에 안전하다고 취급되는 소수의 이미지 타입만이 사용될 수 있다. 
  image: ["image/gif", "image/png", "image/jpeg", "image/bmp" ],
  // application은 모든 종류의 이진 데이터를 나타낸다. 
  // application/octet-stream은 잘 알려지지 않는 이진파일을 의미. 
  // 브라우저는 보통 자동으로 실행하지 않거나 실행해야 할지 묻기도 한다. 
  // 대부분의 웹 서버들은 기본 타입 중 하나인 application/octet-stream MIME 타입을 사ㅛㅇㅇ해 
  // 알려지지 않은 타입의 리소스를 전송한다. 보안상의 이유로 대부분의 브라우저들은 그런 리소스에 대한
  // 사용자화된 기본 동작 설정을 허용하지 않으며 그것을 사용하려면 디스크에 저장할 것을 사용자에게 강제한다. 
  binary: [ "application/octet-stream"] // 서브타입이 없는 이진 문서에 대해서는 application/octet-stream을 사용 
}
```

## 플러그인의 객체 사용

마찬가지로 provide() 함수를 사용하여 객체를 제공한다.

```javascript
app.provide('hello', hello)
```

App.vue 파일에서 inject()를 사용하여 MIMEType 객체를 주입하고 그 값을 꺼내본다.

```javascript
const MIMEType = inject('MIMEType')

onMounted(() => { 
  console.log(MIMEType.image)
})
```

다음과 같이 콘솔에 값이 출력된다.

```shell
(4) ["image/gif", "image/png", "image/jpeg", "image/bmp"]
0: "image/gif"
1: "image/png"
2: "image/jpeg"
3: "image/bmp"
```

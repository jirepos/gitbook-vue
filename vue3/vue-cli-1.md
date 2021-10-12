# Vue CLI 구성하기

## 글로벌 CLI 구성

@vue/cli를 위한 몇몇 global 구성들은 당신의 홈디렉토리에 .vuerc 파일에 저장된다.

global CLI config를 확인하려면

```shell
vue config
```

다음과 같이 출력이 되었다.

```shell
PS F:\source-private\coding-room\java\repository\spring5-framework\vueapp> vue config
Resolved path: C:\Users\sangh\.vuerc
 {
  "useTaobaoRegistry": false
}
```

## vue.config.js

이 파일은 선택적인 구성 파일인데, #vue/cli-service에 의해 자동적으로 로드될 것이다. 이것이 프로젝트 루트에 있다면(package.json 바로 옆에), package.json에서 vue 필드를 또한 사용할 수 있다.

파일은 옵션들을 포함하는 object를 export해야 한다.

```javascript
// vue.config.js
module.exports = {
  // options...
}
```

### baseUrl

Vue CLI 3.3부터 사용되지 않으므로 publicPath대신 사용하십시오 .

### publicPath

* default: '/'

애플리케이션 번들이 배포 될 기본 URL. 이것은 webpack의 output.publicPath와 동일하지만 webpack의 output.publicPath를 수정하는 대신에 Vue CLI는 다른 목적으로 publicPath를 항상 사용해야 한다.

디폴트로, Vue CLI는 당신의 앱이 도메인의 루트에 디플로이될 거라고 가정한다. 예를들어, https://www.my-app.com과 같이. 당신의 앱이 sub-path에 디플로이 된다면, 이 옵션을 사용하여 sub-path를 명시할 필요가 있다. 예를들어, 당신의 앱이 https://www.foobar.com/my-app/에 디플로이 된다면, publicPath를 '/my-app/'로 설정한다.

값은 또한 empty string('') 꼬는 상대 패스(./)로 설정될 수 있다. 모든 자산들이 상대 패스로 연결되기 위해서. 이것은 생성된 번들이 어떤 public path 아래 디플로이 되는 것을 허용한다. 또는 Cordovar hybrid app와 같은 파일 시스템 기반의 환경에서 사용될 수 있다.

**Limitations of relative publicPath**\


상대적 publicPath는 제약들을 가지고 다음과 같은 상황을 피해야 한다.\


* HTML5 history.pushState routing을 사용하고 있다.
* multi-pated app을 생성하기 위해 pages 옵션을 사용하고 있다.

이 값을 개발 중일 때에도 존중된다. 당신이 당신의 dev server가 대신에 root에서 서비스되기를 원한다면, 당신은 조건 값을 사용할 수 있다.

```javascript
module.exports = {
  publicPath: process.env.NODE_ENV === 'production'
    ? '/production-sub-path/'
    : '/'
}
```

프로젝트의 context가 spring5-framework라고 가정하자. URL을 다음과 같이 작성해야 한다.

```
http://localhost:8080/spring5-framework/
```

Vue 소스들을 webroot/vueapps로 빌드할 것이다. 따라서 publicPath는 다음과 같이 설정해야 한다.

```javascript
// vue.config.js
module.exports = {
    publicPath: "/spring5-framework/vueapps/",
}
```

### outputDir

* Default: 'dist'

vue-cli-service-build를 실행할 때 프로덕션이 생성될 디렉토리. target directory는 building 전에 삭제될 것임으로 주의해야 한다. (이 동작은 --no-clean 을 전달함으로써 disable될 수 있다.)

> webpack ouput.path를 수정하는 대신에 ouputDir를 항상 사용해라.

프로젝트 폴더의 구조를 살펴보자.

```shell
protject-root/
    src/main/webapp/   <-- webroot
    vueapp/            <-- Vue Source
```

vueapp 폴더로 이동하고 Vue 프로그램을 빌드하면 src/main/webapp아래의 vueapps 아래로 프러덕션이 생성되어야 한다. 이렇게 하기 위해서는 다음과 같이 vue.config.js 파일을 작성한다.

```javascript
module.exports = {
    outputDir: "../src/main/webapp/vueapps/",
}
```

### assetsDir

* Default: ''

outputDir에 상대적으로, 생성된 static assets(js, css, img, fonts) 아래에 내포하기 위한 디렉토리이다.

> 생성된 assets으로 부터 filename 또는 chunkFilename이 덮어쓸 때 assetDir은 무시된다.

### indexPah

* Default : 'index.html'

outputDir에 상대적인, 생성된 index.html에 대한 output path를 명시한다. 또한 절대 패스가 될 수 있다.

### filenameHashing

* Default: true

디폴트로, 생성된 static assets는 더 나은 caching control을 위해 그들의 파일 이름에 hashes를 포함한다. 그러나, 이것은 Vue CLI에 의해 자동으로 생성될 index HTML을 요구한다. Vue CLI에 의해 생성된 index HTML을 사용할 수 없다면, 당신은 이 옵션을 false로 설정함으로써 filename hashing을 disable할 수 있다.

### pages

* Default : undefined

multi-page mode에서 app을 빌드한다. 각 페이지는 대응하는 JavaScriptp entry file을 가져야 한다. 그 값은 key가 entry의 이름인 object이어야 한다. 그리고 그 값은 다음과 같을 수 있다.

* entry, template, filename 그리고 chuns을 명시한 object
* 또는 그것의 entry를 명시하는 문자열

```javascript
module.exports = {
  pages: {
    index: {
      // entry for the page
      entry: 'src/index/main.js',
      // the source template
      template: 'public/index.html',
      // output as dist/index.html
      filename: 'index.html',
      // when using title option,
      // template title tag needs to be <title><%= htmlWebpackPlugin.options.title %></title>
      title: 'Index Page',
      // chunks to include on this page, by default includes
      // extracted common chunks and vendor chunks.
      chunks: ['chunk-vendors', 'chunk-common', 'index']
    },
    // when using the entry-only string format,
    // template is inferred to be `public/subpage.html`
    // and falls back to `public/index.html` if not found.
    // Output filename is inferred to be `subpage.html`.
    subpage: 'src/subpage/main.js'
  }
}
```

아래는 아직 정리 되지 않은 내용들입니다. **작업 중입니다.**

## 색인파일

* 파일 public/index.html은 html-webpack-plugin 으로 처리 될 템플릿입니다

## 보간

색인 파일이 템플리트로 사용되므로 lodash 템플리트 구문을 사용하여 값을 보간 할 수 있습니다 .

* <%= VALUE %> 이스케이프 처리되지 않은 보간;
* <%- VALUE %> HTML 이스케이프 된 보간;
* <% expression %> 자바 스크립트 제어 흐름

모든 클라이언트 측 환경 변수 를 직접 사용할 수도 있습니다. 예를 들어 BASE_URL값 을 사용하려면

```html
<link rel="icon" href="<%= BASE_URL %>favicon.ico">
```

## Preload

\<link rel="preload"> 는로드 후 바로 페이지에 필요한 리소스를 지정하는 데 사용되는 리소스 힌트 유형으로, 브라우저의 기본 렌더링 기계가 작동하기 전에 페이지로드 수명주기 초기에 미리로드를 시작하려고합니다.

## Prefetch

\<link rel="prefetch"> 페이지로드가 완료된 후 사용자가 브라우저의 유휴 시간에 사용자가 방문 할 수있는 콘텐츠를 미리 가져 오도록 브라우저에 지시하는 리소스 힌트 유형입니다.

## 다중 페이지 앱 빌드

모든 앱이 SPA 일 필요는 없습니다. Vue CLI는의 pages옵션을vue.config.js 사용하여 여러 페이지 앱을 빌드 할 수 있습니다 . 빌드 된 앱은 최적의 로딩 성능을 위해 여러 항목간에 공통 청크를 효율적으로 공유합니다.

## 정적 자산 처리

정적 자산은 두 가지 방법으로 처리 할 수 ​​있습니다.

* JavaScript로 가져 오거나 상대 경로를 통해 템플릿 / CSS에서 참조됩니다. 이러한 참조는 webpack에 의해 처리됩니다.
* public 디렉토리에 배치되고 절대 경로를 통해 참조됩니다. 이러한 자산은 단순히 복사되어 웹팩을 거치지 않습니다.

## References

See [Configuration Reference](https://cli.vuejs.org/config/).

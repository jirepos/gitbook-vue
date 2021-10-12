# Logging 처리

JavaScript에서 로깅을 할 때 보통 console.log()를 사용한다. 그런데 Java에서 처럼 debug,info,warn,error과 같이 레벨로 나누어 로그를 처리하고 싶다. 그런데 console.log()가 실행되면 실행되는 위치에서 소스파일명과 라인 수가 출력되기 때문에 다른 자바스크립트 파일에서 로그를 출력하면 안된다. 다음과 같이 네 개의 로거를 만들 것이다.

* debug
* info
* warn
* error

로거는 로거 레벨에 따라 아무 것도 하지 않는 더미 함수를 반환하거나 bind() 함수를 사용하여 console.log() 함수를 반환할 것이다.

```javascript
let debug = (level >=0)? console.log.bind(window.console): () => {};
```

아래는 로거에 대한 전체 아이디어를 보여준다.

```javascript


let logLevel =  {
  'DEBUG': 0,
  'INFO': 1, 
  'WARN': 2,
  'ERROR':3,
  'NONE': 4
};

// 설정파일에 로그 베벨을 설정
// none으로 설정하면 로그가 출력되지 않음 
let LOG_LEVEL = "debug"; // debug, info, warn, error, none 

// 설정파일에 설정된 로그 베벨의 정수 값으 구함 
let level = logLevel[LOG_LEVEL.toUpperCase()]

// 로거를 만듬
let debug = (level >=0)? console.log.bind(window.console): () => {};
let info  = (level >=1)? console.log.bind(window.console): () => {};
let error = (level >=3)? console.log.bind(window.console): () => {};

// 로그 출력  level에 따라 출력이 됨 
// 로그 함수는 소스에서 바로 실행해야 소스 파일명과 줄번호가 출력됨 
debug("debug log")
info("info log")
error("errr log")
```

로그 레벨 설정은 공통 속성을 담고 있는 environment.js에 정의한다.

```javascript
/** Environemnt */
const environment = {
  LOG_LEVEL : "error"  // debug , info,warn, error, none 
}///~
```

이 파일에 Logger 클래스를 작성한다.

```javascript
class Logger  {
  constructor(env) {
    this.env = env;
    this.logLevel =  {
      'DEBUG': 0,
      'INFO': 1, 
      'WARN': 2,
      'ERROR': 3,
      'NONE':  4
    };
  }
  
  getConfigLevel() {
    return this.logLevel[environment.LOG_LEVEL.toUpperCase()]
  }
  debug(msg) {
    let confLevel = this.getConfigLevel();
    return (confLevel >=0)? console.log.bind(window.console): () => {};
  }
  info() {
    let confLevel = this.getConfigLevel();
    return (confLevel >=1)? console.log.bind(window.console): () => {};
  }
  warn() {
    
    let confLevel = this.getConfigLevel();
    return (confLevel >=2)? console.log.bind(window.console): () => {};
  }
  error() {
    let confLevel = this.getConfigLevel();
    return (confLevel >=3)? console.log.bind(window.console): () => {};
  }
}
```

이 함수에 대한 참조 변수를 정의하고 export 한다.

```javascript

const logger = new Logger(environment)
const debug = logger.debug()
const info = logger.info()
const warn = logger.warn()
const error = logger.error()

export { environment as default,  debug, info, warn, error }
```

Vue 컴퍼넌트에서 사용할 수 있도록 Plugin으로 작성한다.

```javascript
import { debug, info, warn, error } from '@/api/environment.js'

export default {
  /**
   * 
   * @param {Object} app  createApp()에서 생성된 App 객체 
   * @param {Object} options 사용자가 전달한 옵션 
   */
  install: (app, options) => {
    app.provide('debug', debug)
    app.provide('info', info)
    app.provide('warn', warn)
    app.provide('error', error)
  }
}
```

UseCase 클래스나 Repository 클래스에서 사용할 때에는 import해야 한다.

```javascript
import { debug, info, warn, error } from '@/api/environment.js'
```

다음과 같이 사용한다.

```javascript
debug("debug")
info("info")
warn("warn")
error("error")
```

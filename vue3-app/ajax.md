# ajax() 함수 생성

화면에 세개의 레이어를 둔다.

* Repository Layer
* UseCase Layer
* Presentation Layer

**Presentation Layer** 화면에 해당한다. 예) LoginView.vue

**UseCase Layer** 화면단의 비즈니스 로직을 처리하는 레이어이다. 클래스로 작성하며 UseCase를 접미사로 붙인다. 예) LoginUseCase.ts

**Repository Layer**\
서버 API 상호작용하는 레이어이다. 클래스로 작성하며 Respository를 접무사도 붙인다. LoginRepository.ts

## ajax() 함수

서버에 실제로 AJAX요청을 처리하는 함수는 fetch() 함수를 래핑하여 만든다. 반환값은 Promise를 반환하도록 작성한다. 기본 뼈대는 다음과 같다.

```javascript
const ajax = (options) => {
  return new Promise( (resolve, reject) => {
    fetch(options.url, options)
      .then( response => {
        if(response.ok) {
          console.log("(1) first then")
          return response.text();
        }else {
          console.log("(1) first then error")
          throw new Error('error')
        }
      })
      .then( response => {
        console.log("(2) second then")
        resolve(response)
      })
      .catch(error => {
        console.log("(2)  catch error")
        reject(error)
      })
  })
}
```

ajax() 함수를 직접 사용하는 경우에는 다음과 같이 사용이 가능하다.

```javascript
ajax(options)
  .then(response => { 
     // 여기서 응답을 처리한다. 
  })
  .catch(error => {
     // 여기서 에러를 처리한다. 
  })
```

View에서 직접 이 함수를 호출하지 않는다. 다음과 같이 호출이 될 것이다.

```shell
LoginView --> LoginUseCase --> LoginRepository -> ajax()
```

그런데 이렇게 개발을 하는 경우에 ajax() 함수가 Promise를 반환하고 Promise는 비동기로 동작하기 때문에 제어가 어려워 진다. 이 문제를 해결하기 위해서는 async와 await를 사용한다. 간단한 의사코드를 살펴 보겠다.

Repository Layer에서 ajax() 함수를 호출하는 메서드를 다음과 같이 만든다. async를 사용하면 Promise가 반환된다.

```javascript
// repository layer logic , this returns Promise 
 async function selectOne() {
   let response = await ajax(options);
   return response 
 }
```

UseCase Layer에서도 마찬가지로 async를 사용한다. selectOne()이 프라미스를 리턴하는데 오류가 발생하는 경우에 처리할 방법이 필요하다. 이 경우 try\~catch 블록을 사용한다.

```javascript
// UseCase Layer 
 async function selectService() {
   try{
    let response = await selectOne();
    console.log("(3) response")
    return response;
   }catch(e) {
    console.log("(3)error")
    throw e;
   }
 }
```

Presentation Layer인 View 컴포넌트에서는 then()과 catch를 사용한다.

```javascript
// presentaion layer
selectService2()
 .then(response => {
   console.log("(4) response")
   console.log(response);
 })
 .catch(error => {
    console.log("(4)error")
 })
```

원하는 순서대로 동작하게 하려면 async와 await가 필수 적이다. 레이어 아키텍처를 사용하지 않으면 then(), catch()로만 충분하지만 로직을 레이어 별로 나누어 작성하기 때문에 실행되는 순서의 제어가 중요하다.

실제 ajax() 함수는 다음과 같다.

```javascript

/**
     * 모든 비동기 HTTP 요청을 처리하기 위한 함수이다. 내부적으로 는 Fetch API를 사용한다. 
     * @param {String} url  the request URL 
     * @param {Object} options the request options
     * @returns 
     */
 const ajax = (options) => {
  //  fetch() 함수에 전달될 기본 옵션이다. 
  let defaultOptions = {
    credentials:  'include',   //'same-origin',    // 자격증명이 포함된 요청을 하려면 이 줄을 추가해야. 이 옵션이 없으면 쿠키 값을 서버로 보내지 않음 
    method: 'POST',  // GET, POST, PUT, DELETE, etc 
    mode:   'cors', //'no-cors',    // no-cors, cors, *same-origin // cors로 값을 설정해야 Content-Type의 값을 설정할 수 있음
    chache: 'no-cache', // *default, no-cache, reload, force-cache, only-if-cached
    //credentials: 'same-origin', // include, *same-origin, omit
    headers: {
      'Accept': 'application/json', // 클라이언트가 이해 가능한 컨텐츠 타입이 무엇인지 
      'Content-Type': 'application/json',  // 서버에 어떤 형식의 데이터를 보내는지 알려줌
      // 'Content-Type': 'application/x-www-form-urlencoded',
    },
    // callback는 모두 필요 없을 것으로 판단 
    //successCallback: () => {},  // 성공했을 때 처리. 이 함수를 호출하는 코드에서 then()으로 처리하면 될 것 같음 
    //bizErrorCallback: () => {}, // http status는 성공이지만 business logic 오류가 있을 때 처리
    //errorCallback: () => {},    // 서버 오류가 발생했을 때 처리
    //redirect: 'follow', // manual, *follow, error
    //referrer: 'no-referrer', // no-referrer, *client
    //body: JSON.stringify(data), // body data type must match "Content-Type" header
  }

  let reqOptions = Object.assign(defaultOptions, options)
  if(reqOptions.body) {
    reqOptions.body = JSON.stringify(reqOptions.body)
  }
  console.log(reqOptions)

  return new Promise((resolve, reject) => {
    fetch(options.url, reqOptions)
      // 우선 json을 응답으로 받는다는 가정하에 
      // res.json()을 사용한다. 
      // 첫번재 then()에서 json() 함수를 호출하고 return해야 한다. 
      // 두번째 then()에서 응답 body의 데이터를 받을 수 있다. 
      .then(res => {
        //debugger; 
        console.log(">>>> RES")
        console.log(res);
        console.log(res.headers.get("Set-Cookie"));

        if(res.ok) {  // res.ok로 반드시 체크
          let contentType = res.headers.get("Content-Type")
          console.log(contentType) 
          if(contentType.indexOf("application/json") >= 0) {
            return res.json()
          }else if(contentType.indexOf("text/xml") >= 0) { 
            return res.text() 
          }else if(contentType.indexOf("text/plain") >= 0) { 
            return res.text() 
          }else if(contentType.indexOf("image/png") >= 0) { 
            return res.blob()
          }else if(contentType.indexOf("application/octet-stream") >= 0) { 
            return res.blob()
          }else {
            // application/xml은 여기서 처리함 
            return res.text()
          }
        }else {

          //debugger
          let MyError = function(code, message, headers) {
            this.code = code;
            this.message = message; 
            this.headers = headers;
          }
          throw new MyError(res.status, "error", res.headers)
        }
      })
      .then(res => {
        // 응답이 성공인 경우 
        resolve(res)
        // 응답은 성공이나 비즈니스 오류가 있는 경우 
        //if(bizErrorCallback) { bizErrorCallback() } 비즈니스 로직 오류가 발생한 경우에는 공통 처리만 필요
        //if(res.status == 200) { 
        //reject(res) 
        //}
      })
      .catch(error => {
        //debugger
        console.log(error)
        console.log(error.headers.get("Set-Cookie"));
        if(error.code == "500") {
          //console.log(error.headers.get("X-Message"));
          alert("system error")
        }else if(error.code  == "409")  {
          console.log(error.headers.get("X-Message-Code"));
          console.log(error.headers.get("X-Message"));
          alert(error.headers.get("X-Message-Code") + ":" + error.headers.get("X-Message"))
        }else if(error.code  == "404" ){
          alert("잘못된 요청")
        }else if(error.code  == "403") {
          alert("권한 없음")
        }else if(error.code  == "401") {
          alert("로그인 후에 이용하여 주세요")
        }
        // 응답이 오류인 경우 
        //if(errorCallback) { errorCallback() }
        reject(error)
      })
  })
}
```

> 위 함수에서 아직은 에러 처리를 공통으로 어떻게 처리할지는 작성되지 않았다. 나중에 정의할 생각이다.

### Response 객체

**status** Http Status의 정수값, 기본값 200

**ok** HttpStatus 값이 200에서 299 중 하나일 때. boolean 값. **body** Body contents

**headers** 헤더 객체

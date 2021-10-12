# Vuex

## Vuex

Vuex는 Applicatoin 전체에 걸쳐 공통의 값을 저장하거나 이벤트 발행 구독처리에 사용하기 위해서 검토하고 있다.

Vuex를 사용하면 이벤트를 한 곳에서 관리가 가능해서 유지관리가 좋을 것으로 생각된다

> Popup window와의 이벤트 처리는 window.postMessage()를 사용하는 방법을 검토한다.

### Version 4 설치

시험용 최신 버전 설치한다.

```shell
yarn add vuex@next --save
```

### 저장소 생성

store(저장소)는 애플리케이션 상태를 보유하고 있는 컨테이너이다.

* Vuex.store는 반응형
* 저장소의 상태를 직접 변경할 수 없다

createStore를 import해야 한다.

```javascript
import { createStore } from 'vuex'
```

createStore() 함수를 사용하여 store를 export 한다.

```javascript
import { createStore } from 'vuex'

export default createStore({
  // 상태 
})
```

store의 기본구조를 정의한다.

```javascript
export default createStore({
  state: {
  },
  getters: {
  },
  mutations: {
  },
  actions: { 
  }
```

### 상태

state는 Vuex 저장소의 루트 상태 객체이다. 여기에 상태를 추가한다.

#### 기본 타입 상태

기본타입인 counter를 추가한다.

```javascript
  state: {
    counter: 10
  },
```

Vue 컴포넌트에서 state에 추가한 counter 상태의 값을 읽으려면 useStore를 import해야 한다. useStore()를 사용하여 store 객체의 인스턴스를 가져온다.

```javascript
<script setup>
import { computed } from "vue";
import { useStore } from "vuex";

const store = useStore();

</script>
```

state의 counter 값을 읽으려면 store.state.counter를 사용한다.

상태 값을 template에 표시하려면 필드를 생성해야 하는데 상태값이 변경될 때마다 반응적으로 화면에 출력하려면 계산 필드로 만들어야 한다.

```javascript
const counter = computed(() => store.state.counter);  
```

template에는 mustache를 사용한다.

```html
{{ counter }} 
```

#### 객체 타입 상태

기본타입의 상태 뿐 아니라 객체 타입의 상태도 추가 가능하다.

```javascript
  // 상태 
  state: {
    loginUser: {  // state에 object 정의 가능
      userId: 'default@domain.com', 
      empCode: '100'
    }
  },
```

객체인 상태의 값은 다음과 같이 읽을 수 있다.

```javascript
const userId  = computed(() => store.state.loginUser.userId); // 상태값 가져오기
```

### 변이 Mutations

Vuex 저장소에서 실제로 상태를 변경하는 유일한 방법은 변이(mutation)하는 것이ㅣ다. mutations 객체에 핸들러 함수를 정의한다. 그러나 이 핸들러 함수를 직접 호출할 수 없다.

counter 상태에 값을 설정하는 핸들러 함수를 다음과 같이 작성한다.

```javascript
  mutations: {
    setCounter(state, value) {
      state.counter = value;
    },
  }
```

첫번째 파라미터는 state 객체이고 두번째는 전달할 값이다.

Vue 컴포넌트에서 이 핸들러 함수를 사용하려면 commit()를 이용한다. 첫번째 파라미터는 핸들러 함수의 타입이고 두번째는 전달할 값이다.

```javascript
const inc = () => store.commit("setCounter", counter.value + 1); // 상태값 변경
```

객체인 경우에는 다음과 같이 핸들러 함수를 작성할 수 있다.

```javascript
  mutations: {
    setUserId(state, value) {
      state.loginUser.userId = value;
      console.log("사용자 정보를 변경했음")
    }
  },
```

### 액션 Action

상태를 바로 변경하지 않고 어떤 처리를 한 다음에 상태를 변경해야하는 경우가 있을 수 있는데, 어떤 처리는 비동기적일 수도 있다. 이런 경우에 Action을 사용한다.

```javascript
  actions: { 
    increment(context, payload) {
      let value = payload.amount
      console.log(value)
      context.commit('setCounter', value)
    }
```

action은 dispatch()를 사용하여 호출한다.

```javascript
const setCounter = () => {
    store.dispatch('increment', { amount: 100 })
}
```

### Getters

상태값을 있는 그대로 가져오는 것이 아니라 가공을 해서 가져와야할 필요가 있다. 그런데 각각의 컴포넌트들에서 상태를 가져와서 가공하는 로직을 작성하면 로직이 분산되어 파악하기가 어렵다. 이럴 때 getters를 사용한다.

첫번째 전달 인자로 상태를 받는다.

```javascript
  getters: {
    multiCount: state => {
      return state.counter * 10 
    }
  },
```

store.getters를 이용하여 getter를 호출할 수 있다.

```javascript
const multiCount = computed(() => store.getters.multiCount); // 계산된 상태값 가져오기
```

### 구독하기

상태 값이 변할 때마다 어떤 처리를 해야 한다고 가정하자. 이경우 상태값이 변경되는지 지켜보고 있어야 하는데, subscribe를 이용하면 mutation을 이용하여 상태값이 변경된 이후에 이벤트를 통지 받는다.

```javascript
store.subscribe( (mutation, state) => {
    console.log("mutation type:" + mutation.type)
    if(mutation.type == "setUserId") {
        console.log("사용자 정보 변경된 이후에 구독에 통지")
        console.log(mutation.payload)  // mutation에 전달된 값 
    }
})
```

type으로 어떤 변이가 호출되었는지 알 수 있고 payload로 전달된 값을 알 수 있다.

### map

작성할 예정이다.

### 모듈

store를 Vuex 모듈로 나눠 관리할 수 있다.

> 개별 Vuex 모듈은 로컬 상태, 뮤테이션, 액션, 게터를 가질 수 있다.

blog-store.js 파일을 생성한다.

```javascript

export default { 
  namespaced:true, 
  state: {
    selectedPostId: ''
  },
  getters: {
    prefixPostId: state => {
      return "AA" + state.selectedPostId
    }
  },
  actions: {
  },
  mutations: {
    setSelectedPostId(state, selectedPostId) {
      state.selectedPostId = selectedPostId
    }
  }
}
```

store-blog.js를 임포트하고 modules 속성에 설정한다.

```javascript
///store.js
import { createStore } from 'vuex'
import blog from './blog-store.js'

export default createStore({
  state: {
    counter: 10
  },
  getters: {
  },
  mutations: {
    setCounter(state, value) {
      state.counter = value;
    },
  },
  actions: { 
  },
  modules: {
    blog: blog
  }
})
```

#### 모듈의 state에 대한 접근

'store.state.모듈'과 같이 접근한다.

```javascript
const postId = computed (() => store.state.blog.selectedPostId)
console.log(postId.value)
```

#### 상태 변경

기본적으로 모듈 내의 액션, 변이 및 getter는 여전히 전역 네임 스페이스 아래에 등록된다. 여러 모듈이 동일한 변이/액션 유형에 반응 할 수 있다. 만약 모듈이 독립적이거나 재사용되기를 원한다면, namespaced: true라고 네임스페이스에 명시하면 된다. 모듈이 등록될 때, 해당 모듈의 모든 getter, 액션/변이는 자동으로 등록된 모듈의 경로를 기반으로 네임스가 지정된다.

```javascript
const setSelectedPostId = (selVal) => {
  store.commit('blog/setSelectedPostId', selVal)
}
setSelectedPostId('2222')
const prefixPostId = computed(() => store.getters['blog/prefixPostId'])
console.log(prefixPostId.value)
```

아래는 예시이다.

```javascript
const store = new Vuex.Store({
  modules: {
    account: {
      namespaced: true,

      // 모듈 자산
      state: () => ({ ... }), // 모듈 상태는 이미 중첩되어 있고, 네임스페이스 옵션의 영향을 받지 않음
      getters: {
        isAdmin () { ... } // -> getters['account/isAdmin']
      },
      actions: {
        login () { ... } // -> dispatch('account/login')
      },
      mutations: {
        login () { ... } // -> commit('account/login')
      },

      // 중첩 모듈
      modules: {
        // 부모 모듈로부터 네임스페이스를 상속받음
        myPage: {
          state: () => ({ ... }),
          getters: {
            profile () { ... } // -> getters['account/profile']
          }
        },

        // 네임스페이스를 더 중첩
        posts: {
          namespaced: true,

          state: () => ({ ... }),
          getters: {
            popular () { ... } // -> getters['account/posts/popular']
          }
        }
      }
    }
  }
})
```

네임스페이스의 getter와 액션은 지역화된 getters, dispatch 그리고 commit을 받는다. 즉, 동일한 모듈 안에서 접두어 없이 모듈 자산을 사용할 수 있다. 네임스페이스 옵션 값을 바꾸어도 모듈 내부의 코드에는 영향을 미치지 않는다.

#### 네임스페이스 모듈 내부에서 전역 자산 접근

전역 상태나 getter를 사용하고자 한다면, rootState와 rootGetters가 getter 함수의 3번째와 4번째 인자로 전달되고, 또한 action 함수에 전달된 'context' 객체의 속성으로도 노출된다.

전역 네임스페이스의 액션을 디스패치하거나 변이를 커밋하려면 dispatch와 commit에 3번째 인자로 { root: true }를 전달하면 된다.

```javascript
modules: {
  foo: {
    namespaced: true,

    getters: {
      // `getters`는 해당 모듈의 지역화된 getters
      // getters의 4번째 인자를 통해서 rootGetters 사용 가능
      someGetter (state, getters, rootState, rootGetters) {
        getters.someOtherGetter // -> 'foo/someOtherGetter'
        rootGetters.someOtherGetter // -> 'someOtherGetter'
      },
      someOtherGetter: state => { ... }
    },

    actions: {
      // 디스패치와 커밋도 해당 모듈의 지역화된 것
      // 전역 디스패치/커밋을 위한 `root` 옵션 설정 가능
      someAction ({ dispatch, commit, getters, rootGetters }) {
        getters.someGetter // -> 'foo/someGetter'
        rootGetters.someGetter // -> 'someGetter'

        dispatch('someOtherAction') // -> 'foo/someOtherAction'
        dispatch('someOtherAction', null, { root: true }) // -> 'someOtherAction'

        commit('someMutation') // -> 'foo/someMutation'
        commit('someMutation', null, { root: true }) // -> 'someMutation'
      },
      someOtherAction (ctx, payload) { ... }
    }
  }
}
```

## 소스

### StoreBaSic.js

**StoreBasic.js**

```javascript
import { createStore } from 'vuex'

export default createStore({
  // 상태 
  state: {
    counter: 10,  // state에 primitive 정의 가능
    loginUser: {  // state에 object 정의 가능
      userId: 'default@domain.com', 
      empCode: '100'
    }
  },
  // 상태를 가공할 필요가 있는 경우에 getters에서 사용 
  getters: {
    multiCount: state => {
      return state.counter * 10 
    }
  },
  // 상태 값을 수정하려면 mutations 사용 
  // 변이는 무조건 동기적이다. 
  mutations: {
    setCounter(state, value) {
      state.counter = value;
    },
    setUserId(state, value) {
      state.loginUser.userId = value;
      console.log("사용자 정보를 변경했음")
    }
  },
  // 상태를 변이 시키는 대신 액션으로 변이에 대한 커밋을 한다. 
  // 작업에는 임의의 비동기 작업이 포함될 수 있다. 
  actions: { 
    increment(context, payload) {
      let value = payload.amount
      console.log(value)
      context.commit('setCounter', value)
    }
    // increment(context) {
    //   context.commit('increment')
    //   context.getters 나 context.state에 접근할 수 있다. 
    // },
    // 값을 전달할 수 있음. 파라미터는 context 객체를 분해해서 전달하고 있음
    // checkout( { commmit, state }, products) {
    // 
    // }
    // 호출은 dispatch를 사용 
    // store.dispatch('increment')
  }
})
```

### main.js

```javascript
import { createApp } from 'vue'
import App from './App.vue'
import { createStore } from 'vuex'
import store from './store/StoreBasic.js'

createApp(App)
  .use(store)
  .mount('#app')
```

### VuexComp.vue

**VuexComp.vue**

```html
<template>
    <strong>Vuex Basic</strong> br 
    {{ counter }} <br>
    {{ userId }} <br>
    MultiCount: {{ multiCount }}
    
    <button @click="inc">inc</button>
    <button @click="setUserId">Set UserID</button> <br>
    <button @click="setCounter">Set Counter</button>
</template>
<script setup>
import { computed } from "vue";
import { useStore } from "vuex";
 
const store = useStore();
const counter = computed(() => store.state.counter); // 상태값 가져오기
const userId  = computed(() => store.state.loginUser.userId); // 상태값 가져오기 
const inc = () => store.commit("setCounter", counter.value + 1); // 상태값 변경
const setUserId = () => store.commit("setUserId",  "aaa@gmail.com"); // 상태값 변경
const multiCount = computed(() => store.getters.multiCount); // 계산된 상태값 가져오기 
const setCounter = () => {
    store.dispatch('increment', { amount: 100 })
}


// https://vuex.vuejs.org/kr/api/#replacestate
// 저장소의 변이를 구독한다. handler는 모든 변이 이후 호출되고 변이 디스크립터와
// 변이 상태를 인자로 받는다
store.subscribe( (mutation, state) => {
    console.log("mutation type:" + mutation.type)
    if(mutation.type == "setUserId") {
        console.log("사용자 정보 변경된 이후에 구독에 통지")
        console.log(mutation.payload)  // mutation에 전달된 값 
    }
})
</script>
```

## References

[Vuex 3 공식사이트](https://vuex.vuejs.org/kr/)\
[Vuex 4 공식사이트](https://next.vuex.vuejs.org)\
[Vuex API 레퍼런스](https://vuex.vuejs.org/kr/api/#replacestate)\
[Vue 3 Vuex 사용법](https://kyounghwan01.github.io/blog/Vue/vue3/composition-api-vuex/#store-%E1%84%8B%E1%85%A7%E1%84%85%E1%85%A5-module%E1%84%85%E1%85%A9-%E1%84%87%E1%85%AE%E1%86%AB%E1%84%85%E1%85%B5)

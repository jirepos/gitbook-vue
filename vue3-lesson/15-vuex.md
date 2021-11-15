# Vuex 

Vuex는 Applicatoin 전체에 걸쳐 공통의 값을 저장하거나 이벤트 발행 구독처리에 사용하기 위해서 검토하고 있다.

Vuex를 사용하면 이벤트를 한 곳에서 관리가 가능해서 유지관리가 좋을 것으로 생각된다


> Popup window와의 이벤트 처리는 window.postMessage()를 사용하는 방법을 검토한다.



## Version 4 설치 

```shell
yarn add vuex@next --save
```


## 저장소 생성 

store(저장소)는 애플리케이션 상태를 보유하고 있는 컨테이너이다.

* Vuex.store는 반응형
* 저장소의 상태를 직접 변경할 수 없다


store 디렉터리를 생성한다.  그 아래 store.js를 생성한다. 

```shell
📁project_root
  📁vue
     📁src
       📁store
          📄store.js 
```

createStore.js를 import 한다. 
```jsx
import { createStore } from 'vuex'
```

createStore() 함수를 사용하여 store를 export 한다.

```jsx
import { createStore } from 'vuex'

export default createStore({
  // 상태 
})
```
store의 기본구조를 정의한다.


```jsx

import { createStore } from 'vuex'

export default createStore({
  state: {
  },
  getters: {
  },
  mutations: {
  },
  actions: { 
  }
})   
```

## store 등록
main.js에 store를 사용할 수 있도록 use()를 사용하여 등록한다. 
```jsx
import { createApp } from 'vue'
import App from './App.vue'
import store from './store/store.js'

createApp(App)
  .use(store)
  .mount('#app');
```


## 상태 

state는 Vuex 저장소의 루트 상태 객체이다. 여기에 상태를 추가한다.

### 기본 타입 상태
기본타입인 counter를 추가한다.
```jsx
  state: {
    counter: 10
  },
```
이제 store를 사용해 보자. c15 디렉터리를 만든다.  그 아래 StoreComp.vue 파일을 생성한다. 
```shell
📁project_root
  📁vue
     📁src
       📁components
          📁c15
             📄StoreComp.vue
```

Vue 컴포넌트에서 state에 추가한 counter 상태의 값을 읽으려면 useStore를 import해야 한다. useStore()를 사용하여 store 객체의 인스턴스를 가져온다


**StoreComp.vue** 
```html
<script>
import { defineComponent } from 'vue'
import { useStore } from "vuex";

export default defineComponent({
  setup() {
    const store = useStore();
  },
})
</script>
```

state의 counter 값을 읽으려면 store.state.counter를 사용한다.

상태 값을 template에 표시하려면 필드를 생성해야 하는데 상태값이 변경될 때마다 반응적으로 화면에 출력하려면 계산 필드로 만들어야 한다.


```jsx
const counter = computed(() => store.state.counter);  
```
template에는 mustache를 사용한다.
```html
{{counter}}
```

**StoreComp.vue**
```html
<template>
  <div>
    <h1>StoreComp.vue</h1>
    <div>
      {{counter}}
    </div>
  </div>
</template>
<script>
import { defineComponent, computed } from 'vue'
import { useStore } from "vuex";

export default defineComponent({
  setup() {
    const store = useStore();
    const counter = computed(() => store.state.counter);  
    return { 
      counter 
    }
  },
})
</script>
```
### 객체 타입 상태

기본타입의 상태 뿐 아니라 객체 타입의 상태도 추가 가능하다.

```jsx
  // 상태 
  state: {
    loginUser: {  // state에 object 정의 가능
      userId: 'default@domain.com', 
      empCode: '100'
    }
  },
```

객체인 상태의 값은 다음과 같이 읽을 수 있다.
```jsx
const userId  = computed(() => store.state.loginUser.userId); // 상태값 가져오기
```



## 변이(Mutations)
Vuex 저장소에서 실제로 상태를 변경하는 유일한 방법은 변이(mutation)하는 것이다. mutations 객체에 핸들러 함수를 정의한다. 그러나 이 핸들러 함수를 직접 호출할 수 없다.


counter 상태에 값을 설정하는 핸들러 함수를 다음과 같이 작성한다.

**store.js** 
```jsx
  mutations: {
    setCounter(state, value) {
      state.counter = value;
    },
  }
```

> 다시 말하면, state에 있는 상태 값을 변경하려면 mutations에 핸들러 함수를 정의해야 한다. 


첫번째 파라미터는 state 객체이고 두번째는 전달할 값이다.

Vue 컴포넌트에서 이 핸들러 함수를 사용하려면 commit()를 이용한다. 첫번째 파라미터는 핸들러 함수의 타입이고 두번째는 전달할 값이다



template에 button 요소를 생성하고 click 핸들러 함수를 만든다. 
```html
<input type="button" @click="onInc" value="Increase Counter">
```
```jsx
const counter = computed(() => store.state.counter);  
const onInc = () => store.commit("setCounter", counter.value + 1); // 상태값 변경
```

객체인 경우에는 다음과 같이 핸들러 함수를 작성할 수 있다.

**store.js**
```jsx
  mutations: {
    setUserId(state, value) {
      state.loginUser.userId = value;
      console.log("사용자 정보를 변경했음")
    }
  },
```


## 액션(Action)
상태를 바로 변경하지 않고 어떤 처리를 한 다음에 상태를 변경해야하는 경우가 있을 수 있는데, 어떤 처리는 비동기적일 수도 있다. 이런 경우에 Action을 사용한다.

**store.js**
```jsx
  actions: { 
    increment(context, payload) {
      let value = payload.amount
      console.log(value)
      context.commit('setCounter', value)
    }
```    

action은 dispatch()를 사용하여 호출한다.
```jsx
const setCounter = () => {
    store.dispatch('increment', { amount: 100 })
}
```


## Getters

상태값을 있는 그대로 가져오는 것이 아니라 가공을 해서 가져와야할 필요가 있다. 

그런데 각각의 컴포넌트들에서 상태를 가져와서 가공하는 로직을 작성하면 로직이 분산되어 파악하기가 어렵다. 이럴 때 getters를 사용한다.

첫번째 전달 인자로 상태를 받는다.

```jsx
  getters: {
    multiCount: state => {
      return state.counter * 10 
    }
  },
```

store.getters를 이용하여 getter를 호출할 수 있다.
```jsx
const multiCount = computed(() => store.getters.multiCount); // 계산된 상태값 가져오기
```


## 구독하기(Subscribe)

상태 값이 변할 때마다 어떤 처리를 해야 한다고 가정하자. 이경우 상태값이 변경되는지 지켜보고 있어야 하는데, subscribe를 이용하면 mutation을 이용하여 상태값이 변경된 이후에 이벤트를 통지 받는다.

**StoreComp.vue**
```jsx
store.subscribe( (mutation, state) => {
    console.log("mutation type:" + mutation.type)
    if(mutation.type == "setUserId") {
        console.log("사용자 정보 변경된 이후에 구독에 통지")
        console.log(mutation.payload)  // mutation에 전달된 값 
    }
})
```

type으로 어떤 변이가 호출되었는지 알 수 있고 payload로 전달된 값을 알 수 있다.




## 모듈 

store를 Vuex 모듈로 나눠 관리할 수 있다.

> 개별 Vuex 모듈은 로컬 상태, 뮤테이션, 액션, 게터를 가질 수 있다. 

### 모듈 파일 생성 
store 디렉터리에 blog-store.js 파일을 생성한다. 구조는 store.js와 동일하다. 다만, namespaced: true를 설정한다. 

**blog-store.js**
```jsx
export default { 
  namespaced:true, 
  state: {
  },
  getters: {
  },
  actions: {
  },
  mutations: {
  }
}
```
store.js에서는 이 blog-store.js 파일을 import하고 modules 속성에 설정한다. 
**store.js**
```jsx
///store.js
import { createStore } from 'vuex'
import blog from './blog-store.js'

export default createStore({
  state: {
  },
  getters: {
  },
  mutations: {
  },
  actions: { 
  },
  modules: {
    blog: blog
  }
})
```


### 모듈의 state에 대한 접근
blog-store.js 파일에  selectedPostId state를 정의한다. 
**blog-store.js**
```jsx
export default { 
  namespaced:true, 
  state: {
    selectedPostId: '222'
  },
  getters: {
  },
  actions: {
  },
  mutations: {
  }
}
```
'store.state.[모듈 이름]'과 같이 접근한다. 
```jsx
const postId = computed (() => store.state.blog.selectedPostId)
console.log(postId.value)
```


### 상태 변경

기본적으로 모듈 내의 액션, 변이 및 getter는 여전히 전역 네임 스페이스 아래에 등록된다. 여러 모듈이 동일한 변이/액션 유형에 반응 할 수 있다. 

만약 모듈이 독립적이거나 재사용되기를 원한다면, **namespaced: true**라고 네임스페이스에 명시하면 된다. 모듈이 등록될 때, 해당 모듈의 모든 getter, 액션/변이는 자동으로 등록된 모듈의 경로를 기반으로 네임스가 지정된다.

다음과 같이 상태 값을 변경할 수 있다. 
```jsx
store.commit('blog/setSelectedPostId', selVal)
```
버튼을 하나 생성한 다음에 상태 값을 변경 해 보자. 

먼저 mutation을 정의한다.
**blog-store.js**
```jsx
  mutations: {
    setSelectedPostId(state, value) {
      state.selectedPostId = value;
    },
  }
```
**StoreComp.vue**
버튼의 이벤트 핸들러를 작성하여 store의 상태를 변경하도록 작성한다.
```html
<template>
  <input type="button" @click="onSetPostId" value="Set selectedPostId"> 
</template>
<script>
    //... 생략 
    const onSetPostId = () => {
      store.commit('blog/setSelectedPostId', "ABCDEF")
    }
</script>
```
### Getters
모듈명을 접두사로 붙여 맵 형식으로 조회한다. 

```jsx
const prefixPostId = computed(() => store.getters['blog/prefixPostId'])
console.log(prefixPostId.value)
```


### 액션(Action)
모듈명을 앞에 붙여 호출한다. 
```jsx
 store.dispatch('account/login', value)
```


### 구독하기
상태명 앞에 모듈명을 붙인다. 

```jsx
    store.subscribe( (mutation, state) => {
        console.log("mutation type:" + mutation.type)
        if(mutation.type == "blog/selectedPostId") {
            console.log(mutation.payload)  // mutation에 전달된 값 
        }
    })

```


## 네임스페이스 모듈 내부에서 전역 자산 접근 
### Getters
전역 상태나 getter를 사용하고자 한다면, rootState와 rootGetters가 getter 함수의 3번째와 4번째 인자로 전달되고, 또한 action 함수에 전달된 'context' 객체의 속성으로도 노출된다.

즉, blog-store.js의 blog 모듈에서는 getter를 만들 때 다음과 같이 함수 시그니처를 작성한다. 

```jsx
  getters: {
    getPostId2(state, getters, rootState, rootGetters) {
    }
  },
```

**state**
blog 모듈의 state

**getters**
blog 모듈의 getters

**rootState**
전역의 state

**rootGetters**
전역의 getters


사용 방법을 살펴 보자. 

**store.js**
전역의 getter를 선언한다. 
```jsx
  getters: {
    multiCount: state => {
      return state.counter * 10 
    }
  },
```

**blog-store.js**
아래 코드를 보면 blog 모듈의 getter는 getters.postId를 사용하여 접근하고 store.js의 getter는 rootGetters.multiCount를 통해 접근하는 것을 볼 수 있다. 

```jsx
  },
  getters: {
    getPostId : state => {
       return state.selectedPostId + "///"
    },
    getPostId2(state, getters, rootState, rootGetters) {
      let retVal = getters.getPostId;
      retVal = retVal + "<>" + rootGetters.multiCount;
      return retVal;
    }
  },
```





### Actions
전역으로 dispatch를 사용하려면 다음과 같이 함수 시그니처를 작성한다. 
```jsx
   someAction ({ dispatch, commit, getters, rootGetters}) {

   }
```

전역 dispatch/commit을 하려면 'root' 옵션을 사용한다. 다음과 같이 dispatch와 commit를 사용할 수 있다. 

```jsx
actions: {
  someAction ({ dispatch, commit, getters, rootGetters }) {
    getters.someGetter // -> 'blog/someGetter'
    rootGetters.someGetter // -> 'someGetter'
    dispatch('someOtherAction') // -> 'blog/someOtherAction'
    dispatch('someOtherAction', null, { root: true }) // -> 'someOtherAction'
    commit('someMutation') // -> 'blog/someMutation'
    commit('someMutation', null, { root: true }) // -> 'someMutation'
  },
}      
```

































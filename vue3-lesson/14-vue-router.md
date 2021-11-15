# Vue Router

Vue 라우터는 Vue.js (opens new window)의 공식 라우터입니다. Vue.js를 사용한 싱글 페이지 앱을 쉽게 만들 수 있도록 Vue.js의 코어와 긴밀히 통합되어 있습니다.

아래의 기능을 포함합니다.

* 중첩된 라우트/뷰 매핑
* 모듈화된, 컴포넌트 기반의 라우터 설정
* 라우터 파라미터, 쿼리, 와일드카드
* Vue.js의 트랜지션 시스템을 이용한 트랜지션 효과
* 세밀한 네비게이션 컨트롤
* active CSS 클래스를 자동으로 추가해주는 링크
* HTML5 히스토리 모드 또는 해시 모드(IE9에서 자동으로 폴백)
* 사용자 정의 가능한 스크롤 동작




## 설치
npm i vue-router는 이전 버전이고 Vue3에서 사용하려면 @next를 붙여야 한다.
```shell
npm i vue-router@next
```

## router.js 생성

다음 경로에 router.js 파일을 생성한다. 

```shell
📁project_root
  📁vue
     📁src
       📁router
          📄router.js 
```

createRouter와 createWebHistory를 import한다. 

```shell
import { createWebHistory, createRouter } from 'vue-router';
```

route 경로를 설정할 routes 상수를 하나 배열로 선언한다. 

```jsx
const routes = [

]
```
createRouter() 를 사용하여 router를 생성한다. 
```jsx
export const router = createRouter({
  history: createWebHistory(),
  routes,
});
```

## router 등록
main.js에서 router.js를 import하고 usee()를 사용하여 router를 등록한다. 
```jsx
import { router } from './router/router.js' 

createApp(App)
 .use(store)
 .use(router)
 .mount('#app')
```

## Home.vue 생성
src/components/c15 디렉터리 아래에 Home.vue를 생성하여 다음과 같이 간단히 작성한다. 
```html
<template>
  <div>
    <H1>Home</H1>
  </div>
  
</template>
<script>
export default {
  
}
</script>
```

## App.vue 수정
URL Pattern에 따라 화면이 표시될 자리를 만들어야 한다. App.vue 파일을 열고 \<router-view\> 태그를 추가한다. 
```html
<script setup>
import { ref } from 'vue'
</script>

<template>
  <div>
    <router-view></router-view>
  </div>
</template>

<style>
</style>
```

## 경로 매핑
다시 router.js 파일을 열고 root 경로 일 때 Home 컴포넌트가 표시될 수 있도록 경로를 매핑한다. 
```jsx
const routes = [
  {
    path: '/',
    name: 'Home',
    component: () => import('@/conoibebts/c14/Home.vue'), // 동적 import
  },
]
```


## /login 라우팅 경로 추가 
### Login.vue 생성
c14 디렉토리에 Login.vue 컴포넌트를 다음과 같이 생성한다. 
```html
<template>
  <div>
    <h1>Login</h1>
  </div>
  
</template>
<script>
export default {
  
}
</script>
```

### router.js 수정

```html
<template>
  <div>
    <h1>Login</h1>
  </div>
  
</template>
<script>
export default {
  
}
</script>
```


브라우저에 다음의 주소를 입력한다.  Login 화면이 표시될 것이다. 
```shell
http://localhost:3000/login
```


## link 사용하기 
다음과 같이 A 태그를 사용할 수 있다. 
```html
<template>
  <a href="/">Home</a><br>
  <a href="/login">Login</a><br>
  <router-view />
</template>
<script setup>
</script>
```
## 정적 라우팅 링크 사용 
router-link를 사용하여 링크를 만들 수도 잇다. 

```html
<template>
  <div>
    <router-link to="/">Home</router-link> <br>
    <router-link to="/login">Login</router-link>
    <router-view></router-view>
  </div>
</template>
```


## 동적 경로 매핑 
파라미터를 사용하여 동적으로 경로를 매핑할 수 있다. 파라미터는 콜론으로 표시한다.

URL에 사용자 아이디를 매핑해 보자. 
```jsx
const routes = [
  {
    path: '/login/:id',
    name: 'Login',
    component: () => import('@/views/Login.vue'),
  },
]
```

App.vue를 열고 다음곽 같이 아이디를 추가한다. 
```html
<template>
  <div>
    <router-link to="/">Home</router-link> <br>
    <router-link to="/login/hong">Login</router-link>
    <router-view></router-view>
  </div>
</template>
```
브라우저에서 링크를 클릭하면 잘 동작할 것이다. 


### 파라미터 사용 

Component에서 파라미터는 route.params으로부터 가져올 수 있다. useRoute를 import하고 useRoute()를 사용하여 route를 구한다.


```html
<script>
import { useRoute } from 'vue-router'
export default {
  setup() {
    const route = useRoute()
    console.log(route.params)
    console.log(route.params.id)
  }
}
</script>
```


같은 라우트 경로에 여러개의 params를 가질 수 있다.

| pattern | matched path | route.params |
|---|---|---|
|/users/:username|/users/eduardo| { username: 'eduardo' }|
| /users/:username/posts/:postId| /users/eduardo/posts/123| { username: 'eduardo', postId: '123' }|

### 파라미터 변경에 반응하기 
watch()를 사용하여 파라미터가 변경되는 것을 확인할 수 있다. 

```html
<script>
import { watch  } from 'vue'
import { useRoute } from 'vue-router'
export default {
  setup() {
    const route = useRoute()
    console.log(route.params)
    console.log(route.params.id)

    watch( () => route.params, (newVal, oldVal) => {
     console.log("old=>" + oldVal.id)
     console.log("new=>" + newVal.id)
    })
  }
}
</script>
```


## 중첩 라우팅 


컴포넌트가 하위 컴포넌트를 포함할 할 때의 라우팅을 하는 방법을 알아 보자. User 컴포넌트를 만들고 User 컴포넌트의 하위 컴포넌트인 Profile 컴포넌트와 UserPosts 컴포넌트를 만들다.

**Profile.vue**
```html
<template>
  <div>
    <h1>Profile</h1>
  </div>
  
</template>
<script>
import { defineComponent } from 'vue'

export default defineComponent({
  setup() {
    
  },
})
</script>
```

**User.vue**  
```html
<template>
  <div>
    <h1>User</h1>
  </div>
  
</template>
<script>
import { defineComponent } from 'vue'

export default defineComponent({
  setup() {
    
  },
})
</script>
```

**Posts.vue**
```html
<template>
  <div>
    <h1>Posts</h1>
  </div>
  
</template>
<script>
import { defineComponent } from 'vue'

export default defineComponent({
  setup() {
    
  },
})
</script>
```



User 컴포넌트에 router-view를 추가한다

```html
<template>
  <div>
    <h1>User</h1>
    <router-view></router-view>
  </div>
  
</template>
```


### routing 경로 추가 
router.js에 User 컴포넌트에 대한 라우팅 경로를 추가한다. 하위 컴포넌트들에 대한 경로 매핑을 위해 children을 사용하여 다음과 같이 작성한다. 
```jsx
import { createWebHistory, createRouter } from 'vue-router';

const routes = [
  {
    path: '/',
    name: 'Home',
    component: () => import('@/components/c15/Home.vue'), // 동적 import
  },
  {
    path: '/login/:id',
    name: 'Login',
    component: () => import('@/components/c15/Login.vue'), // 동적 import
  },
  {
    path: '/user/:id',
    name: 'User',
    component: () => import('@/views/User.vue'),
    children: [
      {
        path: 'profile',
        component: () => import('@/components/c15/Profile.vue')
      },
      {
        path: 'posts',
        component: () => import('@/components/c15/Posts.vue')
      }
    ]
  },
]

export const router = createRouter({
  history: createWebHistory(),
  routes,
});
```

이제 /user/jenny/profile 경로를 호출하면 Profile 컴포넌트가 표시될 것이고 /user/jenny/posts 경로를 호출하면 UserPosts 컴포넌트가 표시될 것이다.


추가할 것이 하나 더 있는데, 만일 /user/jenny 경로를 호출하면 하위 컴포넌트들이 아무것도 표시되지 않을 것이다. 이경우에는 빈 경로 표시를 다음과 같이 추가한다.
```jsx
    children: [
      {
        path: '',
        component: () => import('@/components/c15/UserHome.vue')
      },
    ]  
```


**UserHome.vue**
```html
<template>
  <div>
    <h1>User's Home</h1>
  </div>
  
</template>
<script>
import { defineComponent } from 'vue'

export default defineComponent({
  setup() {
    
  },
})
</script>
```


## Named Routes
때로는 이름을 가진 라우트(route)가 편리할 때가 있다.   Router 인스턴스에 경로 매핑을 하면서 이름을 줄 수 있다. 

* name 프러퍼티를 사용하여 이름을 설정한다.

```jsx
const routes = [
  {
    path: '/',
    name: 'Home',
    component: () => import('@/components/c15/Home.vue'), // 동적 import
  },
  {
    path: '/login/:id',
    name: 'Login',
    component: () => import('@/components/c15/Login.vue'), // 동적 import
  },
]  
```
named route에 링크하기 위해서 App.vue의 링크를 다음과 같이 수정해보자. 
**App.vue** 
```html
<template>
  <div>
    <router-link to="/">Home</router-link> <br>
    <router-link to="{name: 'Login', params: { id: 'Jenny'}">Login</router-link>
    <router-view></router-view>
  </div>
</template>
```

> 이제 탐색은 이전과 동일하게 작동하지만 경로를 사용하지 않습니다. 이는 향후 URL 경로가 변경되는 경우 유용할 수 있다. router-links이름을 사용하여 참조하기 때문에 경로가 변경되더라도 모두 변경할 필요가 없다.




## Programmatic Navigation 


rotuer의 인스턴스 메소드들을 사용하여 프로그램적으로 navigate하는 방법을 알아보자.


* 다른 URL로 네비게이트하기 위해서는 **router.push()** 를 사용한다. 이것은 새로운 엔트리를 history stack에 추가할 것이다. 그래서 사용자가 브라우저 백 버튼을 클릭하면 이전 URL로 돌아갈 것이다.
* 이것은 router-link를 클릭했을 때 내부적으로 호출되는 메소드이다. 따라서 <router-link :to="...">를 클릭하는 것은 router.push()를 호출하는 것과 동일하다.

**사용 방법**
```jsx
router.push(location, onComplete?, onAbort?)
```


| Declarative | Programmatic |
|-----|----|
|\<router-link :to=""\> | router.push(...) |


### Home으로 보내기 
User.vue에 버튼을 추가하고 홈으로 이동할 수 있도록 버튼 이벤트를 구현한다.  

useRouter를 import하고 useRouter()를 이용하여 router 인스턴스를 생성한다. 

```html
<script>
  import { useRoute, useRouter } from 'vue-router'
  export default defineComponent({
    setup() {
      // router 
      const router = useRouter(); 
    }
  }
</script>
```
push() 함수를 사용하여 푸시한다. 
```jsx
router.push('/')
```
다음은 전체 코드이다. 

**User.vue**
```html
<template>
  <div>
    <h1>User</h1>
    <input type="button" value="Go Home"  @click="onGoHome">
    <router-view></router-view>
  </div>
  
</template>
<script>
import { defineComponent } from 'vue'
import { useRoute, useRouter } from 'vue-router'

export default defineComponent({
  setup() {
    // router 
    const router = useRouter(); 
    
    const onGoHome = () => { 
      router.push('/')
    }

    return {
      onGoHome 
    }
  },
})
</script>
```
Named Route를 사용하고 싶으면 다음과 같이 작성한다. 
```jsx
router.push({ name: 'Home'})
```




**push 사용 예시**
```jsx
// literal string path
router.push('/users/eduardo')

// object with path
router.push({ path: '/users/eduardo' })

// named route with params to let the router build the url
router.push({ name: 'user', params: { username: 'eduardo' } })

// with query, resulting in /register?plan=private
router.push({ path: '/register', query: { plan: 'private' } })

// with hash, resulting in /about#team
router.push({ path: '/about', hash: '#team' })
```

### query 사용 
Routing할 때 query를 사용할 수 있다.  query 속성을 사용하여 GET 요청할 때와 같은 방식으로 파라미터터를 전달할 수 있다. 

**User.vue**
```jsx
    const onGoHome = () => { 
       router.push({ name: 'Home', query: {  uname: 'Hong'}})
    }
```
파라미터는 'route.query.파라미터명'으로 값을 꺼낼 수 있다. 

**Home.vue**
```html
<template>
  <div>
    <h1>Home</h1>
  </div>
  
</template>
<script>
import { useRoute, useRouter } from 'vue-router'
export default {
  setup() {
    const route = useRoute()
    console.log(route.query.uname)
  }
}
</script>
```

## 아직 설명하지 않은 내용 

* Named View
* hash 를 사용한 push 
* Route Components에 Props 전달하기 






## References
[Query 사용 설명](https://velog.io/@skyepodium/vue-router%EB%A1%9C-%EB%8D%B0%EC%9D%B4%ED%84%B0-%EC%A0%84%EB%8B%AC%ED%95%98%EA%B8%B0-eskrsmr3)
[Vue3  Named Routes](https://www.vuemastery.com/blog/vue-router-a-tutorial-for-vue-3/)











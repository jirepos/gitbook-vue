# Vue Router

## 설치

npm i vue-router는 이전 버전이고 Vue3에서 사용하려면 @next를 붙여야 한다.

```shell
npm i vue-router@next
```

## router.js 작성

Vuex와 마찬가지로 create\* 함수를 제공한다.

```javascript
import { createWebHistory, createRouter } from 'vue-router';
```

다음과 같이 router.js를 작성한다

```javascript
import { createWebHistory, createRouter } from 'vue-router';

const routes = [
  {
    path: '/',
    name: 'Home',
    component: () => import('@/views/Home.vue'), // 동적 import
  },
  {
    path: '/login',
    name: 'Login',
    component: () => import('@/views/Login.vue'),
  },
]

export const router = createRouter({
  history: createWebHistory(),
  routes,
});
```

## main.js

main.js에서 router.js를 import하고 use()를 사용하여 사용할 수 있게 한다.

```javascript
import { router } from './router.js' 

createApp(App)
 .use(store)
 .use(router)
 .mount('#app')
```

## App.vue

App.vue 파일에 \<router-view>를 추가한다.

```javascript
<template>
  <router-view />
</template>
<script setup>
</script>
```

## src/views에 Home.vue와 Login.vue를 생성한다.

```javascript
// Home.uve
<template>
   <h1>Home</h1>
</template>
<script setup>
</script>
```

```javascript
// Login.vue
<template>
   <h1>Login View</h1>
</template>
<script setup>
</script>
```

다음을 주소창에 입력하면 Home.vue가 표시된다.

```shell
http://localhost:3000/
```

다음을 입력하면 Login.vue가 표시된다.

```shell
http://localhost:3000/login
```

다음과 같이 직접 링크를 추가할 수도 있다.

```javascript
<template>
  <a href="/">Home</a><br>
  <a href="/login">Login</a><br>
  <router-view />
</template>
<script setup>
</script>
```

## 정적 라우팅 링크 생성

router-link를 사용하여 링크를 만들 수도 있다.

```javascript
<div id="app">
  <h1>Hello App!</h1>
  <p>
    <!-- use the router-link component for navigation. -->
    <!-- specify the link by passing the `to` prop. -->
    <!-- `<router-link>` will render an `<a>` tag with the correct `href` attribute -->
    <router-link to="/">Go to Home</router-link>
    <router-link to="/about">Go to About</router-link>
  </p>
  <!-- route outlet -->
  <!-- component matched by the route will render here -->
  <router-view></router-view>
</div>
```

## 동적 경로 매칭

파라미터를 사용하여 동적으로 경로를 매핑할 수 있다. 파라미터는 콜론으로 표시한다.

```javascript
const routes = [
  {
    path: '/login/:id',
    name: 'Login',
    component: () => import('@/views/Login.vue'),
  },
]
```

to에 /login/johnny와 /login/jessy와 같은 URL들은 loing route에 매핑될 것이다.

```javascript
<template>
  <router-link to="/login/jessy">Go to Login</router-link><br>
  <router-view />
</template>
```

Component에서 파라미터는 route.params으로부터 가져올 수 있다. useRoute를 import하고 useRoute()를 사용하여 route를 구한다.

```javascript
<script setup>
import { ref, reactive , watch } from 'vue' 
import { useRoute } from 'vue-router'

const route = useRoute()
console.log(route.params)
console.log(route.params.id)
</script>
```

같은 라우트 경로에 여러개의 params를 가질 수 있다.

| pattern                        | matched path             | route.params                           |
| ------------------------------ | ------------------------ | -------------------------------------- |
| /users/:username               | /users/eduardo           | { username: 'eduardo' }                |
| /users/:username/posts/:postId | /users/eduardo/posts/123 | { username: 'eduardo', postId: '123' } |

### 파라미터 변경에 반응하기

watch()를 사용하여 파라미터가 변경되는 것을 확인할 수 있다.

```javascript
watch( () => route.params, (newVal, oldVal) => {
   console.log("old=>" + oldVal.id)
   console.log("new=>" + newVal.id)
})
```

## 중첩 라우팅

컴포넌트가 하위 컴포넌트를 포함할 할 때의 라우팅을 하는 방법을 알아 보자. User 컴포넌트를 만들고 User 컴포넌트의 하위 컴포넌트인 Profile 컴포넌트와 UserPosts 컴포넌트를 만들다.

```javascript
// Prorile.vue 
<template>
  <h2>Profile</h2>
</template>
<script setup>
</script>
```

```javascript
// UserPosts.vue
<template>
  <h2>UserPosts</h2>
</template>
<script setup>
</script>
```

User 컴포넌트에 router-view를 추가한다.

```javascript
// User.vue
<template>
  <h2>User</h2>
  <router-view></router-view>
</template>
<script setup>
</script>
```

router.js에 User 컴포넌트에 대한 라우팅 경로를 추가한다. 하위 컴포넌트들에 대한 경로 매핑을 위해 children을 사용하여 다음과 같이 작성한다.

```javascript
import { createWebHistory, createRouter } from 'vue-router';

const routes = [
  {
    path: '/user/:id',
    name: 'User',
    component: () => import('@/views/User.vue'),
    children: [
      {
        path: 'profile',
        component: () => import('@/views/Profile.vue')
      },
      {
        path: 'posts',
        component: () => import('@/views/UserPosts.vue')
      }
    ]
  },
]
```

이제 /user/jenny/profile 경로를 호출하면 Profile 컴포넌트가 표시될 것이고 /user/jenny/posts 경로를 호출하면 UserPosts 컴포넌트가 표시될 것이다.

추가할 것이 하나 더 있는데, 만일 /user/jenny 경로를 호출하면 하위 컴포넌트들이 아무것도 표시되지 않을 것이다. 이경우에는 빈 경로 표시를 다음과 같이 추가한다.

```javascript
  children: [
      // UserHome will be rendered inside User's <router-view>
      // when /user/:id is matched
      { path: '', component: UserHome },
      // ...other sub routes
    ],
```

## Programmatic Navigation

rotuer의 인스턴스 메소드들을 사용하여 프로그램적으로 navigate하는 방법을 알아보자.

다른 URL로 네비게이트하기 위해서는 router.push()를 사용한다. 이것은 새로운 엔트리를 history stack에 추가할 것이다. 그래서 사용자가 브라우저 백 버튼을 클릭하면 이전 URL로 돌아갈 것이다.

이것은 router-link를 클릭했을 때 내부적으로 호출되는 메소드이다. 따라서 \<router-link :to="...">를 클릭하는 것으노 router.push()를 호출하는 것과 동일하다.

| Declarative           | Programmatic     |
| --------------------- | ---------------- |
| \<router-link :to=""> | router.push(...) |

```javascript
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

path가 주어지면 params는 무시된다. qujery의 경우는 아니다. 대신에, route의 name을 제공하거나, parameter와 함께 전체 path를 매뉴얼적으로 명시할 필요가 있다.

```javascript
const username = 'eduardo'
// we can manually build the url but we will have to handle the encoding ourselves
router.push(`/user/${username}`) // -> /user/eduardo
// same as
router.push({ path: `/user/${username}` }) // -> /user/eduardo
// if possible use `name` and `params` to benefit from automatic URL encoding
router.push({ name: 'user', params: { username } }) // -> /user/eduardo
// `params` cannot be used alongside `path`
router.push({ path: '/user', params: { username } }) // -> /user
```

router.push와 모든 다른 네비게이션 메소드는 Promise를 리턴한다.

## Replace current location

router.push와 같이 동작한다. 유일한 차이는 새로운 history 엔트리에 추가 없이 네비게이트한다. 이름이 제안됨으로써, 현재 엔트리를 리플레이스한다.

| Declarative                      | Programmatic        |
| -------------------------------- | ------------------- |
| \<router-link :to="..." replace> | router.replace(...) |

router.push에 전달되는 routeLOcation에 replace:true 속성을 직접적으로 추가하는 것도 가능하다.

```javascript
router.push({ path: '/home', replace: true })
// equivalent to
router.replace({ path: '/home' })
```

## Named Routes

path와 함께 name을 route에 줄 수 있다.

```javascript
const routes = [
  {
    path: '/user/:username',
    name: 'user',
    component: User
  }
]
```

컴포넌트의 prop에 object를 전달할 수 있다.

```javascript
<router-link :to="{ name: 'user', params: { username: 'erina' }}">
  User
</router-link>
```

router.push()로 프로그램적으로 사용된 동일한 객체이다.

```javascript
router.push({ name: 'user', params: { username: 'erina' } })
```

## Named Views

동시에 여러개의 views를 표시할 필요가 있다. 예를 들어 sidebar 뷰와 main 뷰가 있는 레이아웃을 생성할 때 필요하다.

router-view는 여러개 가질 수 있고 각각에 이름을 줄 수 있다. 이름이 주어지지 않는 view는 default가 이름으로 주어진다.

```javascript
<router-view class="view left-sidebar" name="LeftSidebar"></router-view>
<router-view class="view main-content"></router-view>
<router-view class="view right-sidebar" name="RightSidebar"></router-view>
```

view는 컴포넌트를 사용해서 렌더링된다. 따라서 같은 라우팅 경로로 여러개의 컴포넌트들이 요구된다. components를 사용한다.

```javascript
const router = createRouter({
  history: createWebHashHistory(),
  routes: [
    {
      path: '/',
      components: {
        default: Home,
        // short for LeftSidebar: LeftSidebar
        LeftSidebar,
        // they match the `name` attribute on `<router-view>`
        RightSidebar,
      },
    },
  ],
})
```

### Nested Named Views

Nested Views가 있는 named views를 사용하는 것이 가능하다.

```javascript
// template
<!-- UserSettings.vue -->
<div>
  <h1>User Settings</h1>
  <NavBar />
  <router-view />
  <router-view name="helper" />
</div>
```

```javascript
{
  path: '/settings',
  // You could also have named views at the top
  component: UserSettings,
  children: [{
    path: 'emails',
    component: UserEmailsSubscriptions
  }, {
    path: 'profile',
    components: {
      default: UserProfile,
      helper: UserProfilePreview
    }
  }]
}
```

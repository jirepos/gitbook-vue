# Trouble Shooting

## \<template>의 eslint 관련 오류

Visual Studio Code에서 다음과 같이 eslint와 관련된 오류아닌 오류가 표시되는 경우가 있다. 

원인은 template 안에 하나의 root만 있어야 하는데 여러개의 tag가 루트에 있기 때문이다.

```html
<template>
  <h1>{{ msg }}</h1>
  <button @click="count++">count is: {{ count }}</button>
  <p>Edit <code>components/HelloWorld.vue</code> to test hot module replacement.</p>
</template>
```

다음과 같이 수정한다.

```html
<template>
  <div>
    <h1>{{ msg }}</h1>
    <button @click="count++">count is: {{ count }}</button>
    <p>Edit <code>components/HelloWorld.vue</code> to test hot module replacement.</p>
  </div>
</template>
```

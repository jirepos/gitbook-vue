# Vue 3에서 다국어 처리

## Installation

```javascript
yarn add vue-i18n@next
```

## i18n 사용방법

Vue 3의 Composition API를 사용하여 개발을 할 것이기 때문에 I18N도 Vue3 기반에서 사용하는 방법을 알아 볼 것이다.

i18n 객체를 생성하기 위해서 createI18n() 함수를 사용한다. 사용하기 위해서 다음과 같이 import한다.

```javascript
import { createI18n } from 'vue-i18n'
```

Compositon API에서 i18n을 사용하기 위해서는 legacy 옵션의 값을 false로 설정한다.

```javascript
const i18n = createI18n({
  // something vue-i18n options here ...
  legacy: false, // you must set `false`, to use Composition API
})
```

다국어 메시지를 담을 객체를 다음과 같이 생성한다.

```javascript
const messages = { 
  en: { 
    hello: "Hello"

  }, 
  ko: { 
    hello: "안녕하세요"
  }
}
```

createI18n() 함수를 사용하여 i18n 객체를 생성한다. 생성한 객체를 use()를 사용하여 추가한다.

```javascript
import { createI18n } from 'vue-i18n'

const i18n = createI18n({
  // something vue-i18n options here ...
  legacy: false, // you must set `false`, to use Composition API
  locale: 'ko', // set locale
  fallbackLocale: 'en', // set fallback locale
  messages // set locale messages
})

createApp(App)
 .use(store)
 .use(router)
 .use(i18n)
 .mount('#app')
```

template에서 다국어 메시지를 사용하기 위해서는 useI18n을 import해야 한다.

```javascript
import { useI18n } from 'vue-i18n'
```

useI18n() 함수를 사용하여 t 객체를 생성한다.

```javascript
const { t }= useI18n()
```

template에서 t를 사용하여 다음과 같이 다국어 메시지를 출력한다.

```javascript
<template>
 <div>
    여기 다국어 {{ t("hello") }}
  </div>
</template>
```

## Message Translation

### 파라미터를 가진 text

다국어 메시지 named에 msg 파라미터의 값을 출력하려면 다음과 같이 한다.

```javascript
<template>
 <p>{{ t('named', { msg }) }}</p>
</template>
<script setup>

const msg = "메시지" 
const { t }= useI18n({
  messages: {
    ko: {
        named: '{msg} 세상이야!',
    }
  } // messages
</script>
```

파라미터를 직접 추가할 수도 있다.

```javascript
<template>
<p>{{ t('named', { msg: 'Vue.js'} )  }}</p>
</template>
<script setup>
const { t }= useI18n({
  messages: {
    en: {
        named: '{msg} world!',
    },
    ko: {
        named: '{msg} 세상이야!',
    }
  } // messages
</script>
```

### 두 개의 파라미터를 가진 텍스트

다음과 같이 메시지를 작성한다.

```javascript
  messages: {
    ko: {
         text: 'This is {owner} {item}.'
    }
  } // messages
```

다음은 This is my bag을 출력한다.

```javascript
 <p>{{ t('text', { owner:'my', item: 'bag'}   ) }}</p>
```

변수를 선언하고 변수 자체를 넘길 수 있다.

### 배열

list 다국어 메시지를 배열로 작성한다.

```javascript
  messages: {
    ko: {
        list:  ['사과', '딸기']
    }
  } // messages
```

다음은 사과가 출력된다.

```javascript
     <p>{{ t('list[0]') }}</p>
```

다음은 딸기가 출력된다.

```javascript
     <p>{{ t('list[1]') }}</p>
```

### 단수 또는 복수

단수와 복수를 나타내기 위해 파이프(|) 기호를 사용한다.

```javascript
  messages: {
    ko: {
        text:  ' flower | flowers'
    }
```

아래와 같이 템플릿을 작성하면 flower가 출력된다.

```javascript
     <p>{{ t('text', 1 ) }}</p>
```

아래와 같이 템플릿을 작성하면 flowers가 출력된다. 3을 입력해도 flowers를 출력한다.

```javascript
     <p>{{ t('text', 2 ) }}</p>
```

### 숫자

{n}을 사용하고 숫자를 전달하면 숫자로 치환된다. 다음과 같이 메시지를 작성한다.

```javascript
  messages: {
    ko: {
        text:  ' {n} flowers'
    }
  } // messages
```

다음과 같이 템플릿을 작성하면 2 flowers가 출력된다.

```javascript
 <p>{{ t('text', 2 ) }}</p>
```

파이프 기호를 사용하여 다음과 같이 메시지를 작성한다.

```javascript
messages: {
    ko: {
        text:  'flower | {n} flowers'
    }
}
```

다음과 같이 템플릿을 작성하면 'flower'가 출력된다.

```javascript
 <p>{{ t('text', 1 ) }}</p>
```

다음과 같이 템플릿을 작성하면 '2 flowers'가 출력된다.

```javascript
 <p>{{ t('text', 2 ) }}</p>
```

다음과 같이 템플릿을 작성하면 '5 flowers'가 출력된다.

```javascript
 <p>{{ t('text', 5 ) }}</p>
```

다음과 같이 메시지를 작성한다. 세개의 요소가 있다.

```javascript
  messages: {
    ko: {
        text:  'no flower | one flower | {numberOfFlowers} flowers'
    }
  } // messages
```

다음과 같이 템플릿을 작성하면 no flower가 출려된다.

```javascript
 <p>{{ t('text', 0) }}</p>
```

```shell
no flower
```

다음과 같이 1을 전달하면 one flower가 출력된다.

```javascript
<p>{{ t('text', 1) }}</p>
```

다음과 같이 2을 전달하면 flowers가 출력된다.

```javascript
<p>{{ t('text', 2) }}</p>
```

두번째 인자로서 이름을 가진 아이템들을 전단할 수 있다.

```javascript
  messages: {
    ko: {
        apple: 'no apples | one apple | {count} apples'
    }
  } // messages
```

다음과 같이 템플릿을 작성하면 10 apples가 출력된다.

```javascript
<p>{{ t('apple', { count: 10 }) }}</p>
```

세번째 인자로 디폴트 값을 전달할 수 있다.

```javascript
     <p>{{ t('apple', { count: 15 }, 10) }}</p>
```

그래서 다음과 같이 전달하면 10 apples가 출력된다.

```javascript
<p>{{ t('apple',  10) }}</p>
```

다음과 같이 메시지를 작성한다.

```javascript
  messages: {
    ko: {
         banana: 'no bananas | {n} banana | {n} bananas'
    }
  } // messages
```

다음은 no bananas를 출력한다.

```javascript
<p>{{ t('banana',0) }}</p>
```

다음은 1 banana를 출력한다.

```javascript
<p>{{ t('banana',1) }}</p>
```

다음은 2 banana를 출력한다.

```javascript
 <p>{{ t('banana',2) }}</p>
```

다음은 1 banana를 출력한다.

```javascript
<p>{{ t('banana', { n: 1 }, 1) }}</p>
```

다음은 100 bananas를 출력한다.

```javascript
<p>{{ t('banana', { n: 100 }, 1) }}</p>
```

다음은 too many bananas를 출력한다.

```javascript
 <p>{{ t('banana', { n: 'too many' }, 100) }}</p>
```

### 내장 객체 변환

다국어 메시지를 구조화하여 사용하는 방법을 살펴보자. 다음과 같이 main 객체를 생성하고 객체의 필드로 메시지를 정의한다.

```javascript
  messages: {
    ko: {
         main: {
           title: "메인화면"
         }
    }
  } // messages
```

템플릿에서 다음과 같이 작성하면 "메인화면"이 출력될 것이다.

```javascript
 <p>{{ t('main.title') }}</p>
```

### 다른 다국어 메시지 참조하기

구조화된 다국어 메시지를 사용하여 다른 메지지 그룹의 다국어 메시지를 사용하는 방법을 살펴본다.

common 모듈과 main 모듈로 다국어 메시지를 나눈다. @:를 사용하여 common에 있는 yes 메시지를 참조한다.

```javascript
  messages: {
    ko: {
         common: { 
           yes: "Yes"
         },
         main: {
           title: "메인화면",
           button1: "@:common.yes"
         }
    }
  } // messages
```

템플릿에서 사용하는 방법은 동일하다.

```javascript
<p>{{ t('main.button1') }}</p>
```

## 두 개의 메시지를 합치는 방법

다국어 메시지를 모듈별로 별도의 파일로 분리하여 생성할 필요가 있을 수 있다. 그런 경우 각각의 다국어 메시지를 Object.assign()을 사용하여 병합한다.

```javascript
// 메신지만 개별 파일로 빼서 
const i18nMessages = { 
  en: { 
    hello: "Hello"

  }, 
  ko: { 
    hello: "안녕하세요"
  }
}
const i18nMessages2 = { 
  en: { 
    hello2: "Hello2"

  }, 
  ko: { 
    hello2: "안녕하세요 222"
  }
}

const i18n_msg = {}  // 빈 메시지 객체를 만들고 
Object.assign(i18n_msg, i18nMessages)   // assign()을 사용하여 병합
Object.assign(i18n_msg, i18nMessages2)  // 다시 병함 

const i18n = createI18n({
  messages: i18nMessages  // messages에 객체 설정 
})
```

## References

[Vue I18N 공식사이트](https://vue-i18n.intlify.dev)\
[How to translate you Vue.js application....](https://www.codeandweb.com/babeledit/tutorials/how-to-translate-your-vue-app-with-vue-i18n)

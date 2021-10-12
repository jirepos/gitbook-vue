# Slot

자식 컴포넌트가 slot을 만들어 제공하면 부모 컴포넌트에서 자식 컴포넌트에 내용을 추가할 수 있다.

## Slot 기본

SlotButton.vue 자식 컴포넌트를 만든다. slot 태그를 작성한다.

```javascript
// SlotButton.vue
<template>
  <div>
    <slot>제공된 컨텐츠가 없는 경우에 볼 수 있다.</slot>
  </div>
</template>
<script setup>
</script>
```

SlotMain.vue를 작성한다. 이 컴포넌트는 SlotButton 컴포넌트를 포함한다. template에 컴포넌트를 추가하고 컴포넌트 시작 태그와 종료 태그에 내용을 입력하면 하위 컴포넌트에 그 내용이 표시된다.

```javascript
// SlotMain.vue
<template>
  <div>Slot</div>
  <slot-button>
    이 내용이 slot에 들어간다.
  </slot-button>
</template>
<script setup>
import SlotButton from './SlotButton.vue'
</script>
```

slot은 HTML을 포함하여 어떠한 코드라도 포함할 수 있다. 심지어 컴포넌트를 포함시킬 수도 있다.

만약 SlotButton. 컴포넌트의 템플릿이 \<slot> 코드를 가지고 있지 않다면, 태그 내부에 위치한 모든 컨텐츠는 무시된다.

## 렌더 스코프

다음과 같이 slot 내부에 데이터를 사용하는 경우에

```javascript
  <slot-button>
    {{item.name}}
  </slot-button>
```

이 slot은 slotButton의 스코프에 접근할 수 없다. 다시말하면 SlotButton안의 변수나 메소드들을 사용할 수 없다.

> 부모 템플릿에 있는 모든 요소는 부모 스코프에서 컴파일되고, 자식 템플릿에 있는 모든 요소는 자식 스코프에서 컴파일됩니다

## Fallback 컨텐츠

부모 컴포넌트에서 자식 컴포넌트에 아무 것도 전달하지 않는 경우에 대체 컨텐츠가 없으면 아무것도 표현되지 않는다. 그래서 대체할 수 있는 컨텐츠를 넣어 두는 것이 좋다.

즉 이렇게 호출하는 경우에

```javascript
<slot-button></slot-button>
```

아무것도 표시되지 않기 때문에 다음과 같이 대체 컨텐츠를 넣어 주는 것이 좋다.

```javascript
<slot>이건 대체 컨텐츠</slot>
```

## 이름을 갖는 Slot

컴포넌트에 여러개의 슬롯을 둘 수 있는데 이름을 줄 수 있다. 이름 없는 슬롯은 default이다.

NameSlot.vue를 다음과 같이 작성한다. 세 개의 슬롯을 정의했는데 두 개는 이름이 있는 것을 볼 수 있다.

```javascript
<template>
  <hr>
  여기는 Named Slot <br>
  <slot name="header"></slot>
  <slot></slot>
  <slot name="footer"></slot>
</template>
<script setup>

</script>
```

SlotMain.vue에서 NamedSlot을 임포트한다. template 태그와 v-slot 지시자를 사용하여 이름을 준다.

```javascript
<template>
  <named-slot>
    <template v-slot:header>
      <h1>heder slot을 대체함</h1>
    </template>
    <template v-slot:footer>
      <h1>footer slot을 대체함</h1>
    </template>
    <p>default slot을 대체함</p>
  </named-slot>
</template>
<script setup>
import NamedSlot from "./NamedSlot.vue";
</script>
```

## 스코프를 갖는 Slot

부모 컴포넌트의 범위에서 v-slot에 연결한 슬롯 속성(slotProps)를 사영하면 자식 컴포넌트의 데이터를 사용할 수 있다.

자식 컴포넌트에 사용자 객체를 만든다.

```javascript
<script setup>

import { reactive } from 'vue' 

let user = reactive({
  lastName: "김",
  firstName: "장군"
})

</script>
```

슬롯을 정의할 때 v-bind를 사용한다. v-bind의 속성명을 user로 설정한다. 이름이 없는 default 슬롯을 만든다.

```javascript
<template>
   여기는 slotProps를 사용한다. <br>
  <slot v-bind:user="user">이건 이름이 없는 slot</slot>  
</template>
```

부모 컴포넌트에서 default 슬롯에 대해서는 이름이 필요하지 않지만 이름을 지정했다. slotProps를 사용하여 user 객체에 접근할 수 있다.

```javascript
<template>
  <slot-scope v-slot:default="slotProps">{{ slotProps.user.firstName }} </slot-scope>
</template>
```

v-slot="slotProps" 대신에 v-slot="{user}"를 사용할 수 있습니다. user 객체를 바인딩한 slot 하나를 footer로 이름 붙여서 정의한다.

```javascript
<template v-slot:footer="{user}">{{user.firstName}}</template>
```

이제 user 객체를 slotProps를 사용하지 않고 {user}를 사용하여 접근할 수 있다.

```javascript
<template v-slot:footer="{user}">{{user.firstName}}</template>
```

# Slots 

자식 컴포넌트가 slot을 만들어 제공하면 부모 컴포넌트에서 자식 컴포넌트에 내용을 추가할 수 있다.

## slot 기본 
template에 \<slot\> 태그를 추가하기만 하면 slot이 제공된다. 

```html
<template>
  <div>
    <div>
    <slot>
      제공된 컨텐츠가 없는 경우에 볼 수 있다. 
    </slot>
    </div>
    <div>
      <input type="button" value="Input">
    </div>
    
  </div>
</template>

<script>
export default {
  setup() {
  },
}
</script>

<style scoped>
div { 
  background-color: yellow; 
}
</style>
```
MyButton을 임포트하고 \<my-button\> 태그 사이에 컨텐츠를 작성하기만 하면 slot이 제공된 컨텐츠로 대체된다. 
```html
<script setup>
import { ref } from 'vue'
import MyButton from '@/components/c09/MyButton.vue'
</script>

<template>
  <div>
    <my-button>이 내용이 슬롯에 들어간다</my-button>
  </div>
</template>

<style>
</style>
```

slot은 HTML을 포함하여 어떠한 코드라도 포함할 수 있다. 심지어 컴포넌트를 포함시킬 수도 있다.  컴포넌트의 템플릿이 <slot> 코드를 가지고 있지 않다면, 태그 내부에 위치한 모든 컨텐츠는 무시된다.




## Fallback 컨텐츠 
부모 컴포넌트에서 자식 컴포넌트에 아무 것도 전달하지 않는 경우에 대체 컨텐츠가 없으면 아무것도 표현되지 않는다. 그래서 대체할 수 있는 컨텐츠를 넣어 두는 것이 좋다.
즉 이렇게 호출하는 경우에
```html
<slot-button></slot-button>
```
아무것도 표시되지 않기 때문에 다음과 같이 대체 컨텐츠를 넣어 주는 것이 좋다
```html
<slot>이건 대체 컨텐츠</slot>
```

## 이름을 갖는 슬롯 
컴포넌트에 여러개의 슬롯을 둘 수 있는데 이름을 줄 수 있다. 이름 없는 슬롯은 default이다.
NameSlot.vue를 다음과 같이 작성한다. 세 개의 슬롯을 정의했는데 두 개는 이름이 있는 것을 볼 수 있다.


* 이름 없는 슬롯은 default이다


아래는 세 개의 슬롯을 정의했는데 두 개는 이름이 있는 것을 볼 수 있다.

```html
<template>
  <div>
    <slot name="header"></slot>
    <slot></slot>
    <slot name="footer"></slot>
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
slot이 있는 컴포넌트를 사용하는 컴포넌트에서는 template 태그와 v-slot 지시자를 사용하여 이름을 준다.
```html
<script setup>
import NamedSlot from '@/components/c11-slots/NamedSlot.vue'

</script>

<template>
  <div>
    <named-slot>
      <template v-slot:header>header</template>
      <template v-slot:footer>footer</template>
      <p>디폴트 슬롯을 대체한다.</p>
    </named-slot>
  </div>
</template>
```

## Render Scope 

자식 컴포넌트에 변수를 하나 선언해 보자. 
```html
export default defineComponent({
  setup() {

    const childMessage = ref("자식 메시지이다")
    return  {
      childMessage 
    }
  },
})
```
그리고 slot을 사용하는 부모 컴포넌트에서 childMessage 변수를 렌더링 해보자. 
```html
<template>
  <div>
    <named-slot>
      <template v-slot:header>header</template>
      <template v-slot:footer>
        footer <br>
        {{childMessage}}
      </template>
      <p>디폴트 슬롯을 대체한다.</p>
    </named-slot>
  </div>
</template>
```

그러나 childMessage는 부모에서는 사용할 수 없다. 다시 말하면 하위 컴포넌트의 안의 변수난 메소드들을 사용할 수 없다. 



> 부모 템플릿에 있는 모든 요소는 부모 스코프에서 컴파일되고, 자식 템플릿에 있는 모든 요소는 자식 스코프에서 컴파일됩니다




## 스코프를 갖는 슬롯 
부모 컴포넌트의 범위에서 v-slot에 연결한 슬롯 속성(slotProps)를 사용하면 자식 컴포넌트의 데이터를 사용할 수 있다.
자식 컴포넌트에 사용자 객체를 만든다.

```html
<script>
import { defineComponent, reactive } from "vue";

export default defineComponent({
  setup() {
    let user = reactive({
      lastName: "김",
      firstName: "장군",
    });
    return {
      user,
    };
  },
});
</script>
```
슬롯을 정의할 때 v-bind를 사용한다. v-bind의 속성명을 user로 설정한다. 이름이 없는 default 슬롯을 만든다.

```html
<template>
   <slot v-bind:user="user">이건 이름이 없는 슬롯</slot>
</template
```
부모 컴포넌트에서 default 슬롯에 대해서는 이름이 필요하지 않지만 이름을 지정했다. slotProps를 사용하여 user 객체에 접근할 수 있다.
```html
<script setup>
import { ref } from 'vue'
import SlotScope from '@/components/c11-slots/SlotScope.vue'
</script>
<template>
  <div>
    <slot-scope v-slot:default="slotProps">{{slotProps.user.firstName}}</slot-scope>
  </div>
</template>
```

v-slot="slotProps" 대신에 v-slot="{user}"를 사용할 수 있습니다. user 객체를 바인딩한 slot 하나를 footer로 이름 붙여서 정의한다.
```html
<template v-slot:footer="{user}">{{user.firstName}}</template>
```
이제 user 객체를 slotProps를 사용하지 않고 {user}를 사용하여 접근할 수 있다.

## 이름을 갖는 Slot의 축약형
v-on나 v-bind처럼, v-slot도 매개변수 앞에 오는 것(v-slot:)을 #을 이용해 축약할 수 있습니다. 예를 들어, v-slot:header는 #header로 작성할 수 있습니다.


```html
<base-layout>
  <template #header>
    <h1>Here might be a page title</h1>
  </template>

  <template #default>
    A paragraph for the main content.</p>
    And another one.</p>
  </template>

  <template #footer>
    Here's some contact info</p>
  </template>
</base-layout>
```


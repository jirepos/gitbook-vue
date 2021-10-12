# Tagify

쓸만한 Tag 컴포넌트는 [Tagify](https://reposhub.com/javascript/form-input-widgets/yairEO-tagify.html)이다.

설치

```shell
npm i @yaireo/tagify --save
```

Vue 사용샘플은 [https://codesandbox.io/s/tagify-tags-component-vue-example-l8ok4?file=/src/main.js](https://codesandbox.io/s/tagify-tags-component-vue-example-l8ok4?file=/src/main.js)에서 확인할 수 있다.

Tagify.vue 파일을 생성하고 다음과 같이 코드를 작성한다.

```html
<template v-once>
  <input :value="value"   class='customLook'  v-on:change="onChange">
</template>

<script>
import Tagify from "@yaireo/tagify/dist/tagify.min.js";
import "@yaireo/tagify/dist/tagify.css";

export default {
  name: "Tags",
  props: {
    mode: String,
    settings: Object,
    value: [String, Array],
    onChange: Function
  },
  watch: {
    value(newVal, oldVal) {
      // 동작하지 않는다. 확인해보아야 한다.
      this.tagify.loadOriginalValues(newVal)
    },
  },
  mounted() {
    this.tagify = new Tagify(this.$el, this.settings)
  }
};
</script>
<style>
.customLook { 
  --tag-bg: red;
  --tag-text-color: white;
  --tag-remove-btn-color: white; 
  --tag-hover: blue;
}
</style>
```

Tag에 CSS를 적용하고 싶으면 style 태그 안에 변수명을 사용하여 스타일을 정의한다. Tag에 스타일을 적용하기 위해 사용할 수 있는 변수에 대해 더 알아보려면 [https://reposhub.com/javascript/form-input-widgets/yairEO-tagify.html](https://reposhub.com/javascript/form-input-widgets/yairEO-tagify.html)을 참고한다.

```html
<style>
.customLook { 
  --tag-bg: red;
  --tag-text-color: white;
  --tag-remove-btn-color: white; 
  --tag-hover: blue;
}
</style>
```

template의 input tag에 class를 적용한다.

```html
 <input :value="value"   class='customLook'  v-on:change="onChange">
```

Tgify.vue를 사용하기 위해 MyTagify.vue 파일을 생성하고 다음과 같이 작성한다.

```html
<template>
   <Tags ref="myRef" 
       :settings="tagifyStuff.tagifySettings" 
       :suggestions="tagifyStuff.suggestions" 
       :value="tagifyStuff.value" 
       :onChange="onTagsChange"/>
</template>

<script>
import { defineComponent , ref,  reactive } from 'vue' 
import Tags from './Tagify.vue'


export default defineComponent({
  components: {
    Tags 
  },
  setup(props) {
    const myRef = ref(null)
    const tagifyStuff = reactive({ 
      value: "a,b,c",   // 값을 설정하면 초기값이 된다.
      //value: "",   // 값을 설정하면 초기값이 된다.
      tagifySettings: {
        placeholder: "Tag를 입력하세요",
        readOnly: "readOnly",   // 읽기 전용, 작동하지 않음 
        //whitelist: ["Korea", "England", "Japan", "German", "America", "Aus"],
        whitelist: [
          { value: "Korea(aaa@bbb.com)", nickname: "Korea", email: "aaa@bbb.com"},
          { value: "England(aac@bbb.com)", nickname: "England", email: "aac@bbb.com"}
        ],
        userInput: true, // false이면 사용자 입력을 막는다
        dropdown: {
          // sorting은 지원하지 않음 
          //classname: "customLook",  // 작동하지 않음 
          searchKeys: [ "nickname", "email"],  // whitelist에서 검색할 때 사용할 필드 
          maxItems: 5,   // 최대 추천 아이템 수 
          //position: "text",   // dropdown을 입력 텍스트에 가까이 표시
          //closeOnSelect : false,     // 추천을 선택한 후에도 dropdown을 유지하려면 false , true이면 닫힘 
          highlightFirst: true,
          enabled: 1  // 0이면 dropdown을 포커스가 갔을 때 즉시 활성화 , 1이면 타이핑을 했을 때 활성화 
        },
        callbacks: {
          add(e) {
             //console.log("tag added:", e.detail); // tag가 추가되거나 삭제되면 호출된다. 
          }
        }
      },
      suggestions: []
    })
    function updateValue(newValue){
      tagifyStuff.value = newValue
    }

    function onTagsChange(e) {
       console.log("tags changed:", e.target.value); // 태그가 추가되거나 삭제되면 호출된다. 
    }

    return  {
      onTagsChange,
      myRef,
      tagifyStuff
    }

  },
})
</script>
```

Tagify 기본 샘플은 [https://yaireo.github.io/tagify/#users-list](https://yaireo.github.io/tagify/#users-list)을 참고한다. Tagify의 메서드와 속성들을 참고하려면 [https://github.com/yairEO/tagify#css-variables](https://github.com/yairEO/tagify#css-variables)을 참고한다.

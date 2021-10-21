# 클래스와 스타일 바인딩 


## HTML 클래스 바인딩
v-bind:class (v-bind:class의 약자)에 객체를 전달하여 클래스를 동적으로 전환할 수 있습니다.

```html
<script>
import { defineComponent, ref, reactive } from "vue";

export default defineComponent({
  setup(props, context) {

    const isActive = ref(true); 
    return {
      isActive 
    };
  },
});
</script>
<template>
  <div :class="{ active: isActive }">
    hello
  </div>
</template>
<style scoped>
.active { 
  background-color: yellow;
}
</style>
```
여러 개의 클래스를 적용하려면 다음과 같이 한다. 
```html
<script>
import { defineComponent, ref, reactive } from "vue";

export default defineComponent({
  setup(props, context) {

    const isActive = ref(true); 
    const hasError = ref(true); 
    return {
      isActive ,
      hasError 
    };
  },
});
</script>
<template>
  <div :class="{ active: isActive, 'text-danger': hasError }">
    hello
  </div>
</template>
<style scoped>
.active { 
  background-color: yellow;
}
.text-danger { 
  font-weight: bold;
  color: blue; 
}
</style>
```

객체를 사용하여 class를 바인딩할 수 있다. 
```html
<script>
import { defineComponent, ref, reactive } from "vue";

export default defineComponent({
  setup(props, context) {
    const classObj = reactive( {
      active: true, 
      'text-danger': true
    })
    return {
      classObj 
    };
  },
});
</script>
<template>
  <div :class="classObj">
    hello
  </div>
</template>
<style scoped>
.active { 
  background-color: yellow;
}
.text-danger { 
  font-weight: bold;
  color: blue; 
}
</style>
```

## 배열 사용 

배열을 :class에 전달하여 클래스 목록을 적용할 수 있습니다.

```html
<script>
import { defineComponent, ref, reactive } from "vue";

export default defineComponent({

  setup(props, context) {

    const activeClass = ref('active')
    const errorClass = ref('text-danger')
    return {
      activeClass,
      errorClass
    };


  },
});
</script>
<template>
  <div :class="[activeClass, errorClass]">
    hello
  </div>
</template>
<style scoped>
.active { 
  background-color: yellow;
}
.text-danger { 
  font-weight: bold;
  color: blue; 
}
</style>
```


## 인라인 스타일 바인딩 
### 객체 구문 


:style의 객체 구문은 매우 간단합니다. JavaScript 객체라는 점을 제외하면 CSS와 거의 비슷합니다. CSS 속성 이름에는 카멜 케이스(camelCase) 또는 케밥 케이스(kebab-case, 케밥 케이스와 함께 따옴표 사용)를 사용할 수 있습니다.


```html
<script>
import { defineComponent, ref, reactive } from "vue";

export default defineComponent({

  setup(props, context) {

    const activeColor = ref('red')
    const fontSize = ref(30)
    return {
      activeColor,
      fontSize
    };


  },
});
</script>
<template>
  <div :style="{color: activeColor, fontSize: fontSize + 'px'}" >
    hello
  </div>
</template>
<style scoped>
</style>
```

### 객체 스타일 
```html
<script>
import { defineComponent, ref, reactive } from "vue";

export default defineComponent({

  setup(props, context) {

    const styleObject = reactive( {
      color: 'red',
      fontSize: '30px'
    })

    return {
      styleObject,
    };

  },
});
</script>
<template>
  <div :style="styleObject" >
    hello
  </div>
</template>
<style scoped>
</style>
```


### 배열 구문
```html
<script>
import { defineComponent, ref, reactive } from "vue";

export default defineComponent({

  setup(props, context) {

    const baseStyles = ref('color: red')
    const overridingStyles = ref('font-size: 30px')
    return {
      baseStyles,
      overridingStyles
    };

  },
});
</script>
<template>
  <div :style="[baseStyles, overridingStyles]" >
    hello
  </div>
</template>
<style scoped>
</style>
```















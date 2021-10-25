# V-model
HTML 폼 요소들에 대한 양방향 데이터 바인딩을 생성할 수 있는 v-model에 대해서 알아 보겠다. 


## text 요소 
### 단일 변수 
script에 양방향 데이터 바인딩할 변수를 선언한다. 
```javascript
export default {
  setup(props, context) {
    const message = ref('')
    return { 
      message
    }
  },
}
```

template에 간단히 input 요소를 선언한다. v-model 디렉티브를 사용하여 변수와 바인딩한다.  value 속성은 사용하지 않는다. 

> v-model은 모든 form 엘리먼트의 초기 value와 checked 그리고 selected 속성을 무시합니다. 항상 Vue 인스턴스 데이터를 원본 소스로 취급합니다. 컴포넌트의 data 옵션 안에 있는 JavaScript에서 초기값을 선언해야합니다.

v-model은 내부적으로 서로 다른 속성을 사용하고 서로 다른 입력 요소에 대해 서로 다른 이벤트를 전송한다.

* text 와 textarea 태그는 value속성과 input이벤트를 사용한다.
* 체크박스들과 라디오버튼들은 checked 속성과 change 이벤트를 사용한다.
* Select 태그는 value 를 prop으로, change를 이벤트로 사용한다.


```html
<template>
  <div>
    <input type="text" v-model="message"  placeholder="아무거나 입력하세요.">
  </div>
</template>
```

전체 코드는 다음과 같다. 
```html
<script>
import { ref } from 'vue' 

export default {
  setup(props, context) {
    const message = ref('')
    return { 
      message
    }
  },
}
</script>
<template>
  <div>
    <input type="text" v-model="message"  placeholder="아무거나 입력하세요.">
    {{message}}
  </div>
</template>
<style>
</style>
```

### 객체 변수 

user라는 객체를 사용할 대 toRefs로 ref로 변환하여 사용할 수 있다. 
```html
<script>
import { ref, reactive, toRefs } from 'vue' 

export default {
  setup(props, context) {
    const user = reactive({
      name: "홍길동",
      age: 10
    })
    const {  name, age } = toRefs(user)
    return {
      name, 
      age 
    }
  },
}
</script>
```
template에서 다음과 같이 사용할 수 있다. 
```html
<template>
  <div>
    <input type="text" v-model="name"  placeholder="아무거나 입력하세요.">
    {{name}}
  </div>
</template>
```

toRefs를 사용하지 않으면 다음과 같이 한다. 
```html
<script>
import { ref, reactive, toRefs } from 'vue' 

export default {
  setup(props, context) {
    const user = reactive({
      name: "홍길동",
      age: 10
    })
    
    return {
      user 
    }
  },
}
</script>
<template>
  <div>
    <input type="text" v-model="user.name"  placeholder="아무거나 입력하세요.">
    {{user.name}}
  </div>
</template>
<style>
</style>
```

### Textarea
```html
<script>
import { ref, reactive, toRefs } from 'vue' 

export default {
  setup(props, context) {

    const message = ref("")
    return {
      message
    }
  },
}
</script>
<template>
  <div>
    <textarea v-model="message" cols="30" rows="10"></textarea>
    <br>
    {{message}}
  </div>
</template>
<style>
</style>
```


## Checkbox 

```html
<script>
import { ref, reactive, toRefs } from 'vue' 

export default {
  setup(props, context) {
    const checkedNames = ref([]);
    return {
      checkedNames
    }
  },
}
</script>
<template>
  <div>
      <input type="checkbox" id="jack" value="Jack" v-model="checkedNames" />    
      <label for="jack">Jack</label>
      <input type="checkbox" id="john" value="John" v-model="checkedNames" />
      <label for="john">John</label>
      <input type="checkbox" id="mike" value="Mike" v-model="checkedNames" />
      <label for="mike">Mike</label>
      <span>Checked names: {{ checkedNames }}</span>
  </div>
</template>
<style>
</style>
```

이번에는 change 이벤트 핸들러를 작성해서 checkedNames의 변화를 살펴보자. 
```html
<script>
import { ref, reactive, toRefs } from 'vue' 


export default {
  setup(props, context) {
    const checkedNames = ref([]);
    const onChange = () => { 
      //console.log(checkedNames)
      checkedNames.value.forEach( e => console.log(e) )
    }
    return {
      checkedNames,
      onChange
    }
  },
}
</script>
<template>
  <div>
      <input @change="onChange" type="checkbox" id="jack" value="Jack" v-model="checkedNames" />    
      <label for="jack">Jack</label>
      <input @change="onChange" type="checkbox" id="john" value="John" v-model="checkedNames" />
      <label for="john">John</label>
      <input @change="onChange" type="checkbox" id="mike" value="Mike" v-model="checkedNames" />
      <label for="mike">Mike</label>
      <span>Checked names: {{ checkedNames }}</span>
  </div>
</template>
<style>
</style>
```


## Radio 

```html
<script>
import { ref, reactive, toRefs } from 'vue' 

export default {
  setup(props, context) {
    const picked = ref("")
    return {
      picked
    }
  },
}
</script>
<template>
  <div>
      <input type="radio" id="one" value="One" v-model="picked" />
      <label for="one">One</label>
      <br />
      <input type="radio" id="two" value="Two" v-model="picked" />
      <label for="two">Two</label>
      <br />
      <span>Picked: {{ picked }}</span>
  </div>
</template>
<style>
</style>
```

이번에는 Button 요소를 만들고 버튼의 클릭 이벤트 핸들러에서 picked 변수의 값을 바꾸어 보자.
```html
<script>
import { ref, reactive, toRefs } from 'vue' 

export default {
  setup(props, context) {
    const picked = ref("")
    const onClick = () => {
      picked.value = "Two"
    }

    return {
      picked,
      onClick
    }
  },
}
</script>
<template>
  <div>
      <input type="radio" id="one" value="One" v-model="picked" />
      <label for="one">One</label>
      <br />
      <input type="radio" id="two" value="Two" v-model="picked" />
      <label for="two">Two</label>
      <br />
      <span>Picked: {{ picked }}</span>
      <br>
      <input type="button" value="ClickMe" @click="onClick">
  </div>
</template>
<style>
</style>
```

버튼을 클릭하면 Two가 선택이 되고 값이 출력된 값이 변경된 것을 볼 수 있다. 

그런데, 다음과 같이 Checkbox의 value 속성의 값을 "1" 로 변경하면 checked되지 않는다. 
 ```html
 <input type="radio" id="two" value="1" v-model="picked" />
 ```

 라디오, 체크박스 및 셀렉트 옵션의 경우, v-model 바인딩 값은 보통 정적인 문자열(또는 체크 박스의 boolean)이다. 



## Select 
### option 요소에 value가 없는 경우

```html
<script>
import { ref, reactive, toRefs } from 'vue' 

export default {
  setup(props, context) {

    const selected = ref("")
    
    return {
      selected,
    }
  },
}
</script>
<template>
  <div>
        <select v-model="selected">
          <option disabled value="">Please select one</option>
          <option>A</option>
          <option>B</option>
          <option>C</option>
        </select>
        <span>Selected: {{ selected }}</span>
  </div>
</template>
<style>
</style>
```
A를 선택하면 'A'가 selected의 value 값이 된다. 

### option 요소에 value가 있는 경우 
v-bind 디렉티브를 사용하여 option 요소의 value 속서에 값을 설정하려면 다음과 같이 한다. 
```html
<script>
import { ref, reactive, toRefs } from 'vue' 

export default {
  setup(props, context) {

    const selected = ref("")
    const optVal1 = ref('1')
    const optVal2 = ref('2')
    const optVal3 = ref('3')
    
    return {
      selected,
      optVal1, 
      optVal2, 
      optVal3, 
    }
  },
}
</script>
<template>
  <div>
        <select v-model="selected">
          <option disabled value="">Please select one</option>
          <option v-bind:value="optVal1">A</option>
          <option :value="optVal2">B</option>
          <option :value="optVal3">C</option>
        </select>
        <span>Selected: {{ selected }}</span>
  </div>
</template>
<style>
</style>
```
이제 A를 선택하면 selected값은 1이 출력된다. 


### for-loop 사용 
```html
<script>
import { ref, reactive, toRefs } from 'vue' 

export default {
  setup(props, context) {

    const selected = ref("")
    const myoptions = ref([
      {key:"1", value:"사과"},
      {key:"2", value:"배"}
    ])
    return {
      selected,
      myoptions
    }
  },
}
</script>
<template>
  <div>
    <select v-model="selected">
        <option value="">선택하세요</option>
        <option :value="item.key"  v-for="item in myoptions">{{item.value}}</option> <br>
    </select> 
    {{selected}}
  </div>
</template>
<style>
</style>
```

### 다중 선택 
```html
<script>
import { ref, reactive, toRefs } from 'vue' 

export default {
  setup(props, context) {

    const selected = ref([])  // 배열로 선언
    const myoptions = ref([
      {key:"1", value:"사과"},
      {key:"2", value:"배"}
    ])
    

    return {
      selected,
      myoptions
    }
  },
}
</script>
<template>
  <div>
    <select multiple v-model="selected">
        <option disable value="">선택하세요</option>
        <option :value="item.key"  v-for="item in myoptions">{{item.value}}</option> <br>
    </select> 
    {{selected}}
  </div>
</template>
<style>
</style>
```

# File


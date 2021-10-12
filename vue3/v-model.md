# V-Model

v-model 디렉티브를 사용하여 input, textarea, select 요소들에 양방향 데이터 바인딩을 생성할 수 있습니다. v-model 디렉티브는 input type 요소를 변경하는 올바른 방법을 자동으로 선택한다. 약간 이상하지만, v-model은 기본적으로 사용자 입력 이벤트에 대한 데이터를 업데이트하는 “syntax sugar”이며, 일부 경우에는 특별한 주의를 해야한다.

> v-model은 모든 form 엘리먼트의 초기 value와 checked 그리고 selected 속성을 무시합니다. 항상 Vue 인스턴스 데이터를 원본 소스로 취급합니다. 컴포넌트의 data 옵션 안에 있는 JavaScript에서 초기값을 선언해야합니다.

v-model은 내부적으로 서로 다른 속성을 사용하고 서로 다른 입력 요소에 대해 서로 다른 이벤트를 전송한다.

* text 와 textarea 태그는 value속성과 input이벤트를 사용한다.
* 체크박스들과 라디오버튼들은 checked 속성과 change 이벤트를 사용한다.
* Select 태그는 value 를 prop으로, change를 이벤트로 사용한다.

## App.vue

```javascript
<template>
  <model-comp></model-comp>
</template>

<script setup>
import ModelComp from './components/ModelComp.vue'

</script>

<style>
</style>
```

## ModelComp.vue

```javascript
<template>
  <div>
    <h3>Text</h3>
    <input v-model="message" placeholder="여기를 수정해보세요" /><br />
    {{ message }} <br />
    <textarea v-model="message" cols="30" rows="10"></textarea>
    <h3>CheckBox</h3>
    <div id="v-model-multiple-checkboxes">
      <input type="checkbox" id="jack" value="Jack" v-model="checkedNames" />
      <label for="jack">Jack</label>
      <input type="checkbox" id="john" value="John" v-model="checkedNames" />
      <label for="john">John</label>
      <input type="checkbox" id="mike" value="Mike" v-model="checkedNames" />
      <label for="mike">Mike</label>
      <br />
      <span>Checked names: {{ checkedNames }}</span>
    </div>
    <div>
      <h3>Radio</h3>
      <div id="v-model-radiobutton">
        <input type="radio" id="one" value="One" v-model="picked" />
        <label for="one">One</label>
        <br />
        <input type="radio" id="two" value="Two" v-model="picked" />
        <label for="two">Two</label>
        <br />
        <span>Picked: {{ picked }}</span>
      </div>
    </div>
    <div>
      <h1>Select</h1>
      <div id="v-model-select" class="demo">
        <select v-model="selected">
          <option disabled value="">Please select one</option>
          <option>A</option>
          <option>B</option>
          <option>C</option>
        </select>
        <span>Selected: {{ selected }}</span>
      </div>
    </div>
    <div>
      <h1>Multi-Select</h1>
      <select v-model="multiSelected" multiple>
        <option>A</option>
        <option>B</option>
        <option>C</option>
      </select>
      <br />
      <span>Selected: {{ multiSelected }}</span>
    </div>
    <div>
      <h1>값 바인딩하기</h1>
      <div>라디오, 체크박스 및 셀렉트 옵션의 경우, v-model 바인딩 값은 보통 정적인 문자열(또는 체크 박스의 boolean) 입니다.</div>
      <!-- `picked` 는 선택시 문자열 "a" 입니다 -->
      <input type="radio" v-model="picked2" value="a" />선택<input type="button" @click="checkPicked2" value="Check Picked2"><br>
      <input type="radio" v-model="picked2" value="b" />선택 <br>
      <!-- `toggle` 는 true 또는 false 입니다 -->
      <input type="checkbox" v-model="toggle" />Toggle<input type="button" @click="checkToggle" value="Check Toggle"><br>
      <!-- `selected` 는 "ABC" 선택시 "abc" 입니다 -->
      <select v-model="selected2" @change="onSelectChange" >
        <option value="abc">ABC</option>
      </select>
    </div>
    <div>
      <h1>값바인딩하기(2)</h1>
      <input type="checkbox"  v-model="chkToggle" true-value="yes" false-value="no"> 값표시 <br>
      <input type="radio" v-model="pick3" v-bind:value="pickval1"> 값표시 
      <input type="radio" v-model="pick3" v-bind:value="pickval2"> 값표시
       <br>
      {{pick3}} <br>
      <select v-model="myoption">
        <option value="">선택하세요</option>
        <option :value="item.key"  v-for="item in myoptions">{{item.value}}</option> <br>
      </select> 
      {{myoption}}

    </div>
  </div>
</template>
<script>
import { defineComponent, ref , reactive } from "vue";

export default defineComponent({
  setup() {
    const message = ref("");
    const checkedNames = ref([]);
    const picked = ref("");
    const selected = ref("")
    const multiSelected = ref([]);
    

    // 값 바인딩 
    const picked2 = ref("");
    picked2.value = "a"
    // picked2 확인
    const checkPicked2 = () => {
      console.log(picked2.value)
    }
    const toggle = ref(false)
    const checkToggle = () => {
      console.log(toggle.value)
    }
    const selected2 = ref("")
    const onSelectChange = () => {
      console.log(selected2.value)
    }

    // 값바인딩 2 
    const chkToggle = ref("yes") 
    const pick3 = ref("")
    const pickval1 = ref("a")
    const pickval2 = ref("b")
    const myoption = ref("")
    const myoptions = ref([
      {key:"1", value:"사과"},
      {key:"2", value:"배"}
     ])


    return {
      message,
      checkedNames,
      picked,
      selected,
      multiSelected,
      picked2,
      checkPicked2,
      toggle,
      checkToggle,
      selected2,
      onSelectChange,
      chkToggle, 
      pick3,
      pickval1,
      pickval2,
      myoption, 
      myoptions

    };
  },
});
</script>
```

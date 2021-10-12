# Composition API

* Composition API 작업을 시작하려면 실제로 사용할 수 있는 장소가 필요하다. Vue 컴포넌트에서 이 위치는 setup이라고 한다.
* setup 컴포넌트 옵션은 컴포넌트가 생성되기 전에, props가 한 번 resolved 될 때 실행된다.
* Composition API의 진입점 역할을 한다.

> setup이 실행될 때, 컴포넌트 인스턴스가 아직 생성되지않아 setup옵션 내에 this가 존재하지 않습니다. 즉, props를 제외하고, 컴포넌트 내 다른 속성에 접근할 수 없습니다

* setup 옵션은 나중에 언급할 props 와 context에 접근하는 펑션이어야한다.
* 또한, setup에서 반환된 모든 것은 컴포넌트의 템플릿뿐만 아니라 나머지 컴포넌트 (computed properties, methods, 라이프사이클 훅 등)에 노출된다.

```javascript
// src/components/UserRepositories.vue

export default {
  components: { RepositoriesFilters, RepositoriesSortBy, RepositoriesList },
  props: {
    user: { type: String }
  },
  setup(props) {
    console.log(props) // { user: '' }

    return {} // 여기서 반환된 내용은 컴포넌트의 "rest"에서 사용할 수 있습니다
  }
  // 컴포넌트의 "rest" 부분
}
```

setup() 함수가 언제 실행되는지 이해하려면 Lifecycle hooks을 알아야 한다. Lifecycle hooksㄴ은 컴포넌트가 언제 생성되고, DOM에 추가되는지, 업데이트 되는지, 소멸되는지를 알려준다.

beforeCreate

컴포넌트 초기화 시에 실행된다. data는 아직 반응적으로 만들어지지 않았고, events는 아직 설정되지 않았다.

created

반응형 데이터에 접근할 수 있고 events에 접근할 수 있다. template와 DOM은 아직 mounted 되지 않았다.

beforeMount\
render functions들이 컴파일된 이후에 초기 render가 실행되기 전에 발생한다.

mounted

이 훅에서 반응형 컴포넌트, templates, 렌더링 된 DOM에 접근할 수 있다.

setup() 함수는 beforeCreate, created 라이프 사이클 훅 사이에 실행된다. beforeCreate가 setup 직전에 호출되고 created가 setup 직후에 호출된다. 그러니까 beforeCreate와 created 훅에서 작성되어야 하는 코드는 setup에서 작성되어야 한다.

> setup() 함수 내부에서 beforeCreate() created() 훅은 사용하지 않는다.

setup() 함수는 컴포넌트에 의해 훅이 호출될 때 실행될 콜백을 받는다. setup()함수 내에서는 혹이름에 on 접두사를 붙인다.

```javascript
export default { 
  setup() {
    onMounted( () => { 

    })
  }
}
```

## Composition API

Vue.js 3에 도입되는 컴포넌트 로직을 유연하게 구성할 수 있는 API 모음이다. 기존의 Options API를 사용하여 여러 혼재된 논리 구조를 분리하고 재사용 가능하게 하는것이 목적이다.

하나의 싱글 파일 컴포넌트(.vue 파일)에 로직을 구현하다보면 이상하게 가독성도 떨어지고 복잡해지는 문제가 발생했다. 아래와 같은 상황을 예로 들 수 있다.

```javascript
<template>
  <div>
    <h1>{{countWithUnit}}</h1>
    <button @click="increase">Increase</button>
    <button @click="decrease">Decrease</button>  
  </div>
</template>
<script>
  export default {
    data() {
      return {
        count: 0
      }
    },
    computed: {
      countWithUnit(){
        return this.count + '입니다'
      }
    },
    methods:{
      increase() {
        return ++this.count
      },
      decrease() {
        return --this.count    
     }
    },
  }
</script>
```

위의 코드는 간단한 카운터 컴포넌트임을 짐작할 수 있다. 하지만 모두 같은 기능을 위한 코드임에도 불구하고 이곳저곳(data, computed, methods)에 흩어져 있는 모습을 볼 수 있다. 이러한 문제를 해결하기 위해 탄생한 것이 Composition API이다.

### 변수할당에 대한 기초 지식

더 진도를 나가기 전에 두 개의 함수를 가진 객체를 반환하고 객체의 함수들을 변수에 설정하는 코드를 먼저 이해해 보자.

아래 코드에서 userCount는 두개의 함수를 포함하는 객체를 반환한다.

```javascript
let userCount = () => {
  let increase = () => {
    console.log('increase')
  }
  let decrease = () => {
    console.log('decrease')
  }
  return {increase, decrease}
}

let { increase, decrease } = userCount()
console.log(increase)
increase()
decrease()
```

let { increase, decrease } = userCount() 코드에서는 userCount 함수를 실행하고 함수가 반환하는 객체의 내부 함수들을 각 increase, decrease 변수에 할당한다. 그러면 increase(), decrease()와 같이 사용할 수 있다.

### Composition API 사용한 예

위의 예제를 Composition API를 사용하여 다음과 같이 변경할 수 있다.

```javascript
<template>
</template>
<script>

  const useCount = () => {
    const count = ref(0)
    const countWithUnit = computed( () => count.value + '입니다') )
    const increase = () => ++count.value 
    const decrease = () => --count.value
    return { countWithUnit, increase, decrease }
  }
  export default = {
    const { countWithUnit, increase, decrease } = useCount()
    return {
      countWithUnit,
      increase,  
      decrease
    }
  }
</script>
```

다소 복잡해 보일수도 있지만 카운터 기능에 대한 값과 로직이 useCount 함수 내에 모두 모여있는 모습을 볼 수 있다. 얼핏 보면 리액트의 Hooks와 매우 유사한 느낌을 받을 수 있다.

또한 setup 훅이 추가된 것을 볼 수 있는데 컴포지션 API를 사용하기 위한 진입점 즉, 초기화 지점이다. 기존의 생명주기와 비교해보자면, beforeCreate 이전에 setup이 호출된다.

### 반응형 데이터

Vue 2에서의 변경 가능한(Mutable) 반응형 데이터는 아래와 같이 data에 존재하는 값들이 이에 해당했다.

```javascript
<template>
  <div>
    {{message}}
  </div>
</template>
<script>
  export default { 
    data() {
      return {
        message: 'hello'  
      }  
    }
  }
</script>
```

여기서 message는 반응형이며 변경 가능한 데이터이다. computed의 경우 반응형이지만, 읽기만 가능하다. 반응형 데이터는 값이 변경됨에 따라 이를 감지하고 해당 값에 종속된 작업이 수행된다. 예를 들면 위의 message 값이 변경되면, DOM에 존재하는 div의 내부 텍스트가 알아서 변경되는 경우를 의미한다.

컴포지션 API에서는 두 가지 유형(reactive, ref)의 변경 가능한 반응형 데이터를 만들 수 있다.

```javascript
// 1.reactive
const reactiveValue = reactive( {name: 'tom'  } )
reactiveValue.name // tom 

// 2. ref
const refValue = ref(10)
refValue.value // 10 
```

컴포지션 API의 reactive를 통해 생성할 수 있으며 오직 객체만 받는다. 값에 접근하는 방법은 원본 객체에 접근하는 것과 동일하며 reactiveValue.name을 통해 tom이라는 값을 받을 수 있다.

또한 reactive를 통해 생성된 객체는 모두 깊은(Deep) 감지를 수행하기 때문에 객체가 중첩된 상황에서도 반응형 데이터를 쉽게 조작하고 처리할 수 있다.

ref를 알아보기 전에 하나 짚어보자면, reactive로 생성한 값은 프록시 객체라고 했지만 ES6에 추가된 프록시와는 다르다. ES6의 프록시 객체는 원본 데이터와 다른 별도의 프록시 객체를 생성하지만 reactive로 생성한 프록시 객체는 원본 객체와 완전히 동일하다.(원본 객체 내부에 Vue 옵저버만 추가하여 그대로 반환하기 때문)

두번째는 ref 값이다. 이 또한 reactive와 동일하게 컴포지션 API의 ref를 통해 객체를 생성할 수 있으며 reactive는 객체를 받는데 반해 ref는 모든 원시타입(Primitive) 값을 포함한 여러가지 타입의 값을 받을 수 있다. 원본 값은 ref 객체의 value 속성을 통해 접근할 수 있으며 값을 변경할 때에도 value 속성에 접근하여 조작해야 한다.

여기서 한 가지 의아한 점이 있다. reactive는 객체만 받아 값을 생성할 수 있지만, ref는 원시타입 모두(객체 포함)에 해당하는 값을 생성할 수 있다고 했다.

그럼 reactive는 필요 없는 것 아닌가요,라고 생각할 수도 있지만 공식 문서상에 ref의 값으로 객체가 전달될 경우 reactive 메소드를 통해 깊은(Deep) 감지를 수행한다고 한다. reactive와 ref값을 템플릿에 표시하는 방법은 기존의 버전 2와 동일하다.

```javascript
<template>
  <div>
     <h1>{{ first.name }}</h1>
	 <h1>{{ second }}</h1>
  </div>
</template>
<script>

const useValue = () => { 
  const reactiveValue = reactive( { name: 'tom' }) // 객체는 reactive() 사용
  const refValue = ref(10)  // 기본타입은 ref() 사용
}

export default { 
  setup() {
    const { reactiveValue, refValue } = useValues()
	return { 
	   first: reactiveValue, 
	   second: refValue 
	}
  }

}
</script>
```

단순히 콧수염 태그에 값을 넣어주기만 하면 된다.

ref 값을 직접 참조할 땐 value 속성으로 접근했었지만, 템플릿에 사용하는 경우에는 value 속성 접근없이 ref 값 자체를 사용한다. 또한 setup 훅에서 값을 반환할 때 속성명을 변경하여 사용할 수 있다.

ref 객체는 원본 값을 value라는 속성에 담아두고 변경을 감시하는 객체이며, reactive는 원본 객체 자체에 변경을 감지하는 옵저버를 추가하여 그대로 반환한 값이다. 값의 특성에 따라 적절히 선택하여 사용하면 될 듯 하다.

### isRef, toRefs

컴포지션 API에는 단순한 반응형 데이터 뿐만 아니라 어떤 유형의 값인지 확인하기 위한 isRef, reactive 값을 ref 값으로 변환하는 toRefs가 존재한다. isRef는 아래와 같이 ref 값과 reactive 값에 따라 다르게 처리해야 할 경우 사용한다.

```javascript
const reactiveValue = reactive( {name: 'tom' })
const refValue = ref(10)

const printvalue = obj => {
 console.log(isRef(obj)? obj.value: obj) 
}
```

ref 값의 경우 value 속성을 통해 데이터에 접근하므로 ref인 경우 obj.value를 출력하고, ref가 아닌 경우(reactive) obj를 그대로 출력하도록 구현되었다.

toRefs는 아래와 같은 상황에서 사용한다.

```javascript
const useMousePosition = () => { 
  const pos reactive( { x:0, y: 0} )
  return pos 
}

export default { 

  setup() {
    const { x, y} = useMousePosition()
	return { 
	   x, 
	   y
	}
  }
}
```

좌표와 같은 묶여있는 데이터의 경우 ref 값을 개별로 생성하기보단, 하나의 reactive 객체 값을 생성하여 사용한다. 위의 경우엔 x, y 속성을 가지고 있는 객체를 reactive 값으로 생성하고 setup에서 불러온다.

하지만 useMousePosition의 **reactive 객체를 구조 분해하여 재할당 할 경우 반응형으로 동작하지 않는다**. 위의 경우 setup에서 x, y로 분해하는 과정을 의미한다.

기존 객체를 분해하여 각각 할당하기 때문에 반응형으로 동작하지 않는 것은 당연한 일입니다. 이러한 문제를 해결하기 위해 toRefs가 존재한다.

```javascript
const useMousePosition = () => { 
  const pos reactive( { x:0, y: 0} )
  return return toRefs(pos) // Convert to ref 
}


export default { 

  setup() {
    const { x, y} = useMousePosition()
	return { 
	   x, 
	   y
	}
  }
}
```

위와 같이 reactive를 toRefs를 통해 ref 값으로 변환하면 구조 분해 할당을 수행해도 반응형이 그대로 유지된다. x, y를 지닌 객체를 x를 참조하는 ref하나, y를 참조하는 ref하나 이렇게 총 2개의 ref로 변환할 뿐이다.

### 계산된 속성(computed)

계산된 속성(computed) 또한 컴포지션 API를 통해 간단히 사용할 수 있다.

```javascript

<template>
  <div>
    <h1>{{ value2x }}</h1>
  </div>
</template>
<script>
export default { 
  setup(){
    const myValue = ref(10)
    const value2x = computed( () => myValue * 2)
    return {
      value2x
    }
  }
}
</script>
```

computed는 getter 함수가 반환하는 값에 대해 변경 불가능한 반응형 데이터를 반환한다. getter 함수는 위 코드에서 computed의 인자로 전달되는 () => myValue \* 2 에 해당된다. value2x는 myValue에 2를 곱한 값을 제공한다.

### 변경감시(watch)

변경 감시(watch)도 컴포지션 API를 통해 간단히 사용할 수 있으며,Vue 2 버전의 옵션과 동일한 옵션을 사용할 수 있다.(deep, immediate)

```javascript

<script>

const useUserLit = () => { 
  const userList = ref([
       { id: 1, name: 'tom' },
       { id: 2, name: 'james' },	   
       { id: 3, name: 'sally' },	   
    ])
	
	watch(userList, (newVal, oldVal) => {
	   // do stuff when userList was changed 
	}, { deep: true })
	
	return { userList } 
}

export default { 
   setup() {
      const { userList } = useUserList() 
	  
	  setInterval( () =>  {
	      userList.value[0].name += '!'
	  })
	  
   }
}
</script>
```

해당 코드는 유저 리스트에 3명이 존재하고, 그 중 첫 번째 항목의 name값 뒤에 3초마다 느낌표를 추가하는 코드이다. watch를 통해 반응형 값의 상태 변화를 쉽게 감지할 수 있다. watch는 첫 번째 인자의 getter가 반환하는 반응형 값 혹은 ref값이 변경될 경우 두 번째 인자로 전달된 콜백을 실행한다.

위의 userList는 배열 안에 유저 객체가 포함되어 객체가 중첩된 데이터 구조를 갖는데 이런 데이터의 경우 위의 코드처럼 watch의 세 번째 인자로 deep 옵션을 활성화한 옵션을 전달하여 내부 값의 변경도 감지할 수 있다. deep 외에도 immediate 옵션이 존재한다

### Life Cycle Hooks

Vue 2 버전에서는 생명 주기 훅을 아래와 같이 사용했다.

```

export default { 
  mounted() { 
  },
  updated() { 
  },
  destroyed() {
  }
}
```

이 또한 컴포지션 API를 사용하여 바꿀 수 있다.

```



export default {
  setup() {
    onMounted ( () => {
	})
	
	onUpdated(() => {
	})
	
	onUnmounted(() => {
	  // unmounted(destryoyed) 
	})
  }
} 
```

생명 주기 훅에 해당하는 메소드의 인자로 해당 주기 때 호출될 콜백 함수를 전달하여 사용한다. 기존의 생명 주기 훅 이름 앞에 on 수식어가 붙은 형식의 메소드로 구성되어 있어 익숙하신 분들은 금방 사용하실 수 있을 것이다. 다만 생명 주기 훅 이름이 변경된 경우가 있으며 아래 목록을 참고한다.

* beforeCreate -> setup
* created -> setup
* beforeMount -> onBeforeMount
* mounted -> onMounted
* beforeUpdate -> onBeforeUpdate
* updated -> onUpdated
* beforeDestroy -> onBeforeUnmount
* destroyed -> onUnmounted
* errorCaptured -> onErrorCaptured

또한 기존에는 컴포넌트당 하나의 생명 주기 훅을 사용했지만, 컴포지션 API를 사용할 경우 아래와 같이 여러개로 나누어 사용할 수 있다.

```javascript

const something1 = () => { 
  onMounted( () => {
      // somthing 1
  })
}

const something2 = () => {
  onMounted(() => {
      // something2
  
  })
}

const something3 = () => {
  onMounted(() => {
      // something3
  
  })
}

export default { 
  setup() {
    something1()
	something2()
	something3() 
  }
}
```

하나의 mounted 훅 안에 모두 들어갔어야 할 코드들을 각 기능(something 1 \~ 3)별로 나누어 코드를 작성할 수 있다.

### References

[Composition API 살펴보기](https://geundung.dev/102)\
[Composition API 공식사이트](https://v3.vuejs.org/guide/composition-api-introduction.html)

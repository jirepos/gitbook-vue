# Vue3 Web Tutorial2

다음과 같이 HTML을 작성한다.

```html
<div id="counter">
  Counter: {{ counter }}
</div>
```

Javascript을 로드하고 createApp()로 Vue App을 생성한다.

```html
<script src="https://unpkg.com/vue@next"></script>
<script>
const CounterApp = {
  data() {
    return {
      counter: 0
    }
  },
  mounted() {
    setInterval(() => {
      this.counter++
    }, 1000)
  }
}
Vue.createApp(CounterApp).mount('#counter')
```

아래는 실행결과이다.

Counter: {{ counter }}const CounterApp = { data() { return { counter: 0 } }, mounted() { setInterval(() => { this.counter++ }, 1000) } } Vue.createApp(CounterApp).mount('#counter') window.addEventListener('click', () => { })

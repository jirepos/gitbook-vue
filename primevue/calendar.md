# Calendar

자세한 사용방법은 이 [문서](https://primefaces.org/primevue/showcase/#/calendar)를 참고한다.

## import

```javascript
import Calendar from 'primevue/calendar';
```

## Template

```html
<Calendar v-model="value" :inline="false"   @date-select="onDateSelect" dateFormat="yy-mm-dd" />
```

## Events

### date-select

날짜가 선택되었을 때 이벤트가 발생한다. 파라미터로 Date 객체가 넘어온다.

```javascript
const onDateSelect = (event) => {
  console.log(typeof event);  // Object 
  console.log(event.getFullYear())
}
```

# Getting Started

primefaces.org의 문서를 참고해서 작성했다. 이 문서는 Vue 3와 PrimeVue 3를 기준으로 작성하였다.

## Setup

다운로드 하기

```shell
npm install primevue@^3.7.0 --save
npm install primeicons --save
```

Module Loader

core configuration을 설정할 뿐이고 어떤 컴포넌트도 아직 등록하지 않았다.

```javascript

import {createApp} from 'vue';
import App from './App.vue';
import PrimeVue from 'primevue/config';
const app = createApp(App);

app.use(PrimeVue);
```

Single File Components SFC에서 컴포넌트를 사용하려면 다음과 같이 import한다.

```javascript
import Dialog from 'primevue/dialog/sfc';
```

Styles css 의존성은 다음과 같다. Application component에서 다음을 import한다.

```javascript
primevue/resources/themes/saga-blue/theme.css       //theme
primevue/resources/primevue.min.css                 //core css
primeicons/primeicons.css                           //icons
```

Free Themes PrimeVue는 다양한 무료 themes를 지원한다. 다음 중에서 선택할 수 있다.

```javascript
primevue/resources/themes/bootstrap4-light-blue/theme.css
primevue/resources/themes/bootstrap4-light-purple/theme.css
primevue/resources/themes/bootstrap4-dark-blue/theme.css
primevue/resources/themes/bootstrap4-dark-purple/theme.css
primevue/resources/themes/md-light-indigo/theme.css
primevue/resources/themes/md-light-deeppurple/theme.css
primevue/resources/themes/md-dark-indigo/theme.css
primevue/resources/themes/md-dark-deeppurple/theme.css
primevue/resources/themes/mdc-light-indigo/theme.css
primevue/resources/themes/mdc-light-deeppurple/theme.css
primevue/resources/themes/mdc-dark-indigo/theme.css
primevue/resources/themes/mdc-dark-deeppurple/theme.css
primevue/resources/themes/tailwind/tailwind-light/theme.css
primevue/resources/themes/fluent-light/theme.css
primevue/resources/themes/saga-blue/theme.css
primevue/resources/themes/saga-green/theme.css
primevue/resources/themes/saga-orange/theme.css
primevue/resources/themes/saga-purple/theme.css
primevue/resources/themes/vela-blue/theme.css
primevue/resources/themes/vela-green/theme.css
primevue/resources/themes/vela-orange/theme.css
primevue/resources/themes/vela-purple/theme.css
primevue/resources/themes/arya-blue/theme.css
primevue/resources/themes/arya-green/theme.css
primevue/resources/themes/arya-orange/theme.css
primevue/resources/themes/arya-purple/theme.css
primevue/resources/themes/nova/theme.css
primevue/resources/themes/nova-alt/theme.css
primevue/resources/themes/nova-accent/theme.css
primevue/resources/themes/nova-vue/theme.css
primevue/resources/themes/luna-amber/theme.css
primevue/resources/themes/luna-blue/theme.css
primevue/resources/themes/luna-green/theme.css
primevue/resources/themes/luna-pink/theme.css
primevue/resources/themes/rhea/theme.css
```

PrimeFlex PrimeFlex는 CSS utility 라이르버리인데, grid system, flexbox, spacing, elevation 등등과 같은 다양한 helpers를 특징으로 한다. 필수적인지는 않지만, [PrimeFlex](https://primefaces.org/primevue/showcase/#/primeflex)를 추가하는 것은 매우 추천한다.

Riple Ripple는 버튼과 같은 지원되는 컴포넌트들을 위한 선택적 애니매이션이다. 기본적으로 disabled되어 있다. 활성화 하려면 entry file에서 설정한다.

```javascript
import {createApp} from 'vue';
import PrimeVue from 'primevue/config';
const app = createApp(App);

app.use(PrimeVue, {ripple: true});
```

Outlined vs Filled Input Styles

Input filed는 두가지 스타일이 있는데 default는 필드 주변에 보더가 있는 outlined이다. 반면에 filled는 필드에 background color를 추가한다. p-input-filled를 input의 부모에 적용하면 filled style을 가능하게 한다.

```javascript
import {createApp} from 'vue';
import PrimeVue from 'primevue/config';
const app = createApp(App);

app.use(PrimeVue, {inputStyle: 'filled'});
```

ZIndex Layering

ZIndex는 여러 컴포넌트들을 결합할 때 overlay 컴포넌트들의 레이어링이 원할하게 작동하도록 자동으로 관리된다.

```javascript
import {createApp} from 'vue';
import PrimeVue from 'primevue/config';
const app = createApp(App);

app.use(PrimeVue, {
    zIndex: {
        modal: 1100,        //dialog, sidebar
        overlay: 1000,      //dropdown, overlaypanel
        menu: 1000,         //overlay menus
        tooltip: 1100       //tooltip
    }
});
```

Locale PrimeVue provides a Locale API to support i18n and l7n, visit the [Locale](https://primefaces.org/primevue/showcase/#/locale) documentation for more information.

## References

[https://primefaces.org/primevue/showcase/#/setup](https://primefaces.org/primevue/showcase/#/setup)

# VUE ([참고자료](https://kr.vuejs.org/v2/guide/index.html))

## 1. 시작하기
### 1) 환경설정
> VS Code 확장 프로그램 `vetur` 설치

### 2) VUE-CLI를 통한 시작
```c
// 패키지 설치
$ npm i -g @vue/cli

// 프로젝트 시작하기
$ vue create [폴더명]
```

### 3) VUE 처음부터 시작 하기
#### ① 환경 구성
```c
// git에 저장해둔 webpack 활용
$ npx degit ferainK/webapck#eslint [폴더명]

//환경설정
$ npm i vue@next               //vue 문법 해석
$ npm i -D vue-loader@next     //webpack 활용
$ npm i -D vue-style-loader    //webpack 활용
$ npm i -D @vue/compiler-sfc   //vue → html/css/js 변환

$ npm i -D file-loader         //webpack 활용(특정한 파일을 읽어서 출력)

$ npm i -D eslint              //다른 vue 불러오는 확장 기능
$ npm i -D eslint-plugin-vue   //vue에서 eslint 사용하기 위함
$ npm i -D babel-eslint        //babel-eslint 연동

//유용한 기능
$ npm i -D shortid             //고유 ID 생성툴
```
#### ② 디렉토리 설정
```c
# js 폴더 삭제
# src 폴더 추가
# src/main.js 생성
# src/App.vue 생성
# src/components/HelloWorld.vue 생성
# src/assets 폴더 생성 (logo.png 파일 추가)
# .eslintrc.js 생성
```
#### ③ webpack.config.js 재설정
```js
//import 추가
const {VueLoaderPlugin>} = require('vue-loader')

//resolve 설정 추가 지정 (import 할 때, 확장자/경로 생략 가능하게 함)
resolve: {
  extensions: ['.js', '.vue'],
  alias: {
    '~': path.resolve(__dirname, 'src'),
    'assets': path.resolve(__dirname, 'src/assets')
  }
},

//entry 경로 변경 
entry: './src/main.js'

//module/rules 설정 object 추가/수정
{
  test: /\.vue$/, 
  use: 'vue-loader' 
},
{
  test: /\.s?css$/, 
  use: [
    'vue-style-loader', 
    '...'
  ]
},
{
  test: /\.(png|jpe?g|gif\webp)$/,
  use: 'file-loader'
},

// plugins 설정 생성자 추가
new VueLoaderPlugin()
```

#### ④ .eslintrc.js 설정 (참고자료 : [vue](https://eslint.vuejs.org/rules/), [js](https://eslint.org/docs/rules/))
```js
module.exports = {
  env: {
    browser: true,
    node: true,
  },
  extends: [
    // vue (택 1)
    'plugin:vue/vue3-essential',              // Lv1) 가장 낮은
    'plugin:vue/vue3-strongly-recommended',   // Lv2) 중간 정도
    'plugin:vue/vue3-recommended',            // Lv3) 가장 엄격

    // js
    'eslint:recommended'
  ],
  parserOptions: {
    parser: 'babel-eslint'
  },
  //extends에 설정된 기본설정을 변경하고 싶을 때
  rules: {}
}
```

#### ⑤ main.js / index.html 기본 설정
```js
//main.js
import {createApp} from 'vue'
import App from './App.vue'   // resolve 설정했다면, .vue 생략 가능

createApp({App}).mount('#app')
```

```html
<!-- index.html -->
<body>
  <div id="app"></div>
</body>
```

#### ⑥ App.vue / HelloWorld.vue 작성 (예시)
> `<template>`는 HTML로 변환되지 않는다는 점 참고!
```vue
//App.vue
<template>
  <h1>{{ message }}</h1>
  <HelloWorld/>
</template>

<script>
import HelloWorld from '~/components/HelloWorld.vue'
export default {
  components: {
    HelloWorld
  },
  data() {
    return{
      message: 'Hello?'
    }    
  },
}
</script>

<style lang="scss">

</style>
```
```vue
//HelloWorld.vue
<template>
  <img src="~assets/logo.png" alt="Hello">
</template>
```

### 3. VUE 문법
#### 1) 인스턴스
```js
//인스턴스를 만들고 호출하는 방식임
const app = Vue.createApp({/* Vue 파일 경로 */})
app.mount(/* index.html 파일의 선택자 */)
```

### 2) 라이프사이클
![라이프사이클](./src/assets/lifecycle.png)
```vue
//라이프사이클 훅 : 초기화 -> HTML 변환(컴파일) -> 화면 출력 -> update 과정에서 특정 명령 실행 가능하게 만든 클래스
// 1) 초기화 과정
beforeCreate() {this.xxx}
created() {this.xxx}            //중요 (컴파일 직전)
// 2) vue-html 변환 (HTML 생성)
beforeMount() {this.xxx}
mounted() {this.xxx}            //중요 (화면출력 직전)
// 3) 업데이트중(re-rendered)
beforeupdete() {this.xxx}
updated() {this.xxx}
// 4) 화면종료
beforeunmounted() {this.xxx}
unmounted() {this.xxx}
```
### 3) 디렉티브
```html
<p [Directive]> xxx </p>
```
1\) `v-once` : 반복 실행 허용 안함 (한 번만 도작) <br/>
2\) `v-html` : 변수에 html 형태로 입력하면, html 문법에 따라 출력<br/>
(本 변수에 html 형태로 입력하면, 텍스트 그대로 출력됨) <br/>

3\) `v-bind` : ① 선택자 등의 CLASS/ID 지정 시, ② 속성(attr)의 key|value가 변수인 경우  [`(약어)` `v-bind:` → `:`]
```js
//기본식
v-bind: class = "xxx"

//약어
:class = "xxx"
```
> 동적 클래스 (true일때만 클래스명 지정)
> ```js
> :class = "{[class명]: [true/false], ... }"
> ```

4\) `v-on` : 이벤트 수신        [`(약어)` `v-on:` → `@`]
> 자세한 이벤트 핸들링은 12항 참고
```js
//기본식
v-on:click="[함수명]"

//약어
@click="[함수명]"
```
※ 속성(clinck, class, id 등)을 매개변수로 표현하는 것도 가능 <br/>
```js
//기본식
v-bind:id="app"

//약어
v-bind:[attr]="app" 
  //단, attr이 선언되어 있어야함  
  return{
    attr="id"
  }
```
5\) `v-model ` : input 반응성 기능
- .number / .string : input 요소 데이터타입(type of) 지정
- .trim : 빈공간 허용하지 않음 (빈공간 자동 제거, 렌더 안함)
### 4) 보간법
```html
<h1> 값: {{count}} </h1>
```
6\) `v-slot` : `App.vue`와 `Component.vue`를 서로 연결  [`(약어)` `v-slot` → `#`]
```html
<!-- App.vue -->
<h1 v-slot:icon>연동연동</h1>
<h1 v-slot:text>연결연결</h1>
```
```html
<!-- Component.vue -->
<slot name="icon">기본값</slot>
<slot name="text">기본값</slot>
```

### 5) 반복문 
> index 생략 가능하다. (괄호 불요)

HTML
```html
<li v-for="(fruit, index) in fruits"
      :key="fruit">
      {{ fruit }}</li>
```

JS
```js
export default {
  data() {
    return {
      fruits: ['사과', '배', '포도']}}}
```

> [한걸음더] 배열 데이터 사용하기 <br>
> HTML
> ```html
> <li v-for="fruit in fruits"
>       :key="fruit.id">
>       {{ fruit.name }}</li>
> ```
> 
> JS
> ```js
> export default {
>   data() {
>     return {
>       fruits: ['사과', '배', '포도']}},
>   computed:{
>     newFruits() {
>       return this.fruits.map((fruit, index) => {
>         return {
>           id: index,
>           name: fruit,            
>         }})}}}
> ```

> [한걸음더] 반복생성 시 독립된 id 부여하기 <br>
> JS
> ```js
> import shortid from 'shortid'
> id: shortid.generate()
> ```
### 6) 조건문 (조건부 렌더링)
> 주의사항! <br>
> `v-show`도 조건문과 유사하게 사용되는데, `v-show`는 CSS의 display값만 변환시켜주므로, 렌더링 유무에 차이가 있다. <br>
> 자주 update되는 요소(element)라면 `v-show`를 사용하는게 리소스 관리에 유리하다.
```html
  <template v-if="fruits.length > 0">
    <h1>fruit's list</h1>
  </template>
  <template v-if-else="fruits.length > 5">
    <h1>Too many fruits!!!!</h1>
  </template>
  <template v-else>
    <h1>Nothing</h1>
  </template>
  ```
### 7) data
> \- template에 return하기 위한 데이터 <br>
> \- 재선언 가능 (getter/setter)
```js
data() {
    return {
      count: 2
    }
  }
```

### 8) methods : 매소드(함수/funciton)
> 함수 호출 시, 매개변수가 없다면 괄호 생략 가능 <br>
> 단, 여러 함수를 동시 호출하는 경우 괄호 생략 불가능

<br>

### 9) computed (데이터 값/value)
> 1\. cashing 기능이 있어 연산 최소화하기 위해 사용됨. <br>
> 2\. 매개변수가 없다면, 재선언 불가함 (default: getter) <br>
> 3\. 재선언하기 위해서는, get()/ set() 함수 설정 필요

1\. script에서 조건 생성
```js
computed: {
    reverseFruits(){
      get() {
        return fruit.split('').reverse().join('')
      },
      set(value) {
        this.msg = value
      }
    }}
```

2\. template에서 computed(조건) 호출 방법
```html
<!-- HTML 에서 (get 사용) -->
<li v-for="fruit in reverseFruits"
  :key="fruit">
  {{ fruit }}
</li>
```
```js
// JS 에서 (set 사용)
add() {
  this.reverseFruits += '!!' }
```

### 10) watch
> 특정 데이터(computed 포함)에 대해 변경되는 경우 실행되는 함수
> `변경된 데이터`는 인수에 명시, `기존 데이터`는 this. 로 명시
```js
watch: {
  [데이터]('[new 데이터]']) {
    console.log('[old 데이터]', →, '[new 데이터]') }}
```

### 11) 리스트 렌더링
> ① 변이 매소드 <br>
`push()` : 가장 마지막에 데이터 추가<br>
`pop()` : 가장 마지막 데이터 반환<br>
`shift()` : 가장 앞 데이터 반환<br>
`unshift()` : 가장 앞에 데이터 추가<br>
`splice()` : 데이터 넣고/빼고/삭제 <br>
`sort()` : 배열 정렬<br>
`reverse()` : 배열 반전<br>

> ② 교체 매소드 (재렌더링 하지는 않는다.) <br>
`filter()` : 배열 추출<br>
`concat()` : 배열 연결<br>
`slice()` : 배열 자르기 <br>

### 12) 이벤트 핸들러 (매소드=함수)
HTML
```html
@click="handler(hi, $event)"  /* defalut는 '$event' */
```
JS
```js
methods: {
  handler(msg) {
    console.log(msg)    //'hi' 뿐만 아니라, 발생한 event에 대한 다양한 정보를 확인할 수 있다. (마우스 위치 등)
  }
}
```

> 이벤트 수식어 (chainning 가능)<br>
> `.prevent` : 명령 제한 (중단)<br>
> `.stop` : 이벤트 버블링 방지 (자식을 클릭했을 떄, 자식 클릭 이후 부모도 클릭되는 기본 구조 방지)<br>
> `.capture` : 이벤트 버블링 역순자식을 클릭했을 때, 부모 먼저 클릭되고 자식이 클릭<br>
> `.capture.stop` : 자식을 클릭해도 부모만 클릭되는 효과 <br>
> `.self` : 이벤트 버블링 대상에서 제외 (자식을 클릭하더라도 부모는 클릭되는 효과가 없고, 부모 본인을 클릭했을때만 이벤트 발생) <br>
> `.once` : 한 번의 명령만 이행 <br>
> `.passive` : HTML과 JS를 독립시켜 실행 (부하 저감, 대략 5배 이상 성능 향상 효과, 단, `.prevent`와 함께 사용하면 망해버림) <br>
> > `@keydown.[key.key....] = "handler"` : `key`+`key`+`...`가 입력되는 경우 "handler" 실행 <br>
> > `@input="handler"` : 기본적으로 대부분의 데이터는 단방향 바인딩(get  only)이지만, 이벤트 핸들링을 통해 양반향 바인딩(반응성, get/set)으로 만들 수 있다. <br>
> > ```js
> > methed: {
> >  handler(event) {
> >    this.msg = event.target.value}}
> > ```
> > > `[tip]` <br>
> > > * `@input="handler"`을 인라인 형태인 `@input="msg=$event.target.value"` 혹은 디렉티브인 `v-model="msg"`으로도 표현할 수 있다. <br>
> > > * (단, `v-model`을 통해 한글을 입력받을 경우 글자가 완성 되어야 input으로 입력됨)
> > > * `@input` 대신 `@change`가 더 많이 사용됨. `@change="msg"`는 `v-model.lazy="msg"`로 표현함

### 13) 컴포넌트
#### ① props (외부에서 받는 데이터)
Component.vue
```html
<!-- HTML -->
<div
  :class="{large: large}" <!--> {large로 단축 표현 가능 -->
  :style="{ backgroundcolor: color }"
  class="btn">
  <slot>기본값</slot>
</div>
```
```js
//Javascript
export default {
  props: {
    color:{
      type: String,
      default: 'gray',
    },
    large:{
      type: Boolean,
      default: false    }}}
```
App.vue
```html
<!-- HTML -->
<Mybtn :color="#333"> Okay </Mybtn>
```

#### ② 속성(attr) 상속
- 기본적으로는 `Component.vue`에는 `1개`의 `최상위 요소(root element)`만 선언 가능 <br>
- `Component.vue`에는 `여러 개`의 `최상위 요소(root element)`가 있는 경우, `$attrs`를 통해 직접 상속 가능 <br>
\- Component.vue
```html
<!-- HTML -->
<!-- #1: 모든 요소 상속 -->
<h1 v-bind="$attrs"></h1>

<!-- #2: 일부 요소 상속 -->
<h1 :class="$attrs.class" :style="$attrs.style"></h1>
```
- 상속을 거부하는 방법 (`App.vue`에서 `Component.vue`로 속성 상속 불가) <br>
\- Component.vue
```js
//Javascript
export default {
  inheritAttrs: false }
```
- 이벤트의 상속 (emits) : `Component.vue`의 `디렉티브`는 `v-on` 수식어 이고, `App.vue`의 `디렉티브`는 `사용자 정의`로 선언 <br>

\- Component.vue (`v-on 수식어`)
```html
<!-- HTML -->
<!--  더블클릭 => doubleClick이라는 이벤트와 event 반환 -->
<div @dblclick="$emit('doubleClick', $event)"> ABC </div>
```
```js
//Javascript
export default {
  //이벤트 명시적 선언
    emits: [
    'doubleClick' ]}
```
\- App.vue  (`사용자 정의`)
```html
<!-- HTML -->
<!-- 저장된 이벤트 실행 => "log" 실행 -->
<Mybtn @doubleClick="log"> Okay </Mybtn>
```
```js
//Javascript
export default {
  methods: {
    log(event) {
      console.log('clicked!!', event)  }}}
```

### 14) Provide / Inject (반응성 없음)
> 기본적으로 `import`는 `자식 컴퓨넌트`에 데이터 전달됨 <br>
> `provide - inject`를 사용하면 `하위 컴퓨넌트`에도 데이터 전달 가능
> 객체 데이터를 반환함 <br>
> 반응성 없음 (재렌더링 없음) : computed(() => {})으로 반응성 대체 가능

\- ComponentChild.vue (`v-on 수식어`)
```html
<!-- HTML -->
{{ msg }}
```
```js
//Javascript
export default {
  props:{
    msg:{
      type: String,
      default: ''   }}
```

\- ComponentParent.vue (`v-on 수식어`)
```html
<!-- HTML -->
<Child :msg="msg"/>
```
```js
//Javascript
import Child from '~/components/Child'
export default {
  components: {
    Child },
  props:{
    msg:{
      type: String,
      default: ''   }}
```

\- App.vue  (`사용자 정의`)
```html
<!-- HTML -->
<Parent :msg="message"/>
```
```js
//Javascript
export default {
  components: {
    Parent },
  data() {
    return{
      message: 'hello?' }}}
```

### 15) Refs (class/id 이외의 추가 간편 선택자!)
```html
<!-- HTML -->
<Hello ref="hello"/>
```
```js
//Javascript (import 생략)
export default {
  components: {
    Hello  },
  mounted() {
    console.log(this.$refs.hello.$el)
    //외부에서 가져왔기 때문에 HTML형태의 객체 데이터임.
    //따라서 $el을 하면 HTML 내용 원문이 출력됨
  }
```

## 4. API (setup 매소드)
### 1) setup 사용 유무 차이 (코드 최적화 용도로 사용됨)
```js
// 변경전
export default {
  data() {
    return {
      count: 0     }},
  method: {
    increase() {
      this.count += 1     }},
  computed: {
    doubleCount() {
      return conunt.value * 2
    }
  },
  watch: {
    count(newValue) {
      console.log(newValue)   }}}
  mounted() {
    console.log(this.count)
  }

// 변경후
import { ref, computed, watch, onMounted } from 'vue'   //computed도 들어감
export default {
  inheriAttrs: false,
  props: {
    type: String,
    defalut: 'gray'
  },
  setup(props, context) {                 //setup은 반응성이 없음 (ref로 반응성 부여)
    let count = ref(0)      //ref는 객체 데이터!
    function increase() {
      count.value += 1}
    const doubleCount = computed(() =>{
      return conunt.value * 2
    })
    watch(count.value, (newValue) => {
      console.log(newValue)
    }) 
    onMounted(()=>{
      console.log(count.value)
      console.log(props.color)
      console.log(context.attrs)
    })
    return {
      count,
      increase,
      doubleCount    }}}
```




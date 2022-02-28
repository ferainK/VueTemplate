# JS 백과사전
## 1. JS 개요
***
1. 모듈 가져오기/내보내기
- export default : 모듈에 함수 이름지정 불요, import하는 측에서 `함수 이름` 자유롭게 지정 (함수 한 개만 내보낼 수 있음)
```javascript
  import getType from './getType'
  import myData from './myData.json'
```
- named default : 모듈에 함수 이름지정 필요, import하는 곳에는 `{함수 이름}` 형태로 지정 (함수 여러 개 내보낼 수 있음)
```javascript
  import {getType, getRandom as random} from './getType'
```

2. 일치연산자 : 동등연산자(`==`)는 자동 형변환되어 사용 안함
```javascript
  A === B
  A !== B
```

3. 논리연산자
```javascript
  a && b
  a || b
```

4. 삼항연산자 : if 문을 한줄로 표현
```javascript
  c ? true : false
```

5. 전개연산자 
```javascript
  fruits = ['apple', 'banana', 'cherry']
  console.log(...fruits)            //배열이 아닌 문자데이터(string)로 나열하여 반환
  console.log(toObject(...fruits))  //전개연산자를 배열로도 사용 가능

  funtion toObject(a, b, ...c) {    //...c (rest parameter) : 나머지 매개변수들을 배열로 등록
    return{
      a: a,
      b: b,
      c: c,
    }
  }
```
- 참조 데이터(객체, 배열, 함수)의 얕은 복사방법 (다른 방법으로는 object.assign 사용)
```javascript
  const member = {...user}
```

6. 정규표현식
- 역할
   - 문자 검색 (serch)
   - 문자 대체 (replace)
   - 문자 추출 (extract)

- 테스트 사이트 :  <https://regexr.com/>

- 표현 방식
```javascript
생성자 방식
const regexp1 = new RegExp('정규식', '옵션')

리터럴 방식
const regexp1 = /정규식/옵션

```
> 옵션
> - \+ : 그 뒤의 모든 문자
> - (?=@) : 조건


> 매소드
> 매소드 | 문법 | 설명
> --|--|--
> test|`정규식.test(string)`|일치 여부 (Boolean)
> match|`문자열.match(정규식)`|일치하는 문자열을 배열로 반환 (원본을 바꾸진 않음)
> replace|`문자열.replace(정규식, 대체문자)`|일차하는 문자열을 대체하여 문자열로 반환
>

> 플래그
> 플래그|설명
> --|--
> g|일치하는 모든 문자 global
> i|대소문자 구분 X (ignore case)
> m|여러 줄로 구분 (multi line)


> 패턴(표현)
> 패턴|설명
>--|--
>^ab| 줄 시작에 있는 ab와 일치
>ab$| 줄 끝에 있는 ab와 일치
>.|임의의 한 __문자__ 와 일치 (띄어쓰기 포함)
>a &verbar; b | a 또는 b와 일치
>ab? | a는 있고 바로 뒤에 있는 b가 없을수도 b가 있을수도 
>a{3} | a 3개 연속 일치
>a{3,} | a 3개 이상 연속 일치
>a{3,5} | a 3개 이상 5개 이하 연속 일치
> \w|Word, 대소영문, 숫자, _ 에 일치
> \b|\w와 일치하지 않는 것 (일반적으로 바운더리)
> \d|숫자에 일치
> \s|공백에 일치
> [abc]| a or b or c
> [a-z]: a ~ z 
> [A-Z]: A ~ Z
> [0-9]: 0 ~ 9
> [가-힣]: 가 ~ 힣
> (?=)| 앞쪽 일치
> (?<=)| 뒤쪽 일치

7. 변수의 유효범위
  - const : 블럭 { } 레벨에서만 유효
  - let : 블럭 { } 레벨에서만 유효
  - var : 함수 레벨에서 유효 (너무 광범위함) 

> #### 쉬어가기 : JS를 통해 HTML에 리스트를 추가하고 Event도 추가해보기
> ```javascript
>  const ulEl = document.querySelector('ul')
>  for(let i = 0; i < 10; i += 1){
>    const li = document.createElement('li')
>    li.textContent = `list-${i + 1}`
>    if ((i + 1) % 2 == 0){
>      li.addEventListener('click', function(){
>        console.log(li.textContent)
>      })
>    }
>    ulEl.appendChild(li)
>  }
>```

## 2. JS 기초
***
1. 객체 (리터럴 방식 = 직접 입력 방식)
```javascript
const myname={
  firstName: 'CheolWoo',    //속성 (맴버)
  lastName: 'Lee',          //속성 (맴버)
  getFullName: function() { //메소드 (맴버)
    return `${this.firstName}${this.lastName}`
  }
}
console.log(myname.getFullName())
```
```javascript
//호출방법
myname.firstname
myname['firstname']
```
```javascript
//축약형 : 속성과 값이 일치하는 경우
const myname={
  firstName,    //firstName: 'firstName'
  lastName      //lastName: 'lastName'
  }
}
```
2. 클래스 (프로토타입 방식? = 생성자 입력 방식)
```javascript
class User {
  constructor(first, last) {
    this.firstName = first
    this.lastName = last
  }
  getFullName() {
    return `${this.firstName}${this.lastname}`
  }
}
const myname = new User('CheolWoo', 'Lee')
console.log(myname.getFullName())
```

2. 함수 (프로토타입 방식? = 생성자 입력 방식)
```javascript
function User(first, last){
  this.firstName = first
  this.lastName = last
}
User.prototype.getFullName = function() {
  return `${this.firstName}${this.lastName}`
}

const leename = new User('CheolWoo', 'Lee') 
    //leename: 인스턴스
console.log(leename.getFullName())
```
- 화살표 함수
```javascript
//일반식
(인수) => {
  return '실행내용'
}
//간편식(실행내용인 1줄로 표현 가능한 경우)
(인수) => 실행내용    //return {  } 생략 가능
(인수) => ({배열})      //return 값이 배열인 경우 ({ })으로 입력해야댐
인수 => 실행내용      // 인수/실행내용이 1개인 경우 괄호 생략 가능
```
- 함수 인수 (arguments를 이용해 동적 인수를 입력할 수 있다.)
```javascript
function sum() {
  return arguments[0] + arguments[1]
}
console.log(sum(7,3))
```

- 함수 상속/확장
```javascript
//함수1
class Vehicle{
  constructor(name, wheel){
    this.name = name
    this.wheel = wheel
  }
}

//함수2
class Car extends Bicycle{        //상속 선언
  constructor(name, wheel, license){
    super(name, wheel)            //상속 내용
    this.license = license
  }
}
```

- 콜백 함수
```javascript
function timeout(callback){
  setTimeout(() =>{
    console.log('Hello!')
    callback()                      //함수 호출
  }, 3000)
}
timeout(() => console.log('done!')) //콜백 함수 선언
```
- 타이머 함수
```javascript
setTimeout(함수, 시간 ms) : 일정 시간 후 함수 실행,
setInterval(함수, 시간 ms) : 시간 간격마다 함수 실행
clearTimeout('setTimeout') : Timeout 함수 종료
clearInerval('setInterval') : Interbal 함수 종료
```
4. this의 범위 (???)
- 일반 함수 : "호출 범위"에 따라 범위 결정
- 화살표 함수 : 선언된 함수에서 범위 결정

5. 구조 분해 할당 (배열, 객체)
```javascript
const {name: user, age, email = '??', address} 
= user {name: 'Lee', name: 'CheolWoo', email: '@'} 
```
  - address를 제외하고는 정상 반영
  - 기본값(??)은 업데이트
  - 속성 이름 변경됨 (name → user)

6. 얕은 복사 vs 깊은 복사 (참조형 데이터: 객체/배열/함수)
- 얕은 복사 : 참조형 데이터를 다른 메모리에 저장하기는 하지만, 

    참조형 데이터 안의 참조형 데이터들은 같은 메모리를 공유할 수 있음
    
    (전개연산자 or Object.assign 사용)
- 깊은 복사 : 참조형 데이터 안의 참조형 데이터까지 다른 메모리에 배정

    (lodash.cloneDeep(object) 사용)


## 3. JS 기본(자세한 내용은 MDN자료 참고)
※ prototype 매소드가 붙은 것들은 리터럴 방식에서도 사용 가능
***
1. String
```javascript
string.prototype.indexOf('searchValue', 'Optional: fromIndex')
//string에 searchValue가 몇 번째에 존재하는지? 없으면 -1 반환

string.prototype.slice('startIndex', 'endIndex')
//string을 startIndex부터 endIndex까지 추출

string.prototype.replace('target', 'replace')
//string객체에 있는 target을 replace으로 교체

string.prototype.match(/.+(?=@)/)
//string에서 "정규표현식" 에 따라 내용 추출 (배열로 추출됨)

string.prototype.trim()
//string에서 앞/뒤 공백 제거
```

2. Number
```javascript
number.prototype.toFixed(n)
//number에서 n의 소숫 자리수 만큼 표시 (string으로 전환)

typeof 'value'
//Data Type확인

parseInt('string')
//string을 추출하여 Integer으로 변경

parseFloat('string')
//string을 추출하여 Float으로 변경
```

3. Math
```javascript
math.abs('number')    //number의 절대값 반환

math.min('number')    //number의 최소값 반환

math.max('number')    //number의 최대값 반환

math.ceil('number')   //number의 올림 처리

math.floor('number')  //number의 내림 처리

math.round('number')  //number의 반올림 처리

math.random()         //Random Number반환 (0 ~ 0.99999999)
```

4. Array
```javascript
array.prototype.length
//Array의 element 수

array.prototype.concat('array')
//Array에 Array추가하여 보여주기 (array 변형 X)

array.prototype.forEach(function(element, index) {'명령'})
//Array의 각 element에 명령(callback)실행 (return 없음)

array.prototype.map(function(element, index) {return '명령'})
//Array의 각 element에 명령(callback)실행 (return 가능)

array.prototype.filter(function(element, index) {return '명령'})
//Array의 각 element가 판별함수에 만족하는 요소 반환 (배열)

array.prototype.findIndex(function(element, index) {return '명령'})
//Array의 각 element가 판별함수에 만족하는 첫번쨰 요소의 index반환

array.prototype.find(element => 'function')
//Array에 있는 요소중 판별함수를 만족하는 첫번째 요소 반환, 없으면 undefinded

array.prototype.includes(target)
//Array에 있는 요소중 target이 있는지 여부 (true, false)

array.prototype.push(element)
//Array에 가장 뒤에 Element추가 (Array 수정됨)

array.prototype.unshift(element)
// Array에 가장 앞에 Element추가 (Array 수정됨)

array.prototype.reverse(element)
// Array에 뒤집기 (Array 수정됨)

array.prototype.splice(startIndex, deleteNumber, Optional: replaceElement)
// Array에 Start Index부터 Delete Number개 만큼 지우고 Replace Element를 추가함 (Array 수정됨)
```

5. Object (객체/배열/함수의 경우 자체는 동등 연산자 적용이 어려움, 참조형 데이터)
```javascript
object.assign(target, sorce1, sorce2, ...)
//target object에 sorce object에 삽입 (target이 수정되고 반환됨) 
`데이터 손실 방지를 위해 얕은 복사를 위해 target은 {}으로 선언함`

object.keys(object)
//object의 속성이름들 출력
```

6. Lodash 모듈
```javascript
_.cloneDeep(object)
//Object의 깊은 복사 반환

_.uniqBy(ObjectArray, 속성)
//Object들의 배열을 속성기준으로 고유화

_.unionBy(ObjectArray1, ObjectsArray2, ..., 속성)
//Object들의 배열을 속성기준으로 고유화

_.find(ObjectArray, {속성 : 값})
//Object들의 배열을 {속성: 값 }기준으로 Object 반환

_.findIndex(ObjectsArray, {속성 : 값})
//Object들의 배열을 {속성: 값 }기준으로 Object Index 반환

_.remove(ObjectsArray, {속성 : 값})
//Object들의 배열을 {속성: 값 }기준으로 찾은 후 Object 삭제

```

7. JSON
```javascript
JSON.stringify(object)
//object 데이터를 JSON 형태로 변환 (object → string)

JSON.parse(JSON)
//JSON 데이터를 object 형태로 변환 (string → object)
```

8. localStorage: 영구 데이터_웹주소에 종속 (↔ sessionStorage: 임시 데이터_창 이동시 삭제되는 데이터)
```javascript
localStorage.setItem(key, string)
//localStorage에 string 형태로 저장

localStorage.removeItem(key)
//localStorage에서 데이터 제거

`나중에 lowdb 모듈을 사용해보자`
```

8. OMdb API (영화 검색 API?)
9. axios 모듈
10. parcel-bundler 모듈
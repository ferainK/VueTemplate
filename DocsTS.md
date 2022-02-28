# Tpyescript


## 시작하기 전에
```powershell
# 초기 설정
$npm init -y
$npm install typescript -D   # -g 로 로컬 전역 설치해도 무관
$npx tsc --init

# 사용 방법
$npc tsc                     # TS 디버그 (TS → JS)
$node xxxx.js                # JS 디버그
```

## 1. 문자열
```ts
let a = 'mark'        //string (자동)

let a: string | null         //string (명시)
```
- boolean
- number
```ts
a = 0x39ad1  //16진수
a = 0o12813  //8진수
a = 0b01010  //2진수
a = NaN
a = 1_000_000
```
- string
```ts
 `Hello, ${name}. Your age is ${age + 1}.`
```
- symbol : 고유하고 수정 불가능한 값 (접근 권한에 많이 사용됨)
```ts
const sym = Symbol();
const obj ={
  [sym]: 'value';
}

obj[sym];   //그냥 값이 아니라 Symbol타입을 넣어야지만, 값을 불러올 수 있겠지.
```
- null & undefined : string/number에 사용 불가 (예외 처리 가능하긴함)
```ts
//strictNullChecks 옵션 : 계산결과 string or number에 null/undefined가 계산된다면, 에러 문구 표시
```
- object
```ts
const a = {
  key: 'value'
  index: 1
}

//매개변수 제한 방법 (Interface or Type)
interface PersonInterface{
  name: string;
  age: number;
}

function user(a: PersonInterface): string{
  return `이름은 ${a.name}이고 나이는 ${a.age} 입니다.`
}

//이름(PersonInterface)이 다르더라도 구조(name, age)가 같다면, 서로 대입 가능하다.
```
- array
```ts
let list: (number)[] = [1, 2, 3];
let list: (number | string)[] = [1, 2, 3, "4"];
```
- tuple : 정해진 문자열, 정해진 lenth
```ts
let x: [string, number];
x = ['Lee', 39];
```
- any : 정말 문자열이 다양하게 입력되는게 아니라면, 가급적 사용하지 않는게... (unknown을 쓰자)
```ts
//noImplicitAny 옵션 : TS가 추론하여 any로 결정된 경우, 에러 문구 표시 
//                    (정말 any라면 사용자가 any라고 명기하게끔)

function leakingAny(obj: any){
  const a:number = obj.num; //a가 any로 정해지는 것을 방지함
  const b = a + 1;
  return b;
}
```
- unknown : any와는 다르게 타입을 한정한 상태에서만, 문자열 변경 가능
```ts
declare const maybe: unknown;

if (typeof maybe === "string"){
  const aString: string = maybe; //가능
  const aNumber: number = maybe; //불가능
}
```
- never : 잘못된 타입을 입력하는 것을 막고자 할때 사용
```ts
function error(message: string): never{ //리턴타입이 never
  throw newError(message);
}

function fail() {
  return error("failed");
}

declare const a: string | number;
if (typeof a !== 'string'){
  a;    //a는 자동으로 number로 지정됨 (a는 string or number인데, if 에서 string을 제외했으니)
}

```
- void : 리턴값이 없다.
```ts
// noImplicitReturns 옵션 : 함수의 리턴이 없으면 에러 발생 시킴
// strictFunctionTypes 옵션 : 잘못된 함수 할당 시 에러 발생 시킴 
```
- 타입 별칭 (type) 
```ts
//길게 입력해야할 것을 간략하게 작성
type StringOrNumber = string | number;    //유니온 타입
type PersonTuple = [string, number];
type EatType = (food: string) => void;

//interface : 목적이 명확한 경우
//type : 단순히 이름만 지정할 때 (유니온 타입)
```

## 2. 컴파일 설정
- 자동저장 : "CompileOnSave" : "true"
- tsconfig 파일 확장 : "extend" : "경로"
- ★ 컴파일 대상 설정 (file > exclude > include)
```json
  {
    "file" : {
      "..." : "...",
      "items": {
        "type" : "경로"
      }
    }
  }
```
- "typeRoots" : JS 모듈은 TS에서 바로 사용할 수 없음. 별도 Config 설치 필요 (컴파일 문구에 표출되니 참고해서 설치하자.)
- "target" : JS 버전 (ES3, ES6, ...)
- "rootDir" / "outDir" : 입/출력 대상 경로 지정
- "strict" : noImplicitAny, strictFunctionTypes 등 모두 ture로 설정

## 3. Interface
- 인터페이스 적용 예시
```ts
interface Person{
  name: string;
  age?: number; //필수 아님
  [key: string]: any; //key 또한 변경 가능하게 만들 수 있음
  hello(): void               //함수표현식1
  hello: function(): void     //함수표현식2
  readonly gender: string; //readonly: 최초 선언 후 변경 불가
}

function hello(member: Person): void{
  console.log(`안녕하세요 ${member.name} 입니다.`
}

const member1: Person ={
  name: 'Mark',
  syster: ['Sung', 'Chan']
}
```
- 인퍼페이스 적용 응용 (함수)
```ts
interface HelloPerson{
  (name: string, age?: number): void;
}

const HelloPerson: HelloPerson = function(name: string, age?: number){
  console.log(`안녕하세요 ${name}입니다.`);
}
```
- 인터페이스의 구조 복제
```ts
class User implements Person{
  name: string;
  constructor(user: string){ //class에 선언된게 없다면 constructor를 만들어줘야함?
    this.name = user
  }
  age?: number|undefined;   //필수가 아니르모 undefined가 자동으로 붙음 (아예 age 자체를 없애버려도됨)
  hello():void{
    console.log(`안녕하세요 ${member.name} 입니다.`
  }
}
```

- 인터페이스의 상속 
```ts
interface Person2 extends Person, Person1{
  city: string; //기존 name, age?는 지속 유지 (단, 다른걸로 덮어씌울수도 있음(Override))
}
```

- 인터페이스 중복
```ts
interface A{
  a: string;
}

interface A{
  b: string
}

let value: A{
  a: '두개다';
  b: '사용할수 있다.';
}
```

## 4. class (ES6부터 나옴)
> function을 보완하기 위해 탄생한 object 설계도. 생성자를 통해 선언됨
- class 입력 방법
```ts
class Person {
  //인수(매개변수) 설정 (프로퍼티)
  //#1
  name;
  constructor(name: string){
    this.name = name;
  }

  //#2 (간편해서 많이 쓰임)
  name;
  constructor(name: string) {}
  

  //선택적인 인수 설정 (생성자)
  company;
  constructor(company?: string){
    if (company === undefined){
      company = ''
    }else{
      this.company = company
    }
  }

  //(매소드)
  init(){

  }

  //생성자에 포함되지 않는 매소드
  //Class의 이름(생성자로 만든 이름 X)으로 호출해야됨 (Person.hello으로만 호출 가능)
  static hello(){

  }

  //외부에서 입력되는 값 설정 (입력 누락 시 undefined가 출력되어 문제가 발생할 수 있음, 권장하지 않음)
  age!: number;

  //초기 값 설정
  city: 'Seoul';

  //한번만 입력하고 수정 불가능한 값
  //#1
  readonly ID: string;
  //#2
  readonly ID;
  constructor(ID: stiring);

  //class 에는 asnyc - await 를 사용할 수 없다.
}
```

- 접근 제어자
  - public : class 외부에서 사용 가능
  - privte : class 외부에서는 사용 불가 (호출이 안 된다는 것)
  - protected : class 외부에서 사용 불가능 하지만, 상속은 가능함

- getter/setter
```ts
class Person{
  public constructor(public _name: string, private age: number){}

  get name(){
    return this._name;
  }

  set name(name:string){
    this._name = name;
  }
}
```

- 동적 입력 방법
```ts
class Student{
  //#1
  [key: string]: string;
  //#2
  [key: string]: 'male'|'female';
}

const a = new Student();
a.mark = 'male';
a.jade = 'female';
```

- single ton (struct 만들기)
```ts
//개략적인 코드이니 대략 어떤 내용인지 이해만 해봐
class Struct{
  private static instance: Struct | null = null;
  static getInstance() {
    if (Struct.instance === null){
      Struct.instance = new Struct;
    }
  }
  private constrcutor() {}
}

const a = Struct.getInstance();
```

- Abstract Class (상속을 위해 불완전하게 입력한 Class)
```ts
abstract class AbstractPerson{
  protected _name: string = 'Mark';
  absctract setName(name: string): void; //불완전 함수
}
class Person extends AbstractPerson{

}
```

## 5. Generic
> any의 단점: 데이터 타입이 any로 정해지기 때문에 string, number의 매소드를 사용할 수 없음)
- 사용방법
```ts
// <  > : Generic이라 부룸
// extends 를 통해 T의 데이터 형태를 제한할 수 있음 (string/number만 입력 가능)
function hello<T extends string | number>(message: T): T{
  return message;
}
//#1: 입력값(ex. Mark)에 제한이 생김
hello<string>('Mark');
//#2: 데이터 타입이 의도한것과 다른게 될 수 있음
hello('Mark');
```

- 응용 (extends 활용법)
```ts
interface IPersion{
  name: string;
  age: number;
}

function setProp<T, K extends keyof T>(obj: T, key: K, value: T[K]) {
  return obj[key];
}
```
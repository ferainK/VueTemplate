# SCSS
## 1. 적용방법
```
$ npm init -y             //node.JS 시작
$ npm i -D parcel-bundler //파셀 번들러 설치 (Webpack 사용해도됨)
$ npm i -D scss           //SCSS 설치
```

> parcel-bundler 설정 방법
> 1. "serve": "parcel index.html"
> 2. "bulid": "parcel build index.html"

### 2. 주요 기능
### 1) 선택자
> 기본 : 하위 선택자  </br>
>  \> : 자식 선택자 </br>
> \& : 일치 선택자, 가상 클래스/요소 선택자, 이름 연결(ex. hel {&lo} => hello) </br>
> \: {} : 네임스페이스의 각 속성 나열 (ex. Hel: {lo} => hel-lo)

```scss
.btn{
  //기본
  div{
    backgournd-color: black;
  };
  //자식 선택자
  > h1{
    color: red;
  };
  //일치 선택자 (.btn 이면서 .active ※ not 하위/자식, just 동등)
  &.active{
    color: royalblue;
  };
  //가상선택자, 이름 연결
  li{
    &:lastchild{
      margin-right: 0;
    };
    ul{
      &-1{};
      &-2{};
      &-3{};
    };
  };

  margin:{
    top: 10px;
    left: 20px;
  };
}
```

### 2) 변수 및 연산자
> - 변수 유효범위: 선언된 구역에서만 적용 (재선언도 가능)
>   - 선택자에 변수 지정 시, #{xxx}형태로 이력되어야함 </br>
> - 연산자 유의사항
>   - 단위가 동일해야함 (단, calc를 사용하면 가능함)
>   - 나누기 기호(/)만 사용하면 계산되지 않음, 나누기 하려면 괄호로 감싸야됨
```scss
$size: 100px;
$margin: 20px + 10px;

//재활용 (인수: 기본값) : 생성: $@mixin / 호출: @include
@mixin box($value: 50px, $color: red){
  height: $value;
  width: $value;
  background-color: $color
  display: flex;
  justify-content: center;
  align-items: center;
  @content  //지정된 변수 발고도 선언시 {}에 넣은 값도 추가 적용 가능 기능
}
.container{
  @include box{
    border: 20px solid black;
  };               //50px, red, border 적용
}

.container{
  @include box(100px, blue);  //100px, blue 적용
}

.container{
  @include box($color: blue);  //50px, blue 적용
}

```

### 3) 반복문 : @ 사용
> for : 유사한 규칙의 css를 만들때 사용 (img를 반복적으로 넣거나, :nth-child를 사용할때)
> each : list(=array)나 map(object)에 사용 
> ※ 클래스 명에 변수 선언 시 
```scss
@for $iter from 1 through 10{
  .box:nth-child(#{$iter}) {
    width: 100px * $iter
  }

@each $el in $list{
  .box{
    color: $el
  }
}

@each $key, $value in $map{
  .box-#{$key}{
    color: $value
  }
}
}
```

### 4) 함수 : @ 사용
> 
```scss
//사용자 함수
@function ratio($size, $ratio){
  @return $size * $ratio
}

//내장함수
mix($color, $color) : RPG 색 혼합
lighten($color, 밝기(%))
barken($color, 어두운정도(%))
saturate($color, 채도(+%p))
desaturate($color, 채도(-%p))
grayscale($color) : 그레이스케일
invert($color) : 색 반전
rgba($color, 투명도(0.n))


```

#### 5) 다른 SCSS파일 가져오기
```scss
@import "./sub1" "./sub2
```
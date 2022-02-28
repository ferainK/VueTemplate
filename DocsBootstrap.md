# Bootstrap
<https://getbootstrap.com/docs/5.1/getting-started/introduction/>
## 1. 설정 방법
### 1) 직접 적용
- HTML에 CSS, JS 형태를 연결해주어야함
    - CSS
        > `<link href="https://cdn.jsdelivr.net/npm/bootstrap@5.1.3/dist/css/bootstrap.min.css" rel="stylesheet" integrity="sha384-1BmE4kWBq78iYhFldvKuhfTAU6auU8tT94WrHftjDbrCEXSU1oBoqyl2QvZ6jIW3" crossorigin="anonymous">`
    - JS

        \# Bundle
        > `<script src="https://cdn.jsdelivr.net/npm/bootstrap@5.1.3/dist/js/bootstrap.bundle.min.js" integrity="sha384-ka7Sk0Gln4gmtz2MlQnikT1wXgYsOg+OMhuP+IlRH9sENBO0LRn5q+8nbTov4+1p" crossorigin="anonymous"></script>`

        \# Seperate
        >`<script src="https://cdn.jsdelivr.net/npm/@popperjs/core@2.10.2/dist/umd/popper.min.js" integrity="sha384-7+zCNj/IqJ95wo16oMtfsKbZ9ccEh31eOz1HGyDuCQ6wgnyJNSYdrPa03rtR1zdB" crossorigin="anonymous"></script>`
        `<script src="https://cdn.jsdelivr.net/npm/bootstrap@5.1.3/dist/js/bootstrap.min.js" integrity="sha384-QJHtvGhmr9XOIpI6YVutG+2QOK9T+ZnN4kzFN1RtK3zEFEIsxhlmWl5/YESvpZ13" crossorigin="anonymous"></script>`
- Pooper.js를 사용하는 경우, 'Seperate' 버전으로 적용하여야한다. (단, Pooper, Bootstrap-seperate 순으로 호출해야됨)

### 2) node.js 적용
-  설치방법 (의존성으로 설치)
    ```
    # npm i bootstrap
    ``` 
- 적용방법
    - CSS
        ```css
        @import "./node_modules/bootstrap/scss/bootstrap"
        ```
    - JS
        ```js
        improt bootstrap from 'bootstrap/dist/js/bootstrap'
        ```
### 3) 사용 방법
- DOCS 파일을 확인해보자
- Class 명으로 동작한다. (hover로 컨트롤 가능하도록 만들어주었다.)
- 입력양식(Form)도 있다.
- Msgbox(Modal)도 있다.
    - EventLisener 활용을 위해 DOCS 내에 Events를 확인하자.
- 툴팁도 있다. (단 바로 사용할 수는 없고,  JS를 직접 작업해야됨)
- 로딩애니메이션 (spinner)도 있음.

### 4) Costumize
- SCSS 형식으로, 기본적인 색상(primary, secondary 등) 직접 변경 가능 (DOCS 파일 참고)
    - import 할떄, 먼저 ① 변경할 .scss import() 하고 ② bootstrap.scss import 해야함
- bootstrap 전체를 가져오는게 아니라 필요한 부분만 가져올 수 있음 (Costumaize>Optimize 참고)
    - 단 필요한 기능을 불러오기 할때의 명령어가 다르기 때문에(bootstrap.xxxx → xxxx으로 변경) 다소 불편할 수 있음 (Optimize 내 Default Export참고)
    - 필요에 따라서는 별도 패키지(npm)을 설치해야할 수 있다.
    - CSS는 크게 성능차이를 발생시키지는 않음, 오히려 부분 import는 오류를 발생할 수 있음 (JS는 따로 불러오자)
    - JS를 사용하지 않는 기능이 많으니 꼭 DOSC 내용을 잘 살펴보자. (시간이 없다면 오른쪽에 "via JS"가 메뉴에 있는지 확인해보자)
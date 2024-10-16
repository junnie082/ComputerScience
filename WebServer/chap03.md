# Web Server 3

## Web Server Programming

#### 2024.10.17.(수)

## Chapter 03 웹 프로그래밍 기초

### 목차

1. HTML 기초
2. CSS 기초
3. 자바스크립트 기초
4. [실습 3-1] 회원 가입 폼 만들기
5. [실습 3-2] ToDo 리스트 앱 만들기

### 학습목표

- HTML, CSS, 자바스크립트의 기초 개념을 익힌다.
- HTML, CSS, 자바스크립트의 상호작용 원리를 익힌다.
- 실습을 통해 웹 페이지를 직접 구성하면서 실전 감각을 익힌다.

## Section 01 HTML 기초

### HTML 이란?

- HTML(HyperText Markup Language)

  - 모든 웹 콘텐츠는 HTML로 이루어져 있음
  - 웹 브라우저는 서버로부터 전달받은 HTML 문서의 구조를 해석해 화면을 구성함
  - 클라이언트인 웹 브라우저가 서버로부터 수신하는 데이터의 구조는 HTML임

- 하이퍼텍스트(HypterText)

  - 다른 정보와 연결된 텍스트를 의미하며 사용자가 관련 문서를 링크를 통해 이동하며 정보를 얻을 수 있음
  - 초기 월드 와이드 웹은 바로 이러한 정보의 연결에 중저믈 두고 설계됨

- 마크업 언어(Markup Language)
  - 텍스트에 의미를 부여하기 위해 문서에 주석을 다는 표현 시스템
  - 표현하고자 하는 정보가 있을 때 정보의 앞뒤에 태그(Tag) 표기를 달아 정보에 의미를 부여하는 형식
    - 예) <h1>안녕하세요?</h2>
  - XML(eXtensible Markup Language)
    - HTML 보다 범용적으로 사용할 수 있는 마크업 언어
    - HTML: 사용할 수 있는 태그의 종류가 정해져 있음
      XML: 자신만의 규격을 정의할 수 있음

### HTML의 기본 용어

- 태그(Tag)
  - 태그는 < >를 사용하여 나타냄
  - 태그는 일반적으로 시작과 끝을 표시하는 2개의 쌍으로 이루어져 있으며, 종료 태그 앞에는 /를 붙여야 함
  - 사용할 수 있는 태그는 표준으로 정해져 있으며 태그마다 역할이 다름

`<시작 태그 속성="값" 속성="값"...> 내용 </종료 태그>>`

- 태그의 특징

  - 모든 태그가 종료 태그를 가지는 것은 아님
  - 태그 이름은 대소문자를 구분하지 않음
  - 태그에 추가적인 정보 부여는 속성을 사용함

- 속성(Attribute)
  - 태그에 추가 정보를 제공하기 위해 사용함
  - 사용할 수 있는 속성 역시 정해져 있음
  - 디자인과 관련된 속성은 거의 사용하지 않고 뒤에서 배울 CSS를 사용함

`<img src="photo.jpg" alt="사진">`

- 태그 보디(Tag Body)
  - 시작 태그와 종료 태그 사이에 위치하는 내용(콘텐츠)을 의미함
  - 대체로 텍스트가 위치하며, 다른 태그를 태그 보디에 둘 수도 있음
    - 즉 태그 사이에 다른 태그를 위치시키는 것이 가능함
  - 단, 태그 사이에 부모-자식 관계가 정해져 있는 경우 규칙을 따라야 원하는 결과물을 보여줌

```html
<h2>Hello World</h2>

<ul>
  <li>아이템 1</li>
</ul>
```

- 시맨틱 태그(Semantic Tag)
  - HTML5에서 도입된 개념으로, 특별한 의미를 가지는 태그를 말함
  - 문서의 구조를 정의하는 데 주로 사용됨
  - 시맨틱 태그 그 자체로는 별다른 역할을 수행하지 않고 화면 디자인에도 영향을 미치지 않지만 코드의 가독성을 높이는 데 도움을 줌
    - 또한 부모-자식 관계 없이 다른 태그를 자유롭게 사용할 수 있음

`<header>, <footer>, <article>, <section>, <aside>, <nav>`

### HTML 문서의 구조

- <!DOCTYPE html>: HTML5 문서를 선언하는 구문으로 웹 브라우저에 문서가 HTML5로 작성됨을 알림

- <html> ... </html>: HTML 문서의 시작과 끝을 의미함

- <head> ... </head>: CSS, 자바스크립트, 메타 태그 등이 위치함
  - <title> 태그: 문서의 상단 제목을 표시
  - <meta> 태그: 문서의 정보를 설정하는 등도 포함

- <body> ... </body>: 문서 본문에 해당하는 부분으로 실제 화면에 나타나는 메인 부분

- 자동 생성된 HTML 기본 템플릿의 예

```html
<!DOCTYPE html>
<html>
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <meta http-equiv="X-UA-Compatible" content="ie=edge" />
    <title>Page Title</title>
  </head>
  <body>
    ...
  </body>
</html>
```

- <charset>: 캐릭터 세트로 정상적인 한글 처리를 위해서는 반드시 "UTF-8"로 설정

- <viewport>: PC, 모바일 등의 서로 다른 크기의 단말기에서 화면 최적화를 위한 설정

- <http-equiv>: HTTP 헤더 정보를 설정하는 속성

- <X-UA-Compatible>: 브라우저의 호환성 보기 설정
  - 'ie=edge': 항상 최신 렌더링 엔진을 사용함을 의미

### HTML의 기본 태그

- 제목 태그

  - 제목 태그는 <h1>~<h6>까지 있으며, 숫자가 작을수록 큰 글자로 출력됨
  - 단순히 텍스트의 크기를 지정하는 용도가 아니라 문서에서 제목으로 사용될 텍스트에 사용하는 태그를 의미함
    - 어떤 의미에서는 시맨틱 태그로도 볼 수 있다.
  - 즉 <h1>~<h6>을 제목에서의 상-하위 개념으로 이해하는 것이 좋음
  - 제목 태그가 중요한 이유는 SEO(Search Engine Optimization)임
    - SEO: 구글과 같은 검색 엔진에서 우리가 만든 HTML 문서의 내용이 잘 검색될 수 있도록 최적화 작업

- 문단 태그

  - <p> 태그: 문단(Paragraph)을 구분하기 위해 사용
  - HTML에서는 연속된 공백이나 줄 바꿈은 단순한 공백으로 처리하기 때문에 문단 구분을 할 때는 <p> 태그를, 줄을 바꿀 때는 <br> 태그를 이용함
    - 줄 바꿈뿐만 아니라 공백도 별도 처리가 필요함
    - 여러 공백을 표현하려면 '&npsp;'를 필요한 만큼 입력하거나 CSS로 여백 설정

- 목록 태그
  - 목록 태그는 최신 HTML 문서 작성법에서 매우 중요한 부분임
  - 대다수의 콘텐츠가 목록 형태로 정의될 정도로 많이 사용됨
    - 예) 신문기사 목록: 하나의 신문기사는 <div>로 묶이고 이 안에 제목, 작성일, 신문사, 기자 이름 등이 목록으로 들어가는 구조
  - <ul>: 순서가 없는 목록
  - <ol>: 순서가 있는 목록
    - <ol> 보다는 <ul>을 주로 사용함
  - <li>: 리스트 아이템

```html
<ul>
  <li>아이템 1</li>
  <li>아이템 2</li>
</ul>
```

### 블록 태그와 인라인 태그

- 블록 태그
  - 라인 전체를 사용하는 태그
  - 즉 다른 태그 요소와 같은 라인에 배치할 수 없음

```html
<h1>Hello</h1>
<h2>World</h2>
```

- 대표적인 블록 태그

```html
<p>,</p>
<div>
  ,
  <h
    >,
    <ul>
      ,
      <ol>
        ,
        <form></form>
      </ol></ul
  ></h>
</div>
```

- 인라인 태그

  - 다른 태그 요소와 같은 라인에서 나란히 배치될 수 있는 태그
  - 경우에 따라 블록/인라인 두 가지 형식을 혼합한 'inline-block' 속성을 사용하기도 함

- 대표적인 인라인 태그

`<span>, <img>, <a>`

## Section 02 CSS 기초

### CSS란?

- CSS

  - 글씨의 색상이나 크기, 이미지 크기나 배치 방법 등 웹 문서의 디자인 요소를 담당

- CSS의 장점

  - 웹 문서의 내용과 별개로 디자인만 바꾸거나, 디자인은 그대로 두고 웹 문서의 내용 변경이 용이함
  - 다양한 기기(PC, 스마트폰 등)에 맞게 탄력적으로 디자인이 바뀌도록 반응형 디자인 (Responsive Design)을 구현할 수 있음
  - 동일한 문서 구조이더라도 서로 다른 CSS 테마 적용이 가능함

- CSS의 동작 원리
  - CSS 구문은 선택자와 선언부로 구성됨
  - 선택자는 디자인을 적용하고자 하는 HTML 요소이므로 선택자 정의가 중요함
  - 선언부는 {} 블록을 사용하며, 다수의 속성을 포함함
  - 각 속성 정의는 '속성:값;' 형식이며 항상 세미콜론(;)으로 끝남

### 스타일 시트 작성과 실행

- CSS를 HTML에 적용하기 위한 방법

1. 인라인 스타일 시트: HTML 태그에 CSS 속성을 정의함
2. 내장 스타일 시트: HTML 문서의 <head> 부분에 CSS 정의 부분을 포함함

- 주의할 점) 현재 작성한 문서에만 적용됨

3. 외장 스타일 시트: 별도의 CSS 파일을 생성한 후 HTML 문서에 링크로 포함함

- 주의할 점) 하나의 파일로 여러 문서에 적용 가능함

* CSS의 중첩(Cascading) 적용 방식
  - CSS는 위에서 아래로 중첩되는 방식임
    - 외장 CSS에서 적용한 디자인 속성은 내장 스타일 시트에서 수정하거나 속성을 추가할 수 있음
    - 셀렉터의 중첩에 의해 발생하는 경우에도 동일하게 적용됨

### 셀렉터의 개념

- 셀렉터(Selector)

  - 선택자의 다른 명칭
  - HTML 문서에서 특정 부분을 선택하기 위한 구문을 의미함
  - 기본적인 선택자: 태그, 아이디, 클래스

- 태그 셀렉터
  - 태그는 HTML의 기본 구성요소로, 태그 이름으로 요소를 선택함
  - 태그는 중복 사용되기 때문에 특정 영역을 선택하기보다는 공통 디자인 속성을 정의하는 데 사용됨
  - 같은 디자인 속성을 적용할 여러 태그는 콤마(,)로 나열해 일괄 적용할 수 있음

```html
p { text-align: center; color: red; } h1, h2, h3, h4 { color: blue; }
```

- 아이디 셀렉터
  - 아이디(Id) 속성을 사용함
  - 문서에 존재하는 유일한 값으로 아이디를 지정하여 특정 요소를 가장 확실하게 선택할 수 있는 방법
  - 한 곳만 선택이 가능한 셀렉터로 보통 문서의 전반적인 구조에 해당하는 부분에 사용하고 서로 다른 문서 간에도 동일 규격을 따르는 경우 재활용이 가능하도록 설계
  - 아이디가 선택자로 올 때는 HTML에서 지정한 아이디 앞에 #을 붙여 정의

```CSS
/* css */
#id_name { color: blue; }

<!-- html -->
<div id="id_name">
...
</div>
```

- 클래스 셀렉터
  - 가장 대표적인 CSS 셀렉터
  - 클래스(Class) 이름으로 구분해 스타일을 만들어두고 HTML에서 클래스 속성을 적용해 원하는 디자인을 적용하는 방법
    - 즉 CSS 선언이 먼저이고 HTML에서 이를 사용하는 개념
  - 재활용이 용이하고 누구나 사용할 수 있도록 라이브러리 등을 만드는 데도 기본이 되는 방법임
  - HTML 요소의 클래스가 선택자로 올 때는 클래스 앞에 온점(.)을 붙여 정의함

```CSS
/* CSS */
.title { color: blue; }
p.title { color: red; }

<!-- html -->
<div class="title">

</div>
<h2 class="title">
<p class="title">
```

### 부트스트랩이란?

- 부트스트랩(Bootstrap)
  - 가장 대표적인 오픈소스 CSS 라이브러리
  - 일관된 디자인이 적용된 컴포넌트가 클래스 형태로 정의되어 있고, 원하는 디자인 적용을 위해서는 해당 클래스를 적절한 태그에서 사용하는 형식
  - 부트스트랩을 사용하기 위한 방법
    - 1. 전체 소스코드를 내려받아 원하는 부분을 수정해 프로젝트에서 사용하는 방법
    - 2. 단순히 CSS 링크만 이용하여 기본 설정 형태로 바로 사용하는 방법

> 여기서 잠깐! 부트스랩의 버전 - 부트스트랩은 5.0이 가장 최신 버전이지만, 그동안 사이트 구축에는 4.x 버전이 가장 많이 사용되었다. 5.0과 4.x의 가장 큰 차이점은 내부적으로 jQuery에 대한 의존성을 제거한 것으로 그 외 기능이나 사용성 측면에서는 차이가 없다. 다만 최근 vue.js, react.js 등을 많이 사용하고 jQuery는 점점 사용하지 않는 추세이기 때문에 가급적 5.0 버전을 사용하는 것을 추천한다.

- 부트스트랩을 활용한 버튼 만들기

1. 부트스트랩 홈페이지(https://getbootstrap.com)에서 버튼에 대한 설명을 보고 원하는 모양의 버튼 클래스 샘플을 사용하기도

2. 부트스트랩 속성은 중첩해서 사용할 수 있어 여러 속성을 다양한 형태로 바꾸기

- 배경은 파란색, 폭은 브라우저 크기의 75%
- div 박스와 상단 마진은 5(1~5 가능)
- 박스 테두리에 그림자 효과를 추가한 간단한 박스 영역 생성

`<div class="bg-primary w-75 mt-5 shadow">`

## Section 03 자바스크립트 기초

### 자바스크립트란?

- 자바스크립트(Javascript)

  - 정적인 HTML 콘텐츠에서 사용자와 상호작용하며 동적으로 변경하는 부분을 담당
  - 객체(Object) 기반의 스크립트 언어로 기본적으로는 웹 브라우저에서 해석되는 인터프리터 언어임
  - Node.js와 같은 프레임워크를 사용하면 서버 프로그래밍에도 사용할 수 있음

- 자바스크립트의 특징

  - 동적이며 타입을 명시할 필요가 없는 인터프리터 언어
  - 객체지향 프로그래밍과 함수형 프로그래밍을 모두 표현할 수 있음
  - HTML의 내용, 속성, 스타일을 변경할 수 있음
  - 이벤트를 처리하고 사용자와의 상호작용을 가능하게 함
  - 서버와 실시간 통신 기능을 제공

- 자바스크립트의 현황

  - 뷰(Vue), 앵귤러(Angular), 리액트(React)와 같은 프론트엔드 프레임워크에서부터 Node.js를 기반으로 하는 서버 프로그래밍 영역까지 자바스크립트가 널리 사용됨
  - 자바스크립트는 네이버, 구글과 같은 웹 서비서에서부터 데스크탑 응용 프로그램까지 사용되지 않는 곳이 없음

- 자바스크립트가 사용되는 기능
  - 최신 뉴스 제목, 뉴스, 환율 등이 자동으로 스크롤되는 화면
  - 쇼핑, 뉴스 스탠드 등에서 현재 화면은 유지하면서 해당 콘텐츠의 페이지 섹션의 페이지만 좌우로 변경되는 화면
  - 웹 페이지에 날씨 정보 출력(날씨 정보 사이트에 실시간 데이터 요청 후 출력)
  - 홈쇼핑 등에서 웹 브라우저를 통한 고객 상담 채팅
  - 클릭하면 숨겨진 메뉴가 나오거나 탭 형태의 화면 구성
  - 선택에 따라 일부 화면이 바뀌거나 움직이는 형태

### 자바스크립트의 기본 문법

- 자바스크립트가 지원하는 프로그램 구성요소로

  - 변수, 함수, 객체, 클래스
  - 반복문, 조건문
  - 배열, 리스트, 맵(Map) 등의 자료구조
  - 비동기 처리 지원
  - HTTP 요청 및 응답 처리
  - HTML 문서에 접근할 수 있는 내장 객체와 함수
    - 이를 통해 DOM(Document Object Model)을 다룰 수 있음
  - 객체를 정의하는 JSON(JavaScript Object Notation) 구문

- 자바스크립트의 문법적 특징
  - 변수 타입을 따로 지정하지 않으며, 선언은 var, let, const를 사용함
  - 범위 지정 없이 변수를 선언하면 전역변수가 되고 위치에 상관없이 호이스팅(끌어올림)되므로 주의해야 함
  - 문자열을 표현할 때는 큰따옴표("")와 작은따옴표('')를 모두 사용할 수 있음
  - 함수형 언어를 지원하며 함수는 변수, 함수 인자, 객체 멤버 등에 사용될 수 있음
  - <script> 태그는 HTML의 <head>와 <body> 영역 모두에서 사용 가능함
    - 하지만 DOM 접근을 위해서는 HTML 문서가 모두 로딩된 다음에 접근 가능
  - JSON 구조를 광범위하게 사용함

> 여기서 잠깐! 같은 변수명으로 변수를 선언할 때 var, let, const의 차이점

- var(함수범위)는 이미 만들어진 변수 이름으로 재선언이 가능하다.
- let(블록범위 변수)은 이미 만들어진 변수 이름으로 재선언은 불가능하지만, 재할당은 가능하다.
- const(블록범위 상수)는 이미 만들어진 변수 이름으로 재선언이 불가능하며 재할당 또한 불가능하다.

### 이벤트 처리와 DOM의 개념

- 이벤트 발생과 처리

  - HTML 문서에서 발생하는 특정 상황으로 이벤트 발생은 보통 자바스크립트 코드와 연계됨
    - 예) 버튼 클릭, 마우스 이동, 마우스 클릭, 키보드 입력, 문서 로드, 창 크기 변경 등

- 자바스크립트에서 이벤트를 처리하는 방법
  - 1. HTML 태그에 이벤트 처리 속성을 이용
  - 2. 문서 객체 모델 DOM 요소에 속성을 추가
  - 3. DOM 요소에 addEventListener()로 콜백 함수를 등록

```html
<button type="button" onClick="login()">로그인</button>
<div onMouseOver="show()">마우스가 올라가면 텍스트가 보입니다.</div>
```

- 문서 객체 모델(DOM)

  - 자바스크립트에서는 DOM을 통해 HTML 요소에 접근할 수 있음
  - DOM은 텍스트로 된 HTML 문서를 프로그램적으로 처리할 수 있도록 문서 구조 전체를 객체화한 것을 의미함

- DOM의 체계

  - 태그: 요소 노드(Element node)로 정의됨
  - 태그의 속성: 속성 노드(Attribute node)가 됨
  - 태그 보디의 텍스트는 다시 텍스트 노드(Text node)가 될 수 있음
  - 경우에 따라서는 innerHTML 속성으로 표현하는 것도 가능함
  - 이러한 DOM 체계에 따라 모든 HTML은 브라우저에 의해 로딩될 때 각각의 요소가 하나하나 '부모-자식' 관계를 가지며 [그림 3-7]과 같이 객체 트리 구조로 재구성됨

- 자바스크립트에서 DOM에 접근하는 방법
  - 특정 태그, 아이디를 비롯해 CSS 셀렉터를 통한 요소 선택이 가능함
  - querySelector(): 선택자와 일치하는 첫 번째 노드만 가지고 옴
  - querySelectorAll(): 선택자와 일치하는 모든 노드를 가지고 옴

```JavaScript
document.getElementByTagName("tag_name");
document.getElementByTagName("id_name");
document.getElementByClassName("class_name");
document.getElementsByName("name_attribute");
document.querySelector("p.title");
document.querySelectorAll("p.title");
```

- getAttribute(), setAttribute(): 가져온 요소의 속성에 접근

```JavaScript
element.setAttribute(attribute, value)
element.getAttribute(attribute, value)
```

### DOM을 활용한 동적 디자인 변경

- 스타일 속성 변경
  - 특정 태그 요소의 스타일 속성을 직접 변경하는 방식

`document.getElementById("box1").setAttribute("style", "background-color: yellow");`

- 스타일 객체 변경
  - 스타일 속성 변경과 동일한 방식이지만 코드 구현에서 차이가 있음
  - 태그의 속성을 문자열 형태로 지정하는 것이 아닌 객체 참조 방식으로 스타일 속성을 지정

`document.getElementById("box1").style.backgroundColor = "yellow";`

- 클래스 변경
  - 태그 노드의 스타일 속성이 아니라 클래스 변경을 통해 디자인을 변경하는 형식

`document.getElementById("box1").className = "bgyellow";`

## Section 04 [실습 3-1] 회원가입 폼 만들기

### 실습 3-1 프로젝트의 개요

- 이번 실습의 흐름

1. HTML 기본 문서 구조를 작성한다.
2. CSS를 통한 디자인을 정의한다.
3. 자바스크립트를 통하여 DOM에 접근하고 디자인을 변경한다.

### HTML 기본 문서 구조 작성

1. 이클립스를 실행한다. 화면 왼쪽의 프로젝트 탐색기를 보면 2장에서 생성한 'jwbook' 하단의 [src] -> [main] -> [webapp] 폴더에서 마우스 오른쪽 버튼을 클릭하고 [ch03] 폴더를 새로 생성하기

2. [ch03] 폴더에 'regform.html' 파일을 생성하기

3. 기본 생성된 템플릿의 8행의 <h2> 태그와 <form> 태그를 이용해 간단한 입력 양식을 만들기

```html
<!DOCTYPE html>
<html>
  <head>
    <meta charset="UTF-8" />
    <title>회원 가입 폼 만들기 예제</title>
  </head>
  <body>
    <h2>회원 가입</h2>
    <hr />
    <div id="regform">
      <form name="form1">
        <label>이름</label><br />
        <input type="text" name="name" size="40" /><br />
        <hr />
        <label>이메일</label><br />
        <input type="email" name="email" size="40" /><br />
        <button type="button" onClick="signUp()">가입</button>
      </form>
    </div>
  </body>
</html>
```

4. HTML 파일의 경우 테스트를 위해 톰캣 서버와 불필요하게 연동하지 않아도 됨. 'regform.html' 파일에서 마우스 오른쪽 버튼을 클릭하고 [Open With] -> [Web Browser]를 선택

5. CSS를 이용해 디자인을 정의하기 위해 <head> 내부에서 다음과 같이 스타일을 작성

```html
<!DOCTYPE html>
<html>
  <head>
    <meta charset="UTF-8" />
    <title>회원 가입 폼 만들기 예제</title>
    <style>
      h2 {
        border-radius: 5px;
        background-color: wheat;
        text-align: center;
        padding: 15px 0;
      }

      #regform {
        padding: 15px 20px;
        border-radius: 10px;
        margin: auto;
        width: 50%;
        background-color: SandyBrown;
      }
    </style>
  </head>
  <body>
    ~~생략([예제 3-2]의 내용)~~
  </body>
</html>
```

### 결과 출력 화면 작성

- 가입 버튼을 클릭하면 자바스크립트 함수를 호출해 입력값을 받아와 현재 입력 양식 화면을 지우고 새로운 박스에 내용을 출력하도록 함
- 이번 실습에서는 자바스크립트 활용을 동일 페이지에서 구현함

6. [예제 3-2]의 입력 양식 뒷부분인 20행에 다음 코드를 작성하기

```html
<div id="result" class="result">
  <h3>가입 정보</h3>
  <hr />
  이름: <span id="rname"></span><br />
  이메일: <span id="remail"></span><br />
</div>
```

7. 자바스크립트를 추가해 화면에 입력 양식 아래에 박스가 보이지 않도록 설정하기. <head> 부분에 작성할 경우 HTML 파일이 모두 로딩되지 않은 상태라 속성 지정이 불가능하므로 결과 화면 코드 이후에 스크립트가 와야 함.

- </body> 안쪽으로만 작성하면 됨

```JavaScript
<script>
  document.getElementById("result").style.display = "none";
</script>
```

8. 버튼을 클릭했을 때 호출될 signUp() 함수 구현하기 위해 [예제 3-3]의 22행에 다음 코드를 추가적인

```JavaScript
<script>
  function signUp() {
    alert("정말로 가입하시겠습니까?");
    document.getElementById("regform").style.display = "none";
    document.getElementById("rname").innerHTML = document.form1.name.value;
    document.getElementById("remail").innerHTML = document.form1.email.value;
    document.getElementById("result").setAttribute("style", "display: block; background-color: KhaKi;");
  }
</script>
```

10. 우선 가입 버튼을 누르면 '정말로 가입하시겠습니까?' 메세지가 'alert' 창을 통해 출력되고, <확인> 버튼을 클릭하면 양식이 사라지고 결과 박스가 보임

## Section 05 [실습 3-2] ToDo 리스트 앱 만들기

### [실습 3-2] 프로젝트의 개요

- 이번 실습의 흐름

1. 부트스트랩을 이용하여 디자인을 적용한다.
2. 자바스크립트로 이벤트를 연동한다.
3. DOM을 핸들링한다.

### HTML 기본 문서 구조 작성

1. [실습 3-1]에서 생성한 폴더 [ch03]에 'todoapp.html'을 생성하고 기본적인 화면을 구성하기.

- 화면은 할 일을 입력받기 위한 입력 양식과 버튼을 구성됨
- 이번 실습에서는 각각의 요소를 자바스크립트로 모두 처리할 것이기 때문에 코드를 간소화하기 위해 <form> 태그를 사용하지 않음

### 부트스트랩을 이용한 디자인 정의

2. 부트스트랩 적용을 위해 부트스트랩 홈페이지(https://getbootstrap.com/docs/5.0/getting-started/introduction)에서 CSS 스타일 시트 링크를 복사해 <head> 부분에 축하기.

- 이번 실습에서는 5.0 버전을 사용하며 링크 코드는 직접 입력하지 ㅇ낳고 최신 버전의 코드를 복사해 넣어야 함

* 메인 박스 디자인

3. 전체 화면 구성요소를 하나의 박스로 묶어 브라우저 안에서 별도 실행된 화면과 같은 느낌을 주기 위해 [예제 3-5]의 8행 <div> 부분에 부트스트랩 클래스를 정의

`<div class="container bg-warning shadow mx-auto mt-5 p-5 w-75">`

- 입력 양식 디자인

4. 입력 양식과 버튼에도 부트스트랩 클래스를 지정하기 위해 [예제 3-5]의 11~14행을 다음과 같이 수정

```html
<div class="input-group">
  <input
    id="item"
    class="form-control"
    type="text"
    placeholder="할일을 입력하세요.."
  />
  <button type="button" class="btn btn-primary" onClick="addItem()">
    할일 추가
  </button>
</div>
```

5. 화면에는 보이지 않지만 목록 출력을 위한 <ul> 부분에도 다음과 같이 클래스를 지정

`<ul id="todolist" class="list-group"></ul>`

- 항목 추가 기능

6. 할 일 추가 버튼을 클릭했을 때 호출된 addItem() 함수를 구현하기

- 먼저 함수를 <head> 부분에 정의하고 getElementById()를 이용해 프로그램에서 필요한 HTML 요소를 참조할 수 있도록 함

```html
<script>
  function addItem() {
    // 입력값을 읽어와 todo 변수에 저장
    let todo = document.getElementById("item");
    // <ul> 요소를 참조하여 list 변수에 저장
    let list = document.getElementById("todolist");
    // 새로운 <li> 요소를 생성하여 listitem 변수에 저장
    let listitem = document.createElement("li");
  }
</script>
```

7. 새로운 할 일 아이템에 디자인을 부여하기 위해 부트스트랩 클래스를 추가하고 <li> 요소의 보디 부분에 추가적인

```JavaScript
// 새로운 목록에 디자인 추가
listitem.className
  = "d-flex list-group-item list-group-item-action list-group-item-warning";
// 입력값을 <li> 태그 보디에 추가
listitem.innerText = todo.value;
```

8. <li>의 상위 요소인 <ul>, 즉 리스트에 새로운 <li> 노드를 추가해주면 화면에 추가된 목록이 보임

```JavaScript
// ul 요소에 li 요소 추가
list.appendChild(listitem);

// 입력칸 비우기 및 포커스 이동
todo.value = "";
todo.focus();
```

- 항목 삭제 기능

9. 먼저 각각의 아이템에 닫기 버튼을 추가하기 위해 [예제 3-7]의 11행 다음에 다음 코드를 추가하기

```JavaScript
// <li> 요소에 들어갈 닫기 버튼 생성
let xbtn = document.createElement("button");
// 버튼에 부트스트랩 클래스 적용
xbtn.className = "btn-close ms-auto";
// <li>에 버튼 추가
listitem.appendChild(xbtn);
```

10. 이어서 추가된 버튼이 눌렸을 때 이벤트 처리를 하도록 다음과 같이 버튼에 이벤트 핸들러를 추가하기

- onclick 속성에 콜백 함수를 추가하는 형식으로 구현
- 버튼의 부모 요소, 즉 <li>를 참조하여 리스트에서 삭제하는 형식

```JavaScript
// close 버튼에 이벤트 처리
xbtn.onclick = function(e) {
  // 이벤트가 발생한 <li> 요소를 구하여 <ul> 요소에서 제거
  let pnode = e.target.parentNode;
  list.removeChild(pnode);
}
```

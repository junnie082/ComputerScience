# Web Server 8

## Web Server Programming

#### 2024.11.22.(금)

## Chapter 08 MVC 패턴의 이해

### 목차

1. MVC 패턴
2. 서블릿 컨트롤러 설계

### 학습목표

- 디자인 패턴을 이해한다.
- MVC 패턴의 구조를 이해한다.

## Section 01 MVC 패턴

### 디자인 패턴이란?

- 디자인 패턴(Design Pattern)

  - 처음에는 건축학적 관점에서 출발한 기념이었음
  - 1994년 GoF의 <Design Patterns: Elements of Reusable Object-Oriented Software>를 통해 소프트웨어 설계에서 공통적으로 발생하는 문제에 대한 재사용 가능한 솔루션을 제시됨
  - GoF의 디자인 패턴은 생성, 구조, 행동, 동시실행 등의 문제에 대해 여러 패턴을 제시하고 있으며 UML 클래스 다이어그램을 이용해 구조를 표현하고 있음

- UML(Unified Modeling Language)
  - 객체지향 설계와 구현을 지원하기 위해 만들어진 일종의 모데링 언어
  - 시스템 분석, 설계에 필요한 내용을 여러 다이어그램 형태로 정의한 규격임

### 추상 팩토리 패턴이란?

- 추상 팩토리(Abstract Factory) 패턴

  - 추상(Abstract): 자바의 추상 클래스 Abstract Class에도 사용되는 표현으로 구체적인 내용의 구현을 하위 객체에 위임하는 모델임
  - 팩토리(Factory): 디자인 패턴에서 객체를 생성하는 역할을 의미함
  - 따라서 추상 팩토리는 객체를 생성하는 것과 별도로 구현하되 관련된 구체적인 구현을 하위 클래스에서 담당하게 하는 설계 모델로 이해할 수 있음

- 추상 팩토리 패턴(Abstract Factory Pattern)의 UML 클래스 다이어그램
  - 객체 생성에 대한 문제 해결을 위한 디자인 패턴

### MVC 패턴이란?

- MVC 패턴

  - Model-View-Controller의 약어
  - GUI(Graphical User Interface) 기반의 애플리케이션 개발에 사용되는 디자인 패턴
  - 지금은 백엔드 기반의 웹 애플리케이션 개발의 기본 모델이 되었음
    - 모바일 앱 및 프런트엔드 기반 웹 애플리케이션 개발이 늘어나면서 MVP(Model-View-Presenter), MVVM(Model-View-ViewModel)과 같은 패턴도 널리 사용되고 있음
  - MVC 패턴의 목적은 화면과 데이터 처리를 분리하여 코드 간 종속성을 줄이는 것임
  - 즉 구성요소 간 역할을 명확하게 하여 코드를 쉽게 분리하고 협업이 용이해짐

- 모델(Model)
  - 데이터를 처리하는 영역
  - 일반적으로 데이터베이스와 연동을 위한 DAO(Data Access Object) 클래스와 데이터 구조를 표현하는 DO(Data Object, 엔티티 클래스) 등으로 구성됨
  - 모델은 뷰나 컨트롤러에 독립적인 구조로 데이터베이스 처리를 필요로 하는 여러 애플리케이션에서 공유할 수 있으며, 웹 애플리케이션이 아닌 경우에도 사용할 수 있음

```DAO, DO는 필수적인가?

효과적인 DB 연동 구현을 위해 JPA를 사용하는 경우 DAO는 생략되거나 구현 범위가 축소될 수 있다. 또한 하나의 화면 구성을 위해 여러 데이터베이스 혹은 외부 서비스 등과 연계해야 하는 경우 이들을 통합하기 위해 별도의 서비스 객체를 두거나 DO 이외에 DTO 등을 사용하기도 한다.

```

- 뷰(View)

  - 화면 구성을 담당하는 영역
  - 주어진 데이터를 출력하는 용도로만 사용하는 것이 바람직함
    - 뷰에서 데이터를 직접 가져오는 방식은 권장하지 않음
  - 뷰 영역의 구현을 위해 뷰 템플릿 엔진이 사용되며 JSP 역시 이러한 뷰 템플릿 엔진 중 하나임
  - HTML 이외에 EL, JSTL 등을 사용해 컨트롤러로부터 전달받은 데이터를 출력하고 HTML, CSS 등을 통해 화면을 디자인함
  - 뷰는 기본적으로 모델, 컨트롤러와의 종속성이 없도록 구현해야 함

- 컨트롤러(Controller)

  - MVC 패턴의 핵심으로 모든 사용자 요청의 중심에 위치함
  - 사용자 요청은 특정 뷰에 바로 전달되지 않고 컨트롤러를 통해야 함
  - 컨트롤러는 사용자 요청에 따라 모델을 통해 데이터베이스와 연동하여 데이터를 처리하고 뷰에 전달함
  - 뷰로 전달하기 위해 데이터가 들어 있는 DO 혹은 List<DO> 형태의 객체를 request에 저장한 후 포워딩함
  - 컨트롤러는 특정 뷰를 지정해야 하기 때문에 뷰와 종속관계가 발생할 수밖에 없음
  - 따라서 프로젝트의 규모가 클수록 컨트롤러는 복잡해지고 관리가 어려워지는 문제가 있음

- 컨트롤러의 구현
  - JSP, 서블릿 모두 가능함
  - JSP: 간단한 기능을 구현할 때 유리함
  - 서블릿: 규모 확장과 향후 스프링 프레임워크로의 확장 등에 유리함
    - 컨트롤러를 호출하기

```서블릿만으로 컨트롤러 구현이 가능할까?

단순하게 서블릿만으로 컨트롤러를 구현하는 것은 어렵지 않지만 실제 개발에서는 시스템 전반을 통합할 수 있는 컨트롤러 구조를 설계해서 일종의 프레임워크 개념으로 만들어둘 필요가 있다. 따라서 이러한 부분을 개인 혹은 회사에서 개발하거나 스프링 프레임워크와 같이 오픈소스 프레임워크를 사용하게 된다.

```

- 컨트롤러를 구성하는 세 가지 방법
  1. 사용자 요청마다 컨트롤러를 만들기
  2. 특정 모듈 단위로 하나의 컨트롤러 안에서 여러 요청 단위를 구분해 처리하기
  3. 프론트 컨트롤러를 따로 두어 모든 요청을 하나의 컨트롤러로 모은 다음 요청에 따라 구현 컨트롤러를 호출하기

## Section 02 서블릿 컨트롤러 설계

### 클라이언트 요청 처리

- 컨트롤러의 가장 기본적인 기능 세 가지

1. 클라이언트 요청 처리
2. 입력값 핸들링
3. 뷰 이동

#### 1. 클라이언트 요청 처리

- 클라이언트 요청을 단일 컨트롤러에서 처리할 것인지 개별 컨트롤러에서 처리할 것인지 결정해야 함
- 서블릿은 URL 요청을 GET, POST 등의 HTTP 메서드를 통해 처리하는 구조이기 때문에 여러 URL 패턴을 하나의 서블릿에서 처리할 수 있지만 URL에 따라 다른 처리를 구현할 수는 없음

- 예시 1) 어떤 쇼핑몰의 제품 등록과 삭제 기능

```
* 제품 등록 요청 URL: /shop/addProduct
* 제품 삭제 요청 URL: /shop/delProduct

```

- 이때 두 URL 요청을 하나의 서블릿으로 처리하도록 URL 매핑 설정을 할 수 있으나, 두 요청 모두 GET 방식이라면 URL이 다르더라도 동일한 doGet() 메서드가 호출되기 때문에 어떤 요청이 호출된 것인지 구분할 수 없음

- 따라서 각각의 URL 요청을 별도의 서블릿으로 구현해야 함

```GET 방식과 POST 방식

GET 메서드는 특정 리소스의 표시를 요청할 때 사용한다. GET을 사용하는 요청은 파라미터(Query String)을 통해 클라이언트에서 작성된 데이터를 서버로 전송하며, POST 메서드는 특정 서버 리소스(서블릿 등)에 데이터를 전송할 때 사용된다. GET 방식과 달리 웹 브라우저의 주소창에 데이터 부분은 보이지 않으며 데이터는 RequestBody에 포함되어 전달된다.

```

- 예시2) 회원 관리 프로그램의 회원 가입, 승인, 수정, 탈퇴(삭제), 로그인 기능

```
* 가입 양식 입력 화면 -> 가입 처리 컨트롤러 -> 가입 완료 화면
* 신규 회원 관리자 화면 -> 신규 회원 가입 승인 컨트롤러 -> 승인 완료 화면
* 로그인 양식 입력 화면 -> 로그인 처리 컨트롤러 -> 메인 화면
```

- 각각의 요청을 처리하기 위한 컨트롤러가 있어야 함

- 즉 입력/요청 화면과 입력 데이터를 받아 이를 처리하는 컨트롤러, 처리된 결과를 보여주기 위한 화면이 필요함

- 이 경우 각각의 컨트롤러를 구현해야 하지만 같은 단위의 업무를 하나의 컨트롤러에서 처리하는 것이 구조적으로 관리가 쉬울 수 있음

  - 물론 하나의 컨트롤러에서 처리할 요청이 지나치게 많은 경우 오히려 코드 관리가 어려울 수 있으므로 주의해야 함

- 사용자의 요청을 구분해 하나의 서블릿에서 처리하기 위한 두 가지 방법

1. URL의 파라미터 이용
2. 프런트 컨트롤러 구현

- URL의 파라미터 이용
  - URL에 action과 같이 별도의 파라미터를 두어 요청을 구분함

```
* http://xxx.com/member?acction=create
* http://xxx.com/member?action=login

```

- member: 서블릿 URL 매핑값
- action: 요청을 구분하기 위한 파라미터

  - 요청에는 회원 정보, 로그인 아이디 등의 사용자 데이터가 추가로 포함됨
  - GET, POST 방식이 모두 가능함
  - 컨트롤러에서는 action 값을 비교하여 별도의 메서드 구현 등의 방식으로 처리함
  - 비교적 간단한 방법이지만 action 파라미터의 구조가 변경되며 관련된 HTML, JSP, 컨트롤러의 수정이 필요하다는 단점이 있음

* URL의 파라미터 이용의 실제 구현: 분기문 사용

```java

doGet(...) {
    String action = request.getParameter("action");
    switch(action) {
        case "create": createMember(); break;
        case "login": loginMember(); break;
    ...
    }
}

```

- 프론트 컨트롤러 구현(1)

  - 모든 요청의 진입점이 되는 컨트롤러가 있고 여기서 서브 컨트롤러를 호출하는 구조
  - 좀 더 복잡한 구조를 체계적으로 처리할 수 있음
  - 프런트 컨트롤러 패턴으로 정립되어 있어 여러 구현에 응용되는 디자인 패턴임
  - 모든 요청을 하나로 모으는 방법이 필요하며, 일반적으로는 서블릿 매핑의 구조적인 특징을 활용함

  ```
    * http://xxx.com/member/create.do
    * http://xxx.com/member/login.do
  ```

  - \*.do: 서블릿 URL 매핑값으로 모든 요청은 하나의 서블릿을 호출함

  - 컨트롤러에서는 .do 앞의 요청 이름(create, login)을 구분하여 별도의 메서드 혹은 서브클래스를 통해 실행함

- 프런트 컨트롤러 구현의 장점

  - 요청에 대한 파라미터 없이 명확한 이름(전 예시의 create, login)으로 요청할 수 있음
  - 요청에 대한 URL 관리가 필요 없음

- 프런트 컨트롤러 구현의 단점

  - 전체 시스템이 세부 시스템이 분리되어 있는 경우: 컨텍스트를 분리하는 것은 세션 관리 등에 부담이 갈 수 있음

    - 예) 네이버와 같은 포털 형태

  - 단일 콘텍스트에 경로로 구분하는 경우: 프런트 컨트롤러에서 모든 요청을 조건문과 메서드 구현만으로 처리하기에는 컨트롤러 클래스가 비대해짐

  - -> 규모가 어느 수준 이상이 되면 경로에 따라 서브 컨트롤러로 포워딩하는 처리가 필요함

- 서브 컨트롤러 구현하기
  - URL 요청을 분석해 사용ㅇ자 요청을 구분하는 작업이 우선적으로 필요함
  - 메서드를 이용해 사용자 요청을 분리해서 처리함
    - switch(혹은 if) 문을 사용하는 구조는 기능 추가ㅏ 또는 변경이 필요할 때 조건문도 함께 관리해야 하는 문제가 있음

```java

@WebServlet("*.do")
public class MemberController extends HttpServlet {
    ...
    doGet(...) {
        String uri = request.getRequestURI();
        String conPath = request.getContextPath();
        String command = uri.substring(conPath.length());

        switch(command) {
            case "create.do": createMember(); break;
            case "login.do": loginMember(); break;
        }
    }
}

```

### 클라이언트 요청 처리

- 서브 컨트롤러 구현하기
  - Command 패턴을 사용하면 switch (혹은 if) 구조 없이 해당 요청에 맞는 특정 컨트롤러가 실행되도록 구현할 수 있음

```java

public void init(ServletConfig sc) throws ServletException {
    Map<String, SubController> contList = new HashMap<>();

    contList.put("/create.do", new MemberCreateController());

    contList.put("/login.do", new LoginController());
}

public void service(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
    String url = request.getRequestURI();
    String contextPath = request.getContextPath();
    String path = url.substring(contextPath.length());

    SubController subController = contList.get(path);
    subController.process(request, response);
}

```

- 서브 컨트롤러를 운영하는 프론트 컨트롤러를 설계하기

  - 먼저 서브 컨트롤러의 규격을 정의한 인터페이스가 필요함

  ```java
    public interface SubController {
        void process(HttpServletRequest request, HttpServletResponse response);
    }
  ```

  - 다음으로 필요한 서브 컨트롤러를 구현함
    - 앞의 다중 요청 처리 서블릿을 서브 컨트롤러 규격에 맞게 수정함

  ```java
    public class MemberController implements SubController {
        void process(HttpServletRequest request, HttpServletResponse response) {

        }
    }
  ```

  - 기본적으로는 단일 기능을 수행하며 서브 URL 경로를 추가로 구분해 메서드 단위로 여러 요청을 처리하도록 구현하는 것도 가능함

> > 스프링 프레임워크에서 프런트 컨트롤러 구현

스프링 프레임워크의 경우 애너테이션 방식으로 구현하기 때문에 클래스 생성 시 컨트롤러 클래스로 지정한 다음 메서드 단위로 URL 매핑이 가능해 훨씬 간편하면서도 관리가 용이한 구조의 컨트롤러 운영이 가능하다.

```java

@Controller
@RequestMapping("/member")
public class MemberController {
    @PostMapping("create")
    public void createMember() {

    }
}

```

- http://xxx.com/member/create URL로 POST 요청을 처리한다.
- 같은 방법으로 BlogController, CafeController 등을 두어 운영할 수도 있다.

### 입력값 핸들링

2. 입력값 핸들링

- 서블릿에서 클라이언트의 입력값을 처리하려면 request.getParameter()를 이용해야 함

- 파라미터가 한 두개라면 문제 없겠지만 회원가입과 같이 여러 정보가 전달되는 경우 모든 값을 request.getParameter()로 받는 것은 문제가 됨

- 또한 DAO 클래스와 연동을 위해서는 입력값을 Member 객체로 만든 후에 전달해야 하므로 기본적으로 다음과 같은 코드 구현이 필요함

```java
  doGet(...) {
      Member m = new Member();
      m.setName(request.getParameter("name"));
      m.setTel(request.getParameter("tel"));
      ...
      dao.create(m);
  }
```

- JSP에서는 useBean 액션을 통해 입력값을 Member 객체로 쉽게 만들 수 있음
- 서블릿에서는 그런 기능이 제공되지 않기 때문에 별도의 라이브러리를 사용해야 함
- 대표적으로 Apache Commons BeanUtils가 쓰임

```java
  doGet(...) {
      Member m = new Member();
      BeanUtils.populate(m, request.getParameterMap());
      ...
      dao.create(m);
  }
```

> > 스프링 프레임워크에서의 입력값 핸들링

스프링 프레임워크에서는 기본적으로 메서드 파라미터 지정으로 자동 처리가 가능하다. 즉 create라는 URL 요청으로 전달되는 파라미터를 Member 클래스의 멤버 변수에 자동으로 전달해 인자로 제공한다.

```java

@PostMapping("create")
public void createMember(Member m) {
    dao.create(m);
}

```

### 뷰 이동

3. 뷰 이동

- 컨트롤러에서 사용자 요청을 처리한 다음에는 적절한 뷰로 이동할 수 있어야 함

- 뷰에서 보여줄 데이터를 포함해서 이동해야 하는 경우와 그렇지 않은 경우로 나뉨

- 데이터를 포함하지 않는 경우
  - 사용자 요청 처리 후 별도의 데이터를 포함하지 않는다면 해당 페이지로 리디렉션 Redirection 할 수 있음
  - JSP, 서블릿 모두 response.sendRedirect()를 사용함

```java
response.sendRedirect("main.jsp");
```

- 데이터를 포함하는 경우
  - request scope object에 속성으로 데이터를 넣은 후 원하는 페이지로 포워딩함
  - 데이터 활용 목적에 따라 session 또는 application을 사용할 수도 있으며 여러 데이터를 포함하는 것도 가능함

```java

doGet(...) {
    ...
    request.setAttribute("member", m);
    RequestDispatcher dispatcher = request.getRequestDispatcher("userInfo.jsp");
    dispatcher.forward(request, response);
}

```

> > JSP와 스프링 프레임워크에서의 뷰 이동

JSP에서 뷰 이동을 구현할 경우:

```java
<%
    request.setAttribute("member", m):
    pageContext.forwared("UserInfo.jsp");
%>
```

스프링 프레임워크의 경우 인자로 전달된 Model 객체에 원하는 데이터를 저장하고 뷰 페이지 이름을 리턴하기만 하면 된다.

```java
@GetMapping("info")
public String getMemberInfo(int id, Model model) {
    ...
    model.addAttribute("member", m);
    return "userInfo";
}
```

- 리턴되는 문자열 값은 뷰 페이지의 이름이며 확장자는 생략된다.
  서블릿, JSP, 스프링 프레임워크 모두 코드는 조금씩 다르지만 기본 개념과 구조는 유사하다는 것을 알 수 있다.

## Section 03 [실습 8-1] 컨트롤러 기초 예제: 계산기 구현

### 실습 8-1 컨트롤러 기초 예제: 계산기 구현

- 컨트롤러 동작 원리와 구현 방법을 배우기 위해 단순한 구조의 컨트롤러를 구현해봄

- 7장에서 구현했던 Calculator 클래스를 컨트롤러와 모델 영역으로 구분하고 결과를 보여줌

1. 뷰 구현: calForm.html, calcResult.jsp
2. 모델 구현: Calculator.java
3. 컨트롤러 구현: CalcController.java

```html
// calcForm.html

<!DOCTYPE html>
<html>
  <head>
    <meta charset="UTF-8" />
    <title>계산기-컨트롤러</title>
  </head>
  <body>
    <h2>계산기-컨트롤러</h2>
    <hr />
    <form method="post" action="/jwbook/calcControl">
      <input type="text" name="n1" size="10" /><select name="op">
        <option>+</option>
        <option>-</option>
        <option>*</option>
        <option>/</option></select
      ><input type="text" name="n2" size="10" />
      <input type="submit" value="실행" />
    </form>
  </body>
</html>
```

```html
// calcResult.jsp <% page language="java" contentType="text/html; charset=UTF-8"
pageEncoding="UTF-8" %>

<!DOCTYPE html>
<html>
  <head>
    <meta charset="UTF-8" />
    <title>계산기-컨트롤러</title>
  </head>
  <body>
    <h2>계산 결과-컨트롤러</h2>
    <hr />
    결과: ${result}
  </body>
</html>
```

```java
// CalcController

package ch08;
import java.io.IOException; // 다른 import 문은 생략함

@WebServlet("/calcControl")
public class CalcController extends HttpServlet {
    private static final long serialVersionUID = 1L;

    public calcController() {
        super();
    }

    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        int n1 = Integer.parseInt(request.getParameter("n1"));
        int n2 = Integer.parseInt(request.getParameter("n2"));

        long result = 0;

        switch(request.getParameter("op")) {
            case "+": result = n1+n2; break;
            case "-": result = n1-n2; break;
            case "/": result = n1/n2; break;
            case "*": result = n1*n2; break;
        }

        request.setAttribute("result", result);
        getServletContext().getRequestDispatcher("/ch08/calcResult.jsp").forward(request, response);
    }

    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        doGet(request, response);
    }
}
```

```jsp
// productList.jsp

<%@ page language="java" contentType="text/html; charset=UTF-8" pageEncoding="UTF-8"%>
<%@ taglib uri="http://java.sun.com/jsp/jstl/core" prefix="c"%>

<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>상품 목록</title>
</head>
<body>
  <h2>상품 목록</h2>
  <hr>
  <table border="1">
  <tr> <th>번호</th> <th>상품명</th> <th>가격</th> </tr>
  <c:forEach var="p" varStatus="i" items="${products}">
    <tr>
        <td>${i.count}</td>
        <td><a href="/jwbook/pcontrol?action=info%id=${p.id}">${p.name}</a></td>
        <td>${p.price}</td>
    </tr>
  </c:forEach>
  </table>
</body>
</html>
```

```jsp
// productInfo.jsp

<%@ page language="java" contentType="text/html; charset=UTF-8" pageEncoding="UTF-8"%>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>상품정보 조회</title>
</head>
<body>
    <h2>상품정보 조회</h2>
    <hr>
    <ul>
        <li>상품코드: ${p.id}</li>
        <li>상품명: ${p.name}</li>
        <li>제조사: ${p.maker}</li>
        <li>가격: ${p.price}</li>
        <li>등록일: ${p.date}</li>
    </ul>
</body>
</html>

```

### 모델 구현: Product, ProductService

- Product 클래스: 상품 정보를 표현하기 위한 DO 객체
- ProductService 클래스: 데이터베이스 없이 샘플 데이터를 제공하기 위한 클래스

```java
// Product.java

package ch08;

public class Product {
    private String id;
    private String name;
    private String maker;
    private int price;
    private String date;

    // 데이터 생성을 쉽게 하기 위한 생성자
    public Product(String id, String name, String maker, int price, String date)
    {
        this.id = id;
        this.name = name;
        this.maker = maker;
        this.price = price;
        this.date = date;
    }

    // getter, setter 메서드
    public String getId() { return id; }
    public void setId(String id) { this.id = id; }
    public String getName() { return name; }
    public void setName(String name) { this.name = name; }
    public String getMaker() { return maker; }
    public void setMaker(String maker) { this.maker = maker; }
    public int getPrice() { return price; }
    public void setPrice(int price) { this.price = price; }
    public String getDate() { return date; }
    public void setDate(String date) { this.date = date; }
}
```

- 이번 실습에서는 데이터베이스 없이 샘플 데이터를 Map 형태로 저장하고 전체 상품 목록과 특정 상품 정보를 컨트롤러에 제공하기 위한 서비스 클래스를 만듦

- 서비스 클래스 ProductService는 컨트롤러 서블릿의 init() 메서드에서 초기화되기 때문에 서블릿이 실행되는 동안에는 데이터가 유지되지만 서블릿이 다시 시작되거나 톰캣을 종료하면 데이터는 초기화됨
  - 따라서 샘플 데이터 이외에 별도 기능 구현을 통해 추가한 데이터가 있다면 삭제될 위험이 있으니 주의해야 함
  - 이번 실습에서 데이터 추가 기능은 구현하지 않음

```java

// ProductService.java
package ch08;

import java.util.HashMap;
import java.util.Map; // 이하 import 문은 생략

public class ProductService {
    Map<String, Product> products = new HashMap<>();

    public ProductService() {
        Product p = new Product("101", "애플사과폰 12", "애플전자", 1200000, "2020.12.12");
        products.put("101", p);
        p = new Product("102", "삼전우주폰 21", "삼전전자", 1300000, "2021.2.2");
        products.put("102", p);
        p = new Product("103", "엘스듀얼폰", "엘스전자", 1500000, "2021.3.2");
        products.put("103", p);
    }

    public List<Product> findAll() {
        return new ArrayList<>(products.values());
    }

    public Product find(String id) {
        return products.get(id);
    }
}

```

- 이번 실습에서는 doGet, doPost 메서드를 오버라이딩하지 않고 service 메서드를 오버라이딩하여 컨트롤러 기본 구조를 구현함

- 각각의 요청은 action 파라미터를 비교해 메서드를 호출한 다음 뷰로 이동하는 구조로 구현함

- ProductController 서블릿에서 생성된 기본 코드에 다음과 같이 추가함

```java
@WebServlet("/pcontrol")
public class ProductController extends HttpServlet {
    ProductService service;
    ...
    public ProductController() {
        service = new ProductService();
    }
    @Override
    protected void service(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {

    }
    ...
}
```

- 다음으로 클라이언트 요청을 구분하고 처리 메서드를 호출한 다음 뷰로 이동하는 구조를 service() 메서드에 작성함
  - action 파라미터가 null인 경우(서블릿이 action 파라미터 없이 호출된 경우): 컨트롤러에 action 파라미터를 넣어 포워딩함

```java

@Override
protected void service(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {

    String action = request.getParameter("action");
    String view = "";

    if (request.getParameter("action") == null) {
        getServletContext().getRequestDispatcher("/pcontrol?action=list").forward(request, response);
    } else {
        switch(action) {
            case "lsit": view = list(request, response); break;
            case "info": view = lnfo(request, response); break;
        }
        getServletContext().getRequestDispatcher("/ch08/"+view).forward(request, response);
    }
}
```

각각의 list()와 info() 메서드는 실행 후 문자열로 된 JSP 파일 이름을 리턴하고 [ch08] 폴더에 있는 JSP 파일로 포워딩함

```java

private String list(HttpServletRequest request, HttpServletResponse response)
{
    request.setAttribute("products", service.findAll());
    return "productList.jsp";
}

private String info(HttpServletRequest request, HttpServletResponse response) {
    request.setAttribute("p", service.find(request.getParameter("id")));
    return "productInfo.jsp";
}
```

```java
// ProductController.java
package ch08;
import java.io.IOException; // 이하 import 문은 생략

@WebServlet("/pcontrol")
public class ProductController extends HttpServlet {
    private static final long serialVersionUID = 1L;
    ProductService service;

    public void init(ServletConfig config) throws ServletException {
        super.init(config);
        service = new ProductService();
    }

    @Override
    protected void service(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        String action = request.getParameter("action");
        String view = "";

        if (action == null) {
            getServletContext().getRequestDispatcher("/pcontrol?action=list").forward(request, response);
        } else {
            switch(action) {
                case "list": view = list(request, response); break;
                case "info": view = info(request, response); break;
            }
            getServletContext().getRequestDispatcher("/ch08/"+view).forward(request, response);
        }
    }

    private String info(HttpServletRequest request, HttpServletResponse response) {
        request.setAttribute("p", service.find(request.getParameter("id")));
        return "productInfo.jsp";
    }

    private String list(HttpServletRequest request, HttpServletResponse response) {
        request.setAttribute("products", service.findAll());
        return "productList.jsp";
    }
}
```

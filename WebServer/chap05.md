# Web Server 5

## Web Server Programming

#### 2024.10.18.(금)

## Chapter 05 서블릿의 이해

### 목차

1. 서블릿의 개요
2. 서블릿 클래스 구조와 생명 주기
3. 페이지 이동과 정보 공유
4. [실습 5-1] 서블릿 프로그래밍: Hello World!
5. [실습 5-2] 서블릿 프로그래밍: 계산기 구현

### 학습 목표

- 서블릿의 동작 과정을 이해한다.
- 서블릿 클래스의 구조와 생명 주기를 익힌다.
- 페이지 이동과 함께 페이지 간 정보 공유 기법을 익힌다.

## Section 01 서블릿의 개요

### 서블릿의 동작 과정

- 서블릿 코드 작성, 컨테이너 등록, 클라이언트 요청에 따른 동작 과정

- 서블릿의 장점

  - 자바를 기반으로 하므로 자바 API를 모두 사용할 수 있음
  - 운영체제나 하드웨어의 영향을 받지 않으므로, 한번 개발된 애플리케이션은 다양한 서버 환경에서도 실행할 수 있음
  - 웹 애플리케이션에서 효율적인 자료 공유 방법을 제공함
  - 다양한 오픈소스 라이브러리와 개발도구를 활용할 수 있음

- 서블릿의 단점
  - HTML 응답을 위해서는 출력문으로 문자열 결합을 사용해야 함
  - 서블릿에서 HTML을 포함할 경우 화면 수정이 어려움
  - HTML 폼 Form의 데이터 처리가 불편함
  - 기본적으로 단일 요청과 응답을 처리하는 구조로 다양한 경로의 URL 접근을 하나의 클래스에서 처리하기 어려움

* 실제 자바 웹 개발에서의 서블릿 조합
  - 화면 구성을 위해 JSP와 같은 템플릿 엔진을 사용함
  - REST API 구현을 위해서는 JAX-RS를 사용함
  - 복잡한 서비스 구현을 위해 프론트 컨트롤러 모델 등을 사용함

## Section 02 서블릿 클래스 구조와 생명 주기

### 서블릿 클래스의 구조

- 서블릿 클래스의 구조
  - 서블릿 자체는 자바로 구현하지만 서블릿 컨테이너에 해당 클래스가 서블릿임을 알려야 하며, 어떤 URL 접근에 실행해야 하는지 등록하는 과정이 필요함
  - 서블릿 2.0
    - 웹 애플리케이션 구조를 컨테이너에 알려주기 위한 배포 서술자 web.xml에 등록해야 함
  - 서블릿 3.0
    - 별도의 web.xml 작성 없이 자바 애너테이션을 이용함
  - javax.servlet.Servlet 인터페이스를 구현한 추상 클래스인 GenericServlet 클래스와 HttpServlet 클래스 중 하나를 상속해 구현함
    - HTTP 프로토콜에 최적화되어 있는 HttpServlet 클래스를 상속해 구현하는 것이 좋음
    - HttpServlet을 상속받아 doGet(), doPost() 메서드를 오버라이딩한 구조

```Java
public class HelloWorldServlet extends HttpServlet {
    public doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        ...
    }

    public doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        doGet(request, response);
    }
    ...
}
```

> request와 response

request와 response는 session, application과 함께 JSP의 내장객체이기도 하며 속성 관리 기능을 이용해 컨트롤러와 뷰 페이지 간 데이터 전달 등의 목적에도 사용되므로 잘 알아두도록 한다.

- HttpServletRequest

  - HTTP 프로토콜의 request 정보를 서블릿에 전달하기 위한 목적으로 사용함
    - 이때 사용하는 클래스에는 헤더 정보, 파라미터, 쿠키, URL, URI 등의 정보를 읽어 들이는 메서드와 HTTP Body의 Stream을 읽어 들이는 메서드를 가지고 있음
  - 서블릿 컨테이너에서 생성되고 클라이언트 요청이 doGet(), doPost()로 전달될 때 인자로 함께 전달됨
  - 서블릿에서 클라이언트와 연결해 처리할 작업은 모두 HttpServletRequest를 통해야 함

- HttpServletResponse

  - HttpServletRequest와 마찬가지로 클라이언트와 연결된 처리가 가능함
  - 다만 클라이언트에서 서버로 전달하는 것과 관련된 것이 아니라 서버에서 클라이언트로 전달하려는 목적을 위한 기능으로 구성됨
  - 서블릿 컨테이너는 요청 클라이언트에 응답을 보내기 위한 HttpServletResponse 객체를 생성하여 서블릿에 전달함
  - 서블릿은 해당 객체를 이용하여 content type, 응답 코드, 응답 메세지 등을 전송할 수 있음

- 서블릿 정보 등록
  - 서블릿 클래스만으로는 톰캣에서 실행이 불가능하기 때문에 web.xml 이나 애너테이션으로 서블릿임을 선언해야 함
  - 서블릿 2.x에서 사용 가능한 web.xml의 작성 예

```xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app ...>
    <servlet>
        <servlet-name>HelloWorld</servlet-name> // servlet 이름
        <servlet-class>jwbook.servlet.HelloServlet</servlet-class> // 서블릿 클래스 지정
    </servlet>

    <servlet-mapping>
      <servlet-name>HelloWorld</servlet-name> // servlet name을 매핑
      <url-pattern>/hello</url-pattern> // 서블릿 요청 주소 매핑
    </servlet-mapping>
</web-app>
```

- 서블릿 정보 등록

  - 서블릿 3.0에서 자바 애너테이션을 이용한 등록
    - 필요하다면 web.xml을 사용하는 것도 가능하지만 애너테이션 사용이 권장됨

  ```java
  @WebServlet(description="Hello World Servlet", urlPatterns="/hello")
  public class HelloWorldServlet extends HttpServlet {
    ...
  }
  ```

### 서블릿의 생명 주기

- 서블릿의 생명 주기
  - 객체의 생성에서 종료에 이르는 과정

| 이벤트 | 생명 주기 메서드 | 실행 |
| 서블릿 초기화 | init() | 초기에 한 번만 실행된다. |
| 요청/응답 | service(), doGet(), doPost() | 스레드를 통해 동시에 실행된다. |
| 서블릿 종료 | destroy() | 종료할 때 한번만 실행된다. |

- 서블릿 초기화: init() 메서드

  - 클라이언트 요청이 들어오면 컨테이너는 해당 서블릿이 메모리에 있는지 확인함
  - 해당 서블릿이 메모리에 없을 경우에는 서블릿을 메모리에 적재해야 하는데, 이때 서블릿의 init() 메서드가 호출되며 각종 초기화 작업을 수행함
  - init() 메서드는 처음 한번만 실행되므로, 해당 서블릿에 각각의 스레드에서 공통적으로 사용하는 작업이 있다면 init() 메서드를 오버라이딩해서 구현함
  - 만일 실행 중 서블릿이 변경되는 경우 기존 서블릿은 종료 Destroy 되고 다시 시작되면서 init() 메서드가 호출됨

- 요청/응답: service() 메서드

  - init() 메서드는 최초에 한번만 수행되고 이후 요청은 스레드로 실행되며, service() 메서드를 통해 각각 doGet()이나 doPost()로 분기됨
  - 이때 파라미터로 HttpServletRequest와 HttpServletResponse 클래스 타입인 request와 response 객체가 제공됨
    - request: 사용자 요청 처리
    - response: 응답 처리

- 서블릿 종료: destroy() 메서드
  - 컨테이너로부터 서블릿 종료 요청이 있을 때 destroy() 메서드를 호출함
  - init() 메서드와 마찬가지로 한번만 실행되며, 서블릿이 종료되면서 정리해야 할 작업이 있을 때는 destroy() 메서드를 오버라이딩해서 구현함

## Section 03 페이지 이동과 정보 공유

### 페이지 이동

- 데이터를 포함하지 않는 경우
  - 사용자 요청 처리 후 별도의 데이터를 포함하지 않는다면 해당 페이지로 바로 리디렉션할 수 있음
    - 세션에 데이터를 저장한 경우라면 세션이 유효한 동안 모든 페이지에서 세션 정보를 참조할 수 있어 리디렉션을 통해서도 데이터 참조가 가능함
  - JSP, 서블릿 모두 response.sendRedirect()를 사용할 수 있음

`response.sendRedirect("main.jsp");`

- 데이터를 포함하는 경우

  - 데이터를 포함하며 이동한다면 reqeust 속성으로 데이터를 넣은 후 원하는 페이지로 포워딩해야 함
  - 데이터 활용 목적에 따라 session이나 application을 사용할 수도 있으며 여러 데이터를 포함하는 것도 가능함
  - JSP로 구현할 경우

  ```JSP
    <%
        request.setAttribute("member", m);
        pageContext.forwared("userInfo.jsp");
    %>
  ```

  - 서블릿으로 구현할 경우

  ```java
  doGet(...) {
    ...
    request.setAttribute("member", m);
    RequestDispatcher dispatcher = request.getRequestDispatcher("userInfo.jsp");
    dispatcher.forward(request, response);
  }
  ```

  - 스프링 프레임워크로 구현할 경우

  ```java
  @GetMapping("info")
  public String getMemberInfo(int id, Model model) {
    ...
    model.addAttribute("member", m);
    return "userInfo";
  }
  ```

### 정보 공유

- URL rewriting

  - HTTP의 Query String을 이용하는 방식으로 URL에 파라미터를 추가해 서버로 요청하는 형식
  - 정보 유지를 위해 파라미터를 매 페이지마다 확인하고 계속 추가해 주어야 하며, 복잡한 정보 유지는 어려움

- 쿠키(Cookie)

  - 란 클라이언트에 저장되는 작은 정보를 의미함
  - 서버의 요청에 의해 브라우저가 저장하게 되며 서버가 요청할 때 제공하는 형식

- 쿠키의 특징

  - 파일로 클라이언트의 컴퓨터에 저장되는 방식이며 보안상 문제가 있을 수 있음
  - 광고 혹은 기타 목적으로 사용자의 이용 행태 추적에 이용될 수 있음
    - 이러한 목적의 경우 사용자 정보 활용 동의가 필요함
  - 재방문 등의 확인 용도로 많이 사용됨
  - 'name=value' 형식을 사용함
    - 유효 기간, 요청 경로, 도메인 지정 등의 부가 속성을 포함함
  - 주로 자바스크립트를 통해 처리하지만 HttpOnly 설정으로 서버에서만 사용할 수 있도록 설정 가능함

- 쿠키 저장 방법

  ```java
  response.addCookie(new Cookie("name", "hong"));
  response.addCookie(new Cookie("tel", "010-1234-12345"));
  response.addCookie(new Cookie("email", "hong@korea.com"));
  ```

  - 위의 코드는 다음의 HTTP 응답으로 클라이언트에 전달됨

  ```http
  HTTP/1.1 200 OK
  Server: Apache-Coyote/1.1
  Set-Cookie: name=hong
  Set-Cookie: tel=010-1234-1234
  Set-Cookie: email=hong@korea.com
  Content-Type: text/html;charset=utf-8
  Content-Length: 160
  Date: Sun, 13 Sep 2020 11:11:13 GMT

  ...HTTP BODY...
  ```

- 세션(Session)

  - 클라이언트가 웹 애플리케이션 서버에 접속할 때 서버 쪽에 생성되는 공간으로 내부적으로는 세션 아이디를 통해 참조됨
    - 브라우저: 서버에 접속할 때 발급받은 세션 아이디를 기억함
    - 서버: 해당 세션 아이디로 할당된 영역에 접근함

- 세션의 특징
  - 세션 유효 시간이나 브라우저 종료 전까지 유지되므로 서로 다른 페이지에서도 정보 공유가 가능함
  - 로그인 유지, 장바구니, 컨트롤러 구현 등에서 다양하게 사용됨
  - 사용자마다 생성되는 공간임
  - 동시에 많은 사용자가 세션을 통해 대량의 데이터를 관리한다면 충분한 메모리를 비롯한 세션 관리 대책이 필요함

### 속성 관리

- Scope Object

  - 컨테이너에서 서블릿 관리를 위해 자동으로 생성한 객체 중 속성 관리 기능을 제공하며 특정 범위 동안 유지되는 객체를 의미함
  - 각각의 객체는 관리 목적에 따라 별도의 메서드로 구현된 기능을 가지고 있고 공통적으로 '키-값' 형태의 맵(Map) 자료구조를 가짐
    - 이를 활용하면 페이지 간, 사용자 간 데이터 공유가 가능함
  - JSP 역시 서블릿으로 변환되기 때문에 동일하다고 볼 수 있음
    - userBean 액션의 scope에 사용되는 page, request, session, application이 해당됨
  - 이러한 객체는 각각 생성, 소멸 시기가 정해져 있고 서로 다른 JSP, 서블릿 간의 데이터 전달이나 공유를 위한 용도로 활용됨

- Scope Object의 종류와 특징

| Scope Object | 클래스 | 생성 | 소멸 | 범위 |
| Request | javax.servlet.ServletRequest | 현재 페이지가 요청될 때 | 다른 페이지로 이동할 때 | 현재 페이지, 포워딩의 경우 다음 페이지까지 참조 가능 |
| Session | javax.servlet.http.HttpSession | 클라이언트가 서버에 접속할 때 | 일정 시간이 지나거나 브라우저가 종료될 때 | 동일 클라이언트에 대해 다른 페이지에서도 참조 가능 |
| Web Context | javax.servlet.ServletContext | 웹 애플리케이션이 시작될 때 | 웹 애플리케이션이 종료될 때 | 모든 클라이언트에서 참고 가능 |

- Request와 Session을 주로 활용하게 되며 모든 사용자가 공유하거나 웹 애플리케이션 전체에서 참조가 필요한 경우 Web Context를 사용할 수 있음.
- 이러한 객체는 속성을 저장하고 참조하기 위해 다음 메서드가 공통적으로 제공됨

```java
setAttribute(String name, Object o) // 속성 저장
Object getAttribute(String name) // 속성 참조
```

- 주의할 점) 저장하고자 하는 데이터가 Object 형태임
  - Object 타입은 데이터를 가지고 올 때 리턴된 Object 타입을 원래 저장된 데이터 타입으로 변환해야 함

```java
String name = "홍길동";
request.setAttribute("name", name);
String rname = (String) request.getAttribute("name");
```

### 페이지 이동과 정보 공유 시나리오

- 사례 1: 로그인

  - 로그인 후 세션을 이용해 사용자 이름을 저장하고 메인 화면으로 이동하는 경우

  1. 클라이언트가 로그인한다.
  2. 컨트롤러는 request.getParameter()를 통해 클라이언트와 id와 password를 확인한다.
  3. 로그인 정보가 맞을 경우 사용자 이름이나 기타 정보를 세션에 저장한다.
  4. 메인 화면으로 리디렉션한다.

  ```java
  // 컨트롤러(controller)
  session.setAttribute("uname", "홍길동"); // 이름을 세션에 저장
  response.sendRedirect("/man.jsp"); // 메인 화면으로 리디렉션

  // main.jsp
  <h2>${uname}</h2> // 이름 출력
  ```

- 사례 2: 게시판 목록

  - 데이터베이스 연동으로 리스트 형태의 데이터를 저장하고 JSP에서 사용하는 경우

  1. 컨트롤러는 DB로부터 게시판의 첫 번째 페이지 데이터를 가지고 온다.
  2. request에 리스트 형태로 데이터를 저장한다.
  3. 목록 화면으로 포워딩한다.

  ```java
  // 컨트롤러(controller)
  List<Member> mlist = dao.getMemberList();
  request.setAttribute("mlist", mlist); // 리스트 형태로 데이터 저장
  request.getRequestDispatcher("/mlist.jsp").forward(request, response);
  ```

## Section 04 [실습 5-1] 서블릿 프로그래밍: Hello World!

### 서블릿 생성

- 이번 실습의 개요
  - 이클립스를 이용해 서블릿 생성
  - 기본 서블릿의 구조
  - 서블릿 코드의 기본 구조와 구현 방법
  - 톰캣을 통한 실행 방법

1. 2장에서 생성한 [jwbook Dynamic Web Project]에서 서블릿을 생성. 서블릿은 자바 코드로 작성하기 때문에 프로젝트의 [src/main/java] 폴더에서 마우스 오른쪽 버튼을 클릭하고 <New> -> <Servlet>을 선택

2. 서블릿 클래스 정보를 입력. 패키지와 클래스 이름을 입력하고 나머지는 기본값으로 두고 <Next>를 클릭.

- Java Package: ch05
- Class name: HelloWorld
- Superclass: 기본적으로 javax.servlet.http.HttpServlet으로 선택되어 있음

  - 필요에 따라 커스텀 구현된 서블릿을 사용할 수도 있음

- 서블릿 세부 정보 입력

3. [Create Servlet] 창에 정보 입력. 서블릿과 관련된 정보를 입력하는 창으로 web.xml 혹은 애너테이션에 포함될 서블릿 정보를 나타냄.

- <Description>: 서블릿에 대한 설명을 간단하게 기술하기 위해 'My first servlet' 입력
- <Initialization parameters>: 서블릿에 전달될 초깃값이나 설정값을 작성
- <URL mappings>: 서블릿을 호출하기 위한 URL을 지정
  - 여러 URL을 /, \*, 확장자 등과 결합하여 패턴 형식으로 지정할 수도 있음

* 추가 설정과 메서드 생성

4. 서블릿 생성의 마지막 단계에서 Modifier 지정과 인터페이스 추가가 가능하며, 그 외 기본적으로 생성할 메서드를 선택하는 부분이 있음. 'Constructors from superclass', 'Inherited abstract methods', 'doGet', 'doPost'는 기본적으로 선택되어 있는 것을 확인한 다음 <Finish>를 클릭하여 서블릿 생성을 마무리

5. HelloWorld 서블릿에 기본 생성된 코드를 보면 앞에서 설정한 내용이 모두 반영됨

- @WebServlet 애너테이션

6. 현재 클래스가 서블릿 클래스라는 것을 컨테이너에 알리기 위해 @Web Servlet 애너테이션이 사용됨. 그리고 어떤 요청에 의해 서블릿을 호출할 것인지 결정하는 Url mapping 부분도 urlPatterns 속성을 지정되어 있음

```java
@WebServlet(description = "My first servlet", urlPatterns = {"/hello"})
public class HelloWorld extends HttpServlet {}
```

- 만약 (2번) 과정에서 Description 없이 패턴만 지정한다면 @WebServlet("/hello")와 같이 단순화하는 것도 가능함

- doGet()

7. GET 요청을 처리하는 메서드로, request, response를 인자로 함

- ServletException과 IOException을 throws하고 있기 때문에 호출하는 쪽에서 예외 처리 해야 함

```java
protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
    response.getWriter().append("Served at: ").append(request.getContextPath());
}
```

- doGet() 안에 생성된 코드를 지우고 다음과 같이 수정하기

```java
protected void doGet(HttpServletRequest request, HttpServletResponse response)
response.setContentType("text/html; charset=utf-8");
PrinterWriter out = response.getWriter();
out.append("<!doctype html><html><head><title>Hello World Servlet</title></head><body>")
    .append("<h2>Hello World !! </h2><hr>")
    .append("현재 날짜와 시간은: "+java.time.LocalDateTime.now())
    .append("</body></html>");
```

- doPost()

8. POST 요청을 처리하는 메서드로 단순히 doGet()을 호출하도록 되어 있음

- REST API 구현이 아닌 일반 서블릿 구현이라면 GET, POST를 내부적으로 동일하게 처리함.
- 물론 GET, POST를 구분해 처리해야 하는 경우라면 당연히 별도의 코드로 구성할 수도 있음.

```java
protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
  doGet(request, response);
}
```

9. 완성된 코드 실행하는 방법

1) 서블릿 클래스를 선택한 다음 실행 버튼을 누르기
2) 마우스 오른쪽 버튼을 클릭하고 [Run as] -> [Run on Server]를 클릭하여 실행하기

- JSP와 마찬가지로 서버 런타임 선택 화면에서 프로젝트 생성 시 등록한 'Tomcat v9.0'을 선택하고 실행

## Section 04 [실습 5-2] 서블릿 프로그래밍: 계산기 구현

- 이번 실습의 개요

  - 2개의 숫자와 연산자를 선택하고 실행 버튼을 누르면 입력값을 서블릿으로 전달
  - 서블릿은 브라우저로부터 전달된 입력값을 가져와 계산 후 결과를 포함한 화면을 출력

- 입력 파라미터
  - n1, n2: 숫자 입력. HTML <input>으로 구현함
  - op: 연산자 선택 드롭다운 리스트로 HTML <select>로 구현함
    - 연산자의 종류: +, -, \*, /

1. [webapp] 폴더에서 [ch05] 폴더를 만들고 'calcForm.html' 파일을 생성

2. 계산기 서블릿은 [실습 5-1]의 HelloWorld 서블릿 생성과 마찬가지로 이클립스를 통해 다음과 같이 생성함. 계산기 구현은 doGet() 메서드 내부에서 구현함

- Java Package: ch05
- Class name: CalcServlet
- Url mapping: /calc

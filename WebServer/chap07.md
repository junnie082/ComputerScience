# Web Server 7

## Web Server Programming

#### 2024.11.24.(일)

## Chapter 07 JSP 응용

### 목차

1. 액션 태그
2. 실습 7-1 액션 종합 예제: 계산기 구현
3. 커스텀 태그와 EL
4. JSTL

### 학습목표

- 액션 태그와 자바 빈의 개념을 익힌다.
- useBean 액션 활용 방법을 익힌다.
- JSTL과 EL 종합 예제를 통해 사용법을 익힌다.
- 빌드 도구를 이해하고 Maven 기반으로 프로젝트를 전환한다.

## Section 01 액션 태그

### 액션 태그란?

- 액션(Action) 태그
  - JSP에서 객체 생성과 공유, 페이지 이동과 전달, 태그 파일 작성 등에 필요한 기능을 제공하는 일종의 커스텀 태그
  - 표준 액션이라고도 불리며 커스텀 태그 기반이지만 별도의 taglib 지시어 사용 없이 jsp 접두어를 사용함
  - JSP에서 프로그램적인 요소를 많이 구현하거나 컨트롤러로 활용할 때 유용함
  - JSP 파일에서 커스텀 태그의 구조적인 특징을 살려 HTML 형태로 프로그램 요소를 처리할 수 있기 때문에 간편함

| 액션 태그 | 설명 |
| jsp:forward | request와 response 객체를 포함해 다른 페이지로 포워드함 |
| jsp:include | 다른 페이지의 실행 결과를 포함시킴 |
| jsp:useBean | 자바 빈즈 객체를 생성하거나 불러옴 |
| jsp:setProperty | 자바 빈즈 객체의 속성(멤버 변수)에 값을 할당함 |
| jsp:getProperty | 자바 빈즈 객체의 속성값을 출력함 |
| jsp:param | include, forward 액션 사용 시 파라미터 값을 수정하거나 추가함 |

### 자바 빈

- 자바 빈(Java Bean)

  - 자바의 재활용 가능한 컴포넌트 모델을 의미함,
  - 웹 개발에만 국한된 개념이 아니면 POJO라고 하는 단순한 구조를 가짐
    - POJO(Plain Old Java Object): 특정 기술이나 프레임워크에 종속하지 않고 기본 생성자와 멤버 변수에 대한 getter/setter 메서드를 제공하고 직렬화할 수 있는 자바 클래스를 의미함
  - 예) 엔티티 클래스 혹은 DO(Data Object)는 기본적으로 테이블 칼럼에 해당하는 private 멤버 변수와 getter/setter 메서드로 구성할 수 있음

- 자바 빈 구조의 특징

  - 인자가 없는 생성자(기본 생성자)로 구성됨
  - 파일 혹은 네트워크를 통해 객체를 주고받을 수 있는 직렬화 구조가 가능함
  - getter, setter 메서드를 통해 멤버 변수(속성)에 접근함

- 회원 관리를 위한 Member 클래스를 자바 빈 구조
  - getter/setter 메서드의 이름: 'get/set + 대분자로 시작되는 변수명'의 형태
  - 인자를 받아 현재 멤버 변수에 값을 대입하거나 현재 값을 리턴하는 구조
    - 물론 메서드 내에서 추가적으로 필요한 기능을 구현할 수 있으며 이러한 구조는 객체지향에서 캡슐화의 한 형태이기도 함

```java

class Member {
    private int id;
    private String name;
    private String email;
    ...
    public void setId(int id) {
        this.id = id;
    }
    public int getId() {
        return id;
    }
}
```

### useBean 액션

- useBean 액션

  - JSP에서 자바 빈 객체를 생성하거나 참조하기 위한 액션
  - 매우 유용하지만 JSP를 단순히 뷰 역할로만 사용한다면 사용할 일은 없음

- useBean의 기본적인 동작 방식

  1. useBean을 이용해 만든 객체의 범위는 지정하는 속성인 scope에 주어진 id의 객체가 있는지 확인한다.
  2. 객체가 없다면 새로 객체를 생성하고 해당 scope에 저장한다.

- useBean 액션의 기본 구문과 사용 예

```jsp
<jsp:useBean id="instanceName" scope="page | request | session | application"
class="packageName.className" type="packageName.className"
beanName="packageName.className">
</jsp:useBean>
```

- id: 자바 빈을 특정 scope에 저장하거나 가져올 때 사용하는 이름

  - 현재 페이지에서는 해당 인스턴스를 참조하기 위한 변수명이 됨

- scope: 해당 클래스 타입의 객체를 저장하거나 가지고 오는 범위로 내장객체의 일부

- class: 생성하거나 참조하려는 객체의 클래스명

  - 반드시 패키지명까지 명시해야 하며, 추상클래스나 인터페이스는 사용할 수 없음

- type: 특정 타입의 클래스를 명시할 때 사용함

  - 추상 클래스나 인터페이스, 일반 클래스가 될 수 있으며 class 속성의 클래스에서 상속 혹은 구현이 이루어져야 함

- beanName: type과 beanName 사용을 통해 class 속성을 대체할 수 있음

- useBean을 주로 활용하는 경우
  - HTML 폼에서 입력한 값을 자바 객체로 연동할 때 주로 활용함
  - 예) 회원가입 페이지에서 아이디, 이름, 전화번호, 주소 등 여러 정보를 입력하고 가입하는 경우
    - 입력값을 받아 Member 객체에 넣고 이를 데이터베이스에 저장하기 위한 메서드 호출에 인자로 전달해야 함

```jsp
<jsp:useBean id="m" class="com.my.Member" />
<jsp:setProperty name="m" property="*" />

<%
    MemberDAO dao = new MemberDAO();
    dao.insertDB(m);
%>
```

### useBean의 활용

> 서블릿으로 처리하기

물론 서블릿으로도 회원가입 처리를 구현할 수 있다. 하지만 모든 값을 개발자가 직접 읽어와 객체에 할당해야 하기 때문에 불편하다.

```java
doGet(...) {
    Member m = new Member();
    m.setId("testuser");
    m.setName("테스트 사용자");
    m.setEmail("test@test.com");
    ...
    dao.insertDB(m);
}
```

이와 같이 불편한 부분은 Apache Commons BeanUtils 라이브러리로 해결이 가능하다. 서블릿에서 BeanUtils를 사용한 코드는 다음과 같다.

```java
doGet(...) {
    Member m = new Member();
    BeanUtils.populate(m, request.getParameterMap());
    ...
    dao.insertDB(m);
}
```

### include, forward 액션

- include 액션
  - 다른 페이지를 포함한다는 점에서 include 지시어와 동일하지만 처리 과정에서 차이가 있음
    - include 지시어: include된 파일 구조를 모두 포함해 하나의 파일로 컴파일한 다음 처리
    - include 액션: include된 파일을 각각 호출해 처리된 결과만 포함해 보여줌
  - include 액션을 사용하여 'main.jsp'dp 'header.jsp'를 포함시킨 경우
    - 'main.jsp'를 호출하면 'header.jsp'의 실행 결과가 포함되어 출력됨

```jsp
// main.jsp
<jsp:include page="header.jsp">
    <jsp:param name="title" value="My Homepage" />
</jsp:include>
```

```jsp
// header.jsp
<h2> <%= request.getParameter("title") %></h2>
```

- forward 액션
  - 클라이언트 요청을 다른 페이지로 전환하는 액션
  - 리디렉션(response.sendRedirect())과 기능적으로 유사하지만 내부적으로는 차이가 있음
    - 리디렉션: 서버가 클라이언트에게 새로운 페이지로 다시 접속하도록 응답을 보내고, 응답을 받은 클라이언트가 다시 새로운 페이지로 접속하는 방식
    - forward 액션: 클라이언트가 새롭게 접속하는 것이 아니라 서버에서 내부적으로 새로운 페이지로 이동하고 그 페이지의 내용을 클라이언트에게 응답으로 전달하는 방식
  - 단순한 페이지 이동이 필요한 경우 - 리디렉션이 적합
  - 최초 request를 유지하거나 request의 setAttribute()로 속성값을 저장하고 이를 유지하면서 페이지를 이동하는 경우 - forward 액션이 적합
  - forward 액션
    - include 액션과 마찬가지로 파라미터의 추가가 가능함

```jsp
// main.jsp
<jsp:forward page="result.jsp">
    <jsp:param name="title" value="My Homepage" />
</jsp:include>
```

```jsp
// result.jsp
<h2><%= request.getParameter("title") %></h2>
```

## Section 02 [실습 7-1] 액션 종합 예제: 계산기 구현

### 실습 7-1 액션 종합 예제: 계산기 구현

- 이번 실습의 흐름

1. 계산기 화면은 기존 파일을 재활용하여 구현한다.
2. 자바 빈 객체를 생성한다.
3. useBean 액션으로 입력값을 처리한다.

### 계산기 화면 구현

```java

package ch07;

public class Calculator {
    private int n1;
    private int n2;
    private String op;

    public long calc() {
        long result = 0;
        switch(op) {
            case "+": result = n1+n2; break;
            case "-": result = n1-n2; break;
            case "/": result = n1/n2; break;
            case "*": result = n1*n2; break;
        }
        return result;
    }
}

```

```jsp

<%@ page language="java" contentType="text/html; charset=UTF-8" pageEncoding="UTF-8"%>

<jsp:useBean id="calc" class="ch07.Calculator" />
<jsp:setProperty name="calc" property="*" />

<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>계산기-useBean</title>
</head>
<body>
    <h2>계산 결과-useBean</h2>
    <hr>
    결과: <%=calc.calc() %>
</body>
</html>

```

## Section 03 커스텀 태그와 EL

### 커스텀 태그란?

- 커스텀 태그(Custom Tag)

  - 사용자 정의 태그를 의미함
  - 스크립트릿 사용을 줄이고 태그와 같은 형태로 프로그램 코드를 대체하거나 재활용 가능한 구조를 통해 태그 라이브러리로 활용하고자 개발된 규격임
  - 외형적인 형태는 XML(HTML) 태그 구조이지만 서블릿 형태로 변환될 때 자바 코드로 변경되어 통합되는 방식
  - 커스텀 태그를 사용하기 위해서는 taglib 지시어를 사용하여 커스텀 태그가 어디에 정의되어 있는지를 먼저 선언해야 하며 태그에 사용할 접두어를 지정해야 함
  - 커스텀 태그 자체가 서버에서 해석되는 구조이며, 프로젝트가 특정 커스텀 태그에 종속될 수 있다는 문제 때문에 커스텀 태그를 직접 만드는 방식은 점차 줄어들고 있음
    - 대신 커스텀 태그 기술로 만들어진 JSTL(JSP Standard Tag Library)이 자바 웹 개발에 꼭 필요한 요소가 됨

- 커스텀 태그의 예시

  - 특정 상품 코드를 전달하면 해당 상품에 대한 세부 정보를 출력하기

  ```jsp
    <%@ taglib tagdir="/WEB-INF/tags" prefix="m" %>
    <m:printData pid="87459989" />
  ```

  - 태그 파일로 정의된 커스텀 태그를 사용하며 'WEB-INF/tags/printData.tag' 파일로부터 태그 정의를 가지고 옴
  - m은 태그 앞에 붙일 접두어로 태그 파일명이 태그 이름이 됨

> 표준 액션과 커스텀 태그

커스텀 태그는 태그 구조로 기능을 구현하는 방식으로 개발자가 필요에 따라 커스텀 태그를 구현해 프로그램에 활용할 수 있다. 앞에서 배운 <jsp:useBean> 또는 <jsp:setProperty> 역시 커스텀 태그로 jsp에 기본적으로 포함되어 있어 표준 액션이라고 부르며 taglib 지시어 없이 바로 사용할 수 있다.

### EL이란?

- 표현 언어(Expression Language, EL)

  - 주로 EL이라 불림
  - 현재 페이지의 자바 객체 혹은 scope object에 저장된 자바 빈 객체를 손쉽게 접근하고 사용할 수 있게 함
  - 데이터를 표현하기 위한 용도로 설계되었지만, 제한된 객체 참조가 가능하며 해당 객체의 메서드 호출도 가능함
  - EL은 단순한 출력 외에도 사칙연산, 비교연산, 논리연산, 3항 연산 등을 지원함
    - 연산 기능은 핵심 로직의 구현보다는 상황에 따라 출력값을 변경하는 정도의 용도로 사용하는 것이 좋음

- EL의 장점

  - 간단한 구문으로 손쉽게 변수/객체를 참조할 수 있음
  - 데이터가 없거나 null 객체를 참조할 때 에러가 발생하지 않음

- 자바 빈 접근

  - EL을 통해 scope object에 저장된 자바 빈 객체를 참조하는 방법

  ```jsp
    ${저장이름.변수명}
  ```

  - 앞의 자바 빈 설명에서 만든 Member 클래스의 멤버 정보에 접근하기
    - 컨트롤러에 의해 session에 저장하는 과정이 있었다고 가정함

  ```jsp
    <h2>멤버 정보</h2>
    이름: ${m.name} <br>
  ```

- EL을 사용하지 않을 경우

  - 다음과 같이 표현식을 사용하거나 <jsp:getProperty> 액션으로 출력할 수도 있음

  ```jsp
  이름: <%= m.name %> <br> // 표현식 사용
  이름: <jsp:getProperty name="m" property="name" /> <br> // 액션 사용
  ```

- EL 연산

  - 기본적인 사칙연산, 비교연산, 논리연산, 3항 연산 등이 가능함

  ```jsp
    ${10 + 20} // 사칙연산, 30
    ${10 * 20} // 사칙연산, 200
    ${ture && false} // 논리연산, false
    %{10 >= 20} // 논리연산, false
    %{user.name == "홍길동"? "교수": "학생"} // 3항 연산, 이름이 홍길동이면 교수 출력
  ```

- 배열, 맵 데이터 연동

  - 참조하는 객체가 배열이나 맵 형태인 경우 다음과 같이 사용함

  ```jsp
  ${myList[0]} // 배열인 경우
  ${myMap["name"]} // 맵인 경우
  ```

- Scope Object 접근

  - EL은 기본적으로 모든 scope에서 자바 빈 객체를 찾음
  - 만일 특정 scope만을 대상으로 참조하려면 '내장객체명 Scope.속성이름'으로 사용

    - 예) session과 request 모두에 'm'이라는 이름으로 저장된 객체가 있다고 할 때, request scope에 있는 객체를 참조하려면 다음과 같이 사용할 수 있음

    ```jsp
    이름: ${requestScope.m.name}
    ```

## Section 04 JSTL

### JSTL이란?

- JSTL(JSP Standard Tag Library)

  - JSP에서 스크립트릿(자바 코드 블록)을 사용하지 않고 HTML 형식을 유지하면서 조건문, 반복문, 간단한 연산과 같은 기능을 손쉽게 사용할 수 있도록 지원하기 위해 만들어진 표준 커스텀 태그 라이브러리
  - 서버에서만 해석할 수 있는 구조로 인해 디자이너와의 협업에 불편한 부분이 있음
  - 개발 과정에서 UI 확인을 위해 서버를 통해야만 하는 비효율적인 문제가 있음
    - 따라서 모바일 환경 중심의 프런트엔드 개발 트렌드와는 다소 거리가 있음
  - 뷰 중심의 JSP 구현에는 core 정도만 사용됨

- JSTL 사용하기
  - JSTL을 JSP에서 사용하려면 taglib 지시어를 추가해야 함
  - 다음에 나올 core 라이브러리 사용을 위해서는 다음과 같이 작성함

```jsp
<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
```

### core 라이브러리

- core 라이브러리
  - 변수 처리, 흐름 제어, URL 관리, 출력 등 가장 기본적인 기능을 구현해둔 라이브러리

| 기능 | 태그 | 사용 예 |
| 변수 관련 | remove, set | 변수 지정과 삭제 |
| 흐름 제어 | if, choose _ when _ otherwise, forEach, forTokens | 조건 처리, 반복, 토큰 파싱 |
| URL 관리 | import, redirect,, url, param | URL 핸들링 |
| 기타 | catch, out | 에러 처리, 출력 |

- <c:if>

  - 자바의 if 문과 유사하지만 else는 지원하지 않음
  - 조건 테스트를 위해 속성을 참조할 범위(Scope)를 지정할 수도 있지만 필수 사항은 아님
  - 사용 형식

  ```jsp
  <c:if test="조건" [var="결과 변수"][scope="{page|request|session|application}"]>
    조건이 참(true)인 경우 출력되는 부분
  </c:if>
  ```

  - 사용 예시

  ```jsp
  <c:if test="${msg == 'user1'}" var="result">
   test result : ${result}
  </c:if>
  ```

- <c:forEach>

  - 화면에 데이터를 반복해서 출력할 때 주로 사용함
  - 자바의 for 문과 같은 개념이지만 커스텀 태그 특성상 정밀한 설정이 가능하지 않기 때문에 제공되는 속성을 잘 활용해야 함
  - 진행 상태를 확인하기 위해 index, count 등을 지원하는 varStatus를 제공함
  - 사용 형식

  ```jsp
    <c:forEach [var="참고 객체"] [varStatus="상태 정보 변수"] begin="시작" end="종료" [step="반복 단계 증가 값, 1이 기본"]>
    반복 출력되는 부분
    </c:forEach>
  ```

  - var: 배열, 리스트 등 집합형 객체
  - varStatus: 반복 진행 상황을 참조하기 위한 객체
  - 사용 예시

  ```jsp
    <c:forEach var="m" items="${members}" begin="0" varStatus="status" end="5">
    index: ${status.index} /
    count: ${status.count} <BR>
    name: ${m.name} <BR>
    email: ${m.email} <BR>
    <HR>
    </c:forEach>
  ```

## Section 05 [실습 7-2] JSTL과 EL 종합 예제

### 실습 7-2 JSP 생성

- 이번 실습의 개요
  - JSTL과 EL을 활용한 종합 예제
  - 실습을 통해 기본 JSTL 태그와 EL을 화면 구성에 활용할 수 있음
  - 이번 실습을 따라 하기 위해서는 JSTL 라이브러리가 설치되어 있어야 함
    - 만약 설치하지 않았다면 JSTL을 설치하고 앞의 실습을 진행해야 함

```jsp
// jstlExam.jsp
<%@ page language="java" contentType="text/html; charset=UTF-8" pageEncoding="UTF-8" %>
<%@ taglib uri="http://java.sun.com/jsp/jstl/core" prefix="c" %>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>JSTL 종합 예제</title>
</head>
<body>
    <h2>JSTL 종합 예제<h2>
    <hr>
</body>
</html>
```

### 실습 7-2 <c:set>, <c:out>

- <c:set>
  - 특정 scope에 값을 저장하는 기능
    - scope 내장객체의 setAttribute() 메서드를 사용하는 것과 동일함
  - 기본적으로 문자열 형태로 저장하지만 EL을 사용할 경우 배열과 같은 객체도 저장할 수 있음
  - target 속성으로 특정 타입의 객체에 setter 메서드를 통한 속성 저장도 가능함

product1, product2에 문자열을 저장하고 intArray에는 EL을 이용하여 배열을 저장함

- <c:set> intArray 배열 선언 부분에 버그로 에러가 표시되지만 실행에는 지장이 없으니 무시함!

```jsp
<h3>set, out</h3>
<c:set var="product1" value="<b>애플 아이폰</b>" />
<c:set var="product2" value="삼성 갤럭시 노트" />
<c:set var="intArray" value="${[1,2,3,4,5]}" />
```

- <c:out>
  - 출력을 위한 태그로 대부분은 EL로 대체됨
    - 다만 때에 따라서는 조건문 사용을 줄이는 용도로 활용할 수 있음

product1, intArray 출력

```jsp

<p>
    product1(jstl);
    <c:out value="${product1}" default="Not registered" escapeXml="true" />
</p>

```

- default: 출력하고자 하는 객체가 없을 때 출력할 값을 의미함

  - 예) product1을 찾지 못할 경우 Not registered를 대신 출력함

- escapeXml: true인 경우 태그를 일반 문자열로 처리함.

  - 기본값: false, 태그가 반영되어 출력됨

  ```jsp
    <p>product1(el): ${product1}</p>
    <p>intArray[2]: ${intArray[2]}</p>

    <hr>
  ```

  - <c:set>으로 저장된 값은 EL로 출력할 수 있으며 가장 선호되는 방법임
  - 배열 역시 EL을 이용해 특정 인덱스 값을 출력할 수 있음
  - <c:forEach>
    - JSTL에서 가장 널리 사용되는 태그 중 하나임

<c:set>을 이용해 선언했던 배열값을 출력해보기

- varStatus 속성을 이용해 index 값을 함께 출력함

```jsp
  <!-- forEach -->
  <h3>forEach: 배열 출력</h3>
  <ul>
      <c:forEach var="num" varStatus="i" items="${intArray}">
          <li>${i.index} : ${num}</li>
      </c:forEach>
  </ul>

  <hr>
```

- <c:forEach>: 자바의 for-in 구조와 유사한 형태임
- ${i.index}: 0부터 반복을 시작함
  - 만약 1부터 반복 횟수를 구하고 싶다면 ${i.count}를 사용함

### 실습 7-2 <c:if>

- <c:if>
  - <c:forEach>와 함께 JSTL에서 가장 널리 사용되는 태그 중 하나임
  - 특정 조건에만 태그 보디 부분이 수행되는 단순한 구조라 복잡한 조건 체크에는 적합하지 않음

변수 checkout 값에 따라 조건을 체크함

- checkout이 true인 경우: <c:set>에서 설정했던 product2의 정보를 출력
- checkout이 false인 경우: "주문 제품이 아님!!" 메세지를 출력

```jsp

<!-- if -->
<h3>if</h3>
<c:set var="checkout" value="true" />
<c:if test="${checkout}">
  <p>주문 제품: ${product2}</p>
</c:if>
<c:if test="${!checkout}">
  <p>주문 제품이 아님!!</p>
</c:if>

<c:if test="${!empty product2}">
  <p>
    <b>${product2} 이미 추가됨!!.</b>
  </p>
</c:if>

```

### 실습 7-2 <c:choose>, <c:when>, <c:otherwise>

<c:if>와 동일한 조건을 <c:choose>, <c:when>, <c:otherwise>를 이용해 구현

```jsp
<!-- choose, when, otherwise -->
<h3>choose, when, otherwise</h3>
<c:choose>
  <c:when test="${checkout}">
    <p>주문 제품: ${product2}</p>
  </c:when>
  <c:otherwise>
    <p>주문 제품이 아님!!</p>
  </c:otherwise>
</c:choose>
<hr>
```

### <c:forTokens>

- <c:forTokens>
  - 자바의 StringTokenizer와 유사하게 구분자로 문자열을 나누는(파싱하는) 태그
  - 출력할 문자열이 구분자로 분리되어 있을 때 모든 값을 반복해서 출력하는 경우에 유용

다음은 '|'로 구분된 도시 이름을 <c:forTokens>를 이용해 출력함

```jsp
<!-- forTokens -->
<h3>forTokens</h3>
<c:forTokens var="city" items="Seoul|Tokyo|New York|Toronto" delims="|" varStatus="i">
    <c:if test="${i.first}">도시 목록 : </c:if>${city}
    <c:if test="${!i.last}">,</c:if>
</c:forTokens>
<hr>
```

```jsp
// jstlExam.jsp

<%@ page language="java" contentType="text/html; charset=UTF-8" pageEncoding="UTF-8"%>
<%@ taglib uri="http://java.sun.com/jsp/jstl/core" prefix="c" %>
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>JSTL 종합 예제</title>
</head>
<body>
    <h2>JSTL 종합 예제</h2>

    <hr>
    <h3>set, out</h3>
    <c:set var="product1" value="<h2>애플 아이콘</h2>" />
    <c:set var="product2" value="삼성 갤럭시 노트" />
    <c:set var="intArray" value="${[1,2,3,4,5]}" />
    <p>
        product1(jstl);
        <c:out value="${product1}" default="Not registered" escapeXml="true" />
    </p>
    <p>product1(el); ${product1}</p>
    <p>intArray[2]; $(intArray[2])</p>
    <hr>

    <h3>forEach: 배열 출력</h3>
    <ul>
      <c:forEach var="num" varStatus="i" items="${intArray}">
        <li>${i.index} : ${num}</li>
      </c:forEach>
    </ul>
    <hr>

    <h3>if</h3>
    <c:set var="checkout" value="true" />
    <c:if test="${checkout}">
      <p>주문 제품: ${product2}</p>
    </c:if>
    <c:if test="${!checkout}">
        <p>주문 제품이 아님!!</p>
    </c:if>

    <c:if test="${!empty product2}">
        <p>
          <b>${product2} 이미 추가됨!!.</b>
        </p>
    </c:if>
    <hr>

    <h3>choose, when, otherwise</h3>
    <c:choose>
      <c:when test="${checkout}">
        <p>주문 제품: ${product2}</p>
      </c:when>
      <c:otherwise>
        <p>주문 제품이 아님!!</p>
      </c:otherwise>
    </c:choose>
    <hr>

    <h3>forTokens</h3>
    <c:forTokens var="city" items="Seoul|Tokyo|New York|Toronto" delims="|" varStatus="i">
        <c:if test="${i.first}">도시 목록 : </c:if>
        ${city}
      <c:if test="${!i.last}">,</c:if>
    </c:forTokens>

    <hr>
</body>
</html>
```

## Section 06 Maven 기반 프로젝트 구성

### 빌드 도구

- 자바 빌드 도구
  - Ant: 가장 오래된 자바 빌드 도구
  - Maven: 2004년에 아파치 프로젝트로 Maven이 새롭게 나옴
    - 이후 오랜 기간 Maven은 절대 다수가 사용하는 자바 빌드 도구가 되었으며 특히 스프링 프레임워크 개발에서 기본 빌드 도구로 활용됨
  - Gradle: 시간이 지남에 따라 좀 더 유연하면서도 복잡한 처리를 쉽게 하기 위한 요구 사항의 증대로 2012년 Gradle이 나옴
    - 안드로이드 앱 개발의 기본 빌드 도구가 되었음
  - 현재 Maven과 Gradle이 가장 대표적인 빌드 도구임

### Maven과 Gradle

- Maven과 Gradle의 설정 방식

  - Maven: 빌드 설정을 'pom.xml' 파일에 작성함
    - XML 구조이기 때문에 프로젝트가 커질수록 스크립트의 내용이 길어지고 가독성이 떨어지는 문제가 있음
  - Gradle: Groovy라고 하는 JVM 기반 언어를 통해 프로그램 구조로 설정함
    - 따라서 훨씬 적은 양의 스크립트로 짧고 간결하게 작성할 수 있음

- 다중 프로젝트
  - Maven: 다중 프로젝트에서 특정 설정을 다른 모듈에서 사용하려면 상속을 받아야 함
  - Gradle: 설정 주입 방식을 사용하여 다중 프로젝트에 적합함

### 레포지터리

- 개발환경

  - 안드로이드 프로젝트는 기본적으로 Gradle을 사용함
  - 스프링 프레임워크 기반 프로젝트는 Maven, Gradle 중에 선택할 수 있음
  - 이클립스는 Maven에 친화적이고 IntelliJ는 Gradle에 친화적임

- 빌드 도구를 사용하는 주요 목적 두 가지

1. 컴파일/실행 설정과 라이브러리 설정

- 이중에서도 라이브러리 설정은 꼭 알아두어야 함

2. 꼭 필요한 핵심 라이브러리만 설정 파일에 등록해두면 해당 라이브러리에서 필요로하는 다른 라이브러리는 자동으로 함께 설치되기 때문에 신경 쓸 필요가 없음

- 레포지터리
  - 이러한 라이브러리를 통합 보관하는 일종의 저장소임
  - 설정 파일의 내용을 참고해 해당 라이브러리를 글로벌 저장소로부터 로컬 저장소로 다운로드한 다음 프로젝트에 복사하는 과정을 거쳐 사용함
  - 해당 라이브러리가 로컬 저장소에 있다면 인터넷에서 다운로드하지 않고 바로 사용할 수 있음
  - 필요에 따라서는 개발 회사가 자신들에게 필요한 라이브러리만 저장하거나 자체 라이브러리 저장을 위한 저장소를 두고 사용하기도 함

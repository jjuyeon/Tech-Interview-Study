# :question: Spring MVC

#### reference
https://onejuny.tistory.com/entry/JavaJsp-MVC-1-MVC-2-%EC%B0%A8%EC%9D%B4-%EB%B0%8F-%EC%9E%A5%EB%8B%A8%EC%A0%90<br>
https://chanhuiseok.github.io/posts/spring-3/ <br>
https://catsbi.oopy.io/f52511f3-1455-4a01-b8b7-f10875895d5b
<hr>

## Question
1. [MVC1과 MVC2 패턴의 차이를 설명해 주세요.](#2-mvc1-)
- MVC1은 모든 클라이언트의 요청과 응답을 JSP가 담당하는 구조로, View와 Controller 모두 JSP가 담당합니다.
- 반면, MVC2는 MVC1의 단점인 유지보수의 어려움을 보완하기 위해 나온 모델로, 클라이언트의 요청, 응답, 비즈니스 로직을 처리하는 부분을 모듈화시킨 구조입니다. JSP는 View의 역할만 하고 Servlet이 Controller를 담당합니다.
<br>

1. (꼬리질문) [그렇다면, Spring MVC 는 무엇일까요?](#4-spring-mvc-)
- 기존의 JSP & Servlet 기반 MVC 패턴(MVC1, MVC2)에서 Model, View, Controller로 분할했었습니다.
- 그에 비해 Spring MVC는 Dispatcher Servlet, Handler Mapping, ViewResolver를 추가하며 역할을 더 명확히 분리하고, Restful API를 만드는 데 더 특화되어 있습니다.

<hr>

## :nerd_face:	What I study

### 1. MVC ?
- `Model, View, Controller` 의 줄임말
- 비즈니스 로직과 UI를 분리하여 유지보수를 용이하게 한다.
- Model: 어플리케이션의 데이터, 뷰가 랜더링하는데 필요한 데이터
- View: 사용자 인터페이스 요소, 클라이언트에게 실제로 보이는 화면, 모델로부터 정보를 얻고 표시
- Controller: 데이터와 비즈니스 로직 간의 상호 작용을 관리, view와 model이 직접적인 상호 소통을 하지 않도록 관리

<br><br>

### 2. MVC1 ?
![mvc1](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FI1UUx%2FbtqEZ0IhZ6O%2FqzvwssYAAkEltNqYd3Kpik%2Fimg.png)
- 모든 클라이언트의 요청과 응답을 JSP가 담당하는 구조
- View와 Controller 모두 JSP가 담당한다.
- 하나의 JSP 페이지에서 controller는 자바, view는 html, css는 자바스크립트를 사용한다.
- model은 jdbc 인터페이스로 DB를 조작하면서 클래스를 정의한다.
- 규모가 작고 유지보수가 적은 경우 사용한다.

#### 2-1) 장점
- 페이지의 흐름이 단순하고 구조가 간단하다.
- JSP 하나로 유저의 요청을 받고 응답을 처리하므로 구현 난이도가 쉽다.
#### 2-2) 단점
- JSP 하나에서 MVC가 모두 이루어지므로 
- 1) 재사용성이 매우 떨어진다.
- 2) 가독성도 떨어진다.
- 3) 유지보수가 어려워 웹 규모가 커질수록 복잡해진다.

<br><br>

### 3. MVC2 ? 
![mvc2](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Fbgbhsb%2FbtqE0BOnWqs%2FGhoRjVi90P1dYSBjRHSo91%2Fimg.png)
- 클라이언트의 요청, 응답, 비즈니스 로직을 처리하는 부분을 모듈화시킨 구조
- MVC1의 유지보수가 어렵다는 단점을 보완하기 위해 나온 모델
- JSP는 view의 역할만 하고, 컨트롤러의 역할은 servlet이 하고, model은 MVC1, MVC2 모두 동일하다.
  - view는 JSP로 구성되어 있으며, 자바는 포함되지 않고 jstl을 사용해 결과를 표현한다.
- 규모가 크고 유지보수가 많은 경우 사용한다.

```
1. controller(servlet)에서 웹 브라우저의 요청을 처리 -> 
2. controller가 비즈니스 로직을 수행하며, model을 호출하여 데이터를 요청한다. -> 
3. model은 처리 결과를 view(jsp)에 보내어 사용자에게 응답하게 된다.
```

<br>

#### 3-1) 장점
- controller와 view의 분리로 분명한 구조를 가진다.
- 처리작업의 분리로 인해 유지보수의 확장이 용이하다.
- 개발자와 디자이너의 역할 분담이 확실해진다.
#### 3-2) 단점
- 구조 설계를 위한 시간이 많이 소요된다.
- 개발 기간이 증가한다.
- 높은 수준의 이해도가 필요하다.

<br><br>

### 4. Spring MVC ?
- 스프링이 제공하는 트랜잭션 처리, DI, AOP를 손쉽게 사용할 수 있다.
- Restful API를 만드는데 더 특화되었다.
- Dispatcher Servlet, Handler Mapping, ViewResolver가 추가되었다.
- 다른 MVC 프레임워크와 마찬가지로 컨트롤러를 사용하여 요청을 처리한다.
- DispatcherServlet이 MVC에서의 컨트롤러(Controller)부분을 처리한다.

<br>

#### 4-1) Spring MVC에서의 데이터 처리 흐름
![spring_mvc](https://oopy.lazyrockets.com/api/v2/notion/image?src=https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2F183bb42c-2998-4362-ade1-b7d75f75a851%2FUntitled.png&blockId=209ecc2e-d659-4a44-a519-675c16309d89)
```
1. 클라이언트는 dispatcher servlet에게 Request를 전달한다.
2. dispatcher servlet은 어떤 Controller에게 해당 request의 처리를 맡길지 handler mapping에게 URI 처리를 문의한다.
3. 여기서 handler mapping은 해당 URI를 처리할 수 있는 controller를 반환한다.
4. dispatcher servlet은 request를 controller에게 전달합니다.
5. 해당 controller는 request를 처리할 수 있는 메소드를 이용해 처리한 뒤, 수행 결과를 담은 modelAndView 객체를 반환한다.
6. dispatcher servlet은 view name을 view resolver에게 전달하고, view resolver는 해당 view를 처리할 수 있는 실제 path 및 확장자 정보를 담아 response한다.
7, dispatcher servlet은 view가 클라이언트에게 response하도록 실제 view path와 request를 전달한다.
8. view는 실행 결과를 클라이언트에게 response하며 화면을 표현한다.
```

<br>

#### 4-2) Spring MVC 구성 요소
|구성 요소|설명|
|:---:|:---:|
|DispatcherServlet|클라이언트의 요청을 전달받아 요청에 맞는 컨트롤러가 리턴한 결과값을 View에 전달하여 알맞은 응답을 생성|
|HandlerMapping|클라이언트의 요청 URL을 어떤 컨트롤러가 처리할지 결정|
|Controller|클라이언트의 요청을 처리한 뒤, 결과를 DispatcherServlet에게 리턴|
|ModelAndView|컨트롤러가 처리한 결과 정보 및 View 선택에 필요한 정보를 담음|
|ViewResolver|컨트롤러의 처리 결과를 생성할 View를 결정|
|View|컨트롤러의 처리 결과를 보여줄 화면을 생성|

<br>

#### 4-3) Spring MVC 개발
- 클라이언트에 요청을 받을 DispatcherServlet을 web.xml 파일에 설정한다.
- 클라이언트의 요청을 처리할 controller를 작성한다.
- ViewResolver를 설정한다.
  - 컨트롤러가 전달한 값을 이용해서 응답 화면을 생성할 view를 결정한다.
- JSP 등을 이용하여 view 영역의 코드를 작성한다.
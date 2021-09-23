# Spring MVC

Spring MVC는 기존 MVC Model2 Architecture와 Front-Controller 패턴을 프레임워크 차원에서 제공한다.

기존의 JSP & Servlet 기반 MVC 패턴에서 Model - View - Controller로 분할했던 역할에 비해 Spring MVC는 역할을 더 명확히 분리하고, Restful API를 만드는 데 더 특화되었다.

![image](https://user-images.githubusercontent.com/30489264/134497135-4658b7d7-1c79-47f5-a414-4d18c9e2d2ca.png)

Spring Framework에 의해 **Dispatcher Servlet(Front Servlet), Handler Mapping, ViewResolver**가 추가되었다.

Client로부터의 요청이 왔을 때 실행 순서는 다음과 같다.

1️⃣ Client가 Dispatcher Servlet에게 Request를 전달한다

2️⃣ Dispatcher Servlet은 어떤 Controller에게 해당 Request의 처리를 맡길지 Handler Mapping에게 URI 처리를 문의한다

3️⃣ Handler Mapping은 해당 URI를 처리할 수 있는 Controller를 반환한다

4️⃣ Dispatcher Servlet은 Request를 Controller에게 전달한다

5️⃣ 해당 Controller는 Request를 처리할 수 있는 메소드를 이용해 처리한 뒤 수행 결과를 담은 ModelAndView 객체를 반환한다

Model: Business Logic 수행 후 얻어낸 DTO
View: Response할 View Name
6️⃣ Dispatcher Servlet은 View Name을 ViewResolver에게 전달한다

7️⃣ ViewResolver는 해당 View를 처리할 수 있는 실제 path 및 확장자 정보를 담아 Response한다

8️⃣ Dispatcher Servlet은 View에게 Client에게 Response하도록 실제 View Path와 함께 Request를 전달한다

9️⃣ View는 실행 결과를 Client에게 Response하며 화면을 표현한다
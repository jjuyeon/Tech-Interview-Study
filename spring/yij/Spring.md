## Spring 🆚 Spring Boot

> Whatever happened next, the framework needed a name. In the book it was referred to as the “Interface21 framework” (at that point it used com.interface21 package names), but that was not a name to inspire a community. Fortunately Yann stepped up with a suggestion: “Spring”. His reasoning was association with nature (having noticed that I'd trekked to Everest Base Camp in 2000); and the fact that **Spring represented a fresh start after the “winter” of traditional J2EE**. We recognized the simplicity and elegance of this name, and quickly agreed on it.  
> 
> 👉 [Spring의 기원 ](https://spring.io/blog/2006/11/09/spring-framework-the-origins-of-a-project-and-a-name)

<br>

> Spring Boot makes it easy to create ***stand-alone, production-grade Spring based Applications*** that you can **"just run"**.
> 
> <U>스프링 부트는 상용화 수준의 스프링 기반 단독 어플리케이션을 "당신이 실행만 시킬 수 있도록" 쉽게 만들 수 있습니다.</U>
> 
> 👉 [Spring Boot 공식 문서](https://spring.io/projects/spring-boot)

<br>

||Spring|Spring Boot|
|-|-|-|
|Dependency|xml metadata를 이용해서 표기해야 함(버전까지 직접 명시)|build file을 통해 간단하게 dependency를 설정할 수 있도록 바뀜(버전도 권장 버전으로 자동 설정)<br>spring-boot-starter-** 패키지들을 사용|
|Configuration|AppConfiguration에 Annotation을 많이 달아서 길었음|application.propeties/yml 파일을 이용해 간단하게 설정|
|내장 서버|X|Spring Boot에서 Tomcat(혹은 jetty)을 내장 서버로 가지고 있음 <br>👉 내장 서블릿 컨테이너 덕분에 jar 파일로 쉽게 배포 가능|


<br>

<details>

<summary>예시</summary>

![image](https://user-images.githubusercontent.com/30489264/134816910-31bfd42f-69fb-436c-949f-325d0f26313d.png)

![image](https://user-images.githubusercontent.com/30489264/134816934-be72cb01-3713-468d-8ce4-06e7ba99fb7e.png)

</details>

<br>

#### Spring Boot의 장점(정리)

- 간편한 설정
- 편리한 의존성 관리 & 자동 권장 버전 관리
- 내장 서버로 인한 간단한 배포 서버 구축
- Spring Security, Spring Data JPA 등의 다른 스프링 프레임워크 요소를 쉽게 사용 가능

<br>

## Spring MVC

1. Request를 처리할 Handler를 Mapping
2. View의 처리
3. 등등

Spring MVC 프레임워크는 이러한 역할을 맡은 `DispatcherServlet`을 중심으로 설계되었다.

기본 핸들러는 `@Controller`와 `@RequestMapping` 어노테이션을 기반으로 넓고 유연한 핸들링 메서드를 제공한다. 스프링 3.0과 함께, `@Controller` 메커니즘은 RestfulAPI를 만들기 위한 `@PathVariable` 어노테이션을 추가로 제공한다.

<br>

> Spring Web MVC는 SOLID 원칙 중 하나인 OCP(Open-Closed Principle)을 기반으로 설계되었다.  
> 몇몇 핵심 클래스들의 메서드는 `final` keyword와 함께 정의되었는데, OCP에 의해 개발자가 이것을 override해서 행동을 마음대로 정의하지 못하도록 제한한 것이다.
> 👉 [OCP Paper](https://courses.cs.duke.edu//fall07/cps108/papers/ocp.pdf) 보기


<br>
<br>
<br>
<br>
<br>

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
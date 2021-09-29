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

<br>

## Filter

필터란 리소스에 대한 요청(서블릿/정적 콘텐츠)이나 리소스의 응답 또는 둘 다에 대해 필터링 작업을 수행하는 개체를 말한다.

<br>

``` java
// javax.servlet
Interface Filter {
  void destroy();
  void doFilter(ServletRequest request, ServletResponse response, FilterChain chain);
  void init(FilterConfig filterConfig);
}
```

`doFilter` 메서드에서 필터링 작업을 수행한다. 모든 필터는 초기화 매개변수를 얻을 수 있는 FilterConfig 객체에 대한 액세스 권한을 가지고 있으며 필터링 작업에 필요한 리소스를 로드하는 데 사용할 수 있는 ServletContext에 대한 참조를 가지고 있다. 

<br>

필터의 사용 예시

- Authentication
- Logging
- Encryption
- Tokenizing

### Filter Method

#### init

service에 필터를 적용할 수 있음을 나타내기 위해 컨테이너에 의해 호출되는 메서드

서블릿 컨테이너는 필터를 인스턴스화 한 후 init 메서드를 **정확히 한 번** 호출한다. 필터링 작업의 수행을 요청하기 전 init 메서드가 성공적으로 완료되어야 한다.

init 메서드가 다음 중 하나인 경우 `ServletException`을 throw하며 서비스에 배치할 수 없다.

- ServletException 발생
- 웹 컨테이너서 정의한 기간 내에 반환되지 않을 경우

<br>

#### doFilter

컨테이너에 의해 클라이언트가 `chain` 끝에 있는 리소스를 요청할 때, 요청/응답 쌍이 체인을 통해 전달될 때 마다 호출되는 메서드

이 메서드에 전달된 `FilterChain`을 통해 필터는 Request와 Response를 체인의 다음 엔티티에 전달할 수 있다.

`doFilter`의 일반적 동작 과정은 다음과 같다.

1. Request 검토
2. (선택적)입력 필터링을 위해 Request 객체를 사용자 구현 로직으로 감싸서 콘텐츠 또는 헤더를 필터링
3. (선택적)출력 필터링을 위해 Response 객체를 사용자 구현 로직으로 감싸서 콘텐츠 또는 헤더를 필터링
4. 　
    - `FilterChain` 객체의 (`chain.doFilter()`)를 이용해서 chain에 연결된 다음 엔티티를 호출하거나
    - 요청/응답 쌍을 다음 엔티티로 넘기지 않아서 Filter Chain에 Request 처리를 중단함  
5. filter chain의 다음 엔티티를 호출한 후 Response의 헤더를 직접 설정

<br>

#### destroy

컨테이너에 의해 필터가 서비스 중이 아님을 나타내기 위해 호출되는 메서드

1️⃣ `doFilter` 메서드에 있는 모든 스레드가 종료되거나 2️⃣ timeout period가 지난 후 딱 한 번만 호출된다. 컨테이너가 `destroy` 메서드를 호출한 후에는 filter의 이 인스턴스에서 `doFilter` 메서드를 다시 호출하지 않는다.

이 메서드를 사용하면 filter가 보유중인 자원(ex. 메모리, 파일, 스레드)를 정리하고 영속상태가 필터의 현 상태와 동기화 되도록 만들 수 있다.

<br>

## (Handler)Interceptor

> 💡 이 글에서 언급하는 Interceptor란 Spring MVC의 HandlerInterceptor를 의미한다.

Interceptor(이하 인터셉터)를 알아보기 전 `HandlerMapping`을 다시 한 번 정의하자면, `DispatcherServlet`이 Request를 처리할 수 있게 method와 URL을 연결시키는 Spring MVC 구성요소다.

`DispatcherServlet`은 메서드를 (실제로)실행하기 위해 HandlerAdapter를 사용한다.

Handler Interceptor는 이 전반적 Context에 포함되어 작업을 수행한다. 요청을 1️⃣ 처리하기 전, 2️⃣ 처리한 후, 3️⃣ (view의 렌더링이) 완료된 후에 작업을 처리하기 위해 Handler Interceptor를 사용한다.

인터셉터는 `cross-cutting concern`(횡단 관심사)를 처리하기 위해 사용하거나, Spring 모델에서 사용하는 logging, 파라미터 변경 등의 반복적인 코드 작업을 방지할 수 있다.

```java

void pizzaMethod() {
  System.out.println("pizza method has been called");
  /*
  Business Logic
  ...
  */
}

void hamburgerMethod() {
  System.out.println("hamburger method has been called");
  /*
  Business Logic
  ...
  */
}
```

예시와 같은 로깅 작업은 메서드 들에서 반복적으로 사용될 수 있는 코드면서, 비즈니스 로직과는 관련이 없다.

따라서 이러한 cross-cutting 관심사를 Interceptor로 분리하는 것은 Spring의 AOP와 관련이 있다.

<br>

### Interceptor 설정

<details>
<summary>pom.xml setting for Interceptors</summary>

```xml
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-web</artifactId>
    <version>5.2.8.RELEASE</version>
</dependency>
```

</details>

<br>

### Interceptor 사용 in Spring

프레임워크에서 `HandlerMapping`으로 작업하는 Interceptor는 `HandlerInterceptor`를 implements해야 한다.

- prehandle() - 실제 핸들러가 실행되기 전 호출되어 view가 실행되지 않는다.
- poseHandle() - 핸들러가 실행된 후 호출된다.
- afterCompletion() - 전체 Request가 완료되고 view가 생성된 후 호출된다.

세 가지 메서드를 통해 모든 종류의 pre-, pose- 처리를 할 수 있다.

> `HandlerInterceptor`와 `HandlerInterceptorAdapter`의 차이
> 
> - 전자는 위의 세 가지 메서드를 모두 override 해야 한다  
> - 후자는 필요한 메서드만 override 하면 된다

<br>

메서드의 반환 값으로 request가 handler에 의해 추가로 처리되어야 할 지 여부를 알 수 있다(boolean).

<br>

<details>
<summary>handler 메서드의 사용 예시</summary>

```java
@Override
public boolean preHandle(
  HttpServletRequest request,
  HttpServletResponse response, 
  Object handler) throws Exception {
    // your code
    return true;
}
```

```java
@Override
public void postHandle(
  HttpServletRequest request, 
  HttpServletResponse response,
  Object handler, 
  ModelAndView modelAndView) throws Exception {
    // your code
}
```

```java
@Override
public void afterCompletion(
  HttpServletRequest request, 
  HttpServletResponse response,
  Object handler, Exception ex) {
    // your code
}
```

</details>

`HandlerInterceptor`는 `@Controller` Annotation이 표시 된 모든 클래스에 Interceptor를 적용하는 `DefaultAnnotationHandlerMapping` Bean에 등록된다.

<br>

### Interceptor 활용 - Logger

Web Application의 개발 시 사용하는 Logging은 Interceptor 활용의 대표적인 예시다.

아래의 예시에서 Log4J를 활용한 Interceptor 로깅 방식을 설명한다.

```java
public class LoggerInterceptor extends HandlerInterceptorAdapter {
    ...
}
```

우선 `HandlerInterceptorAdapter`를 상속받아 LoggerInterceptor 클래스를 정의해준 뒤

```java
private static Logger log = LoggerFactory.getLogger(LoggerInterceptor.class);
```

위와 같은 코드를 사용하면 Log4J를 이용해서 로깅을 할 수 있다.

Request로부터 URI와 Parameter를 전달받아 로깅하는 예시이다. 이 예시(특수한 경우)에서는 로깅 시 Parameter에 비밀번호와 같은 민감한 정보가 포함되었을 경우, 정보를 그대로 표시하는 것이 아닌 *(별표)로 치환하도록 작성했다.

```java
@Override
public boolean preHandle(
  HttpServletRequest request,
  HttpServletResponse response, 
  Object handler) throws Exception {
    
    log.info("[preHandle][" + request + "]" + "[" + request.getMethod()
      + "]" + request.getRequestURI() + getParameters(request));
    
    return true;
}
```

<details>
<summary>추가 코드</summary>

```java
private String getParameters(HttpServletRequest request) {
    StringBuffer posted = new StringBuffer();
    Enumeration<?> e = request.getParameterNames();
    if (e != null) {
        posted.append("?");
    }
    while (e.hasMoreElements()) {
        if (posted.length() > 1) {
            posted.append("&");
        }
        String curr = (String) e.nextElement();
        posted.append(curr + "=");
        if (curr.contains("password") 
          || curr.contains("pass")
          || curr.contains("pwd")) {
            posted.append("*****");
        } else {
            posted.append(request.getParameter(curr));
        }
    }
    String ip = request.getHeader("X-FORWARDED-FOR");
    String ipAddr = (ip == null) ? getRemoteAddr(request) : ip;
    if (ipAddr!=null && !ipAddr.equals("")) {
        posted.append("&_psip=" + ipAddr); 
    }
    return posted.toString();
}

private String getRemoteAddr(HttpServletRequest request) {
    String ipFromHeader = request.getHeader("X-FORWARDED-FOR");
    if (ipFromHeader != null && ipFromHeader.length() > 0) {
        log.debug("ip from proxy - X-FORWARDED-FOR : " + ipFromHeader);
        return ipFromHeader;
    }
    return request.getRemoteAddr();
}
```

</details>

<br>

return value를 활용해서 핸들러에게 추가 처리가 필요한 지 여부를 알린다. false를 반환하면 스프링은 request의 처리가 완료되었고 추가 처리가 필요하지 않다고 가정한다.

```java
@Override
public void postHandle(
  HttpServletRequest request, 
  HttpServletResponse response,
  Object handler, 
  ModelAndView modelAndView) throws Exception {
    
    log.info("[postHandle][" + request + "]");
}
```

poseHandle 메서드는 `HandlerAdapter`는 호출되었지만 view는 아직 렌더링 되지 않았을 때 호출된다.

`ModelAndView`에 속성을 추가하거나 핸들러 메서드로 요청 처리 시간을 측정하는 데 활용할 수 있다.

```java
@Override
public void afterCompletion(
  HttpServletRequest request, HttpServletResponse response,Object handler, Exception ex) 
  throws Exception {
    if (ex != null){
        ex.printStackTrace();
    }
    log.info("[afterCompletion][" + request + "][exception: " + ex + "]");
}
```

afterCompletion 메서드를 활용하면 view가 렌더링 된 이후이기 때문에 request와 response 데이터 및 예외에 대한 정보에 대해 로깅할 수 있다.

<br>

### Interceptor Configuration

인터셉터를 Spring Configuration에 추가하려면 `WebMvcConfigurer`를 구현하는 `WebConfig` 클래스 내에서 `addInterceptor()` 메서드를 재정의해야 한다.

```java
@Override
public void addInterceptors(InterceptorRegistry registry) {
    registry.addInterceptor(new LoggerInterceptor());
}
```

또는 xml 파일을 이용해 설정할 수 있다.

```xml
<mvc:interceptors>
    <bean id="loggerInterceptor" class="com.baeldung.web.interceptor.LoggerInterceptor"/>
</mvc:interceptors>
```

configuration이 적용되면 인터셉터가 활성화되고 로깅을 활용할 시 애플리케이션의 모든 요청이 올바르게 기록된다.

스프링 인터셉터가 여러 개 구성된 경우, `preHandle()` 메서드는 구성 순서대로 실행되는 반면 `postHandle()`과 `afterCompletion()` 메서드는 역순으로 호출된다.

Spring Boot를 사용할 경우 `@EnableWebMvc` Annotation을 사용하면 Boot의 auto configuration이 손상될 수 있으니 사용하면 안 된다.

<br>

## Filter vs Interceptor

Interceptor는 **Spring Framework**의 구성 요소지만, Filter는 **Servlet**의 구성 요소다. Filter는 Request 요청이 서블릿에 도달하지 못하도록, 혹은 Response가 클라이언트에 도달하지 못하도록 차단할 수 있다.

**Spring Security**가 <U>Authentication 및 Authorization에 필터를 적용</U>한 대표적인 예시다. `DelegatingFilterProxy`라는 단일 필터를 적용해서 Spring Security를 설정하면, 들어오는 트래픽과 나가는 트래픽 모두를 가로챌 수 있다. 따라서 Spring Security는 Spring MVC 외부에서 사용할 수 있다.

```java
@Component
public class LogFilter implements Filter {

    private Logger logger = LoggerFactory.getLogger(LogFilter.class);

    @Override
    public void doFilter(ServletRequest request, ServletResponse response, FilterChain chain) 
      throws IOException, ServletException {
        logger.info("Hello from: " + request.getLocalAddr());
        chain.doFilter(request, response);
    }

}
```

위에서 로깅에 Interceptor를 적용한 것 처럼, `Filter` Interface를 implement해서 로깅을 할 수도 있다.

`doFilter` 메서드를 override한 후, FilterChain 객체를 이용해 request를 허용하거나 차단할 수 있고 `@Component` 어노테이션을 붙임으로써 Spring에서 해당 Filter를 사용할 수 있다.

<br>

### 차이점

![image](https://user-images.githubusercontent.com/30489264/135289496-a3c3ca51-90d1-4eaa-9780-f97a93091ae7.png)

Filter는 Request가 `DispatcherServlet`에 도달하기 전에 위치해서 동작하지만, HandlerInterceptor는 DispatcherServlet과 Controller 사이에서 동작한다.

Filter는 Spring MVC와 별개의 위치에서 동작하기 때문에 분리를 원하는 작업에 적용하기 좋다.

#### Filter의 사용

- Authentication
- Logging and auditing
- Image and data compression
- Any functionality we want to be decoupled from Spring MVC

#### Interceptor의 사용

- Handling cross-cutting concerns such as application logging
- Detailed authorization checks
- Manipulating the Spring context or model

<br>

결론적으로, 

> Filter는 Request가 Controller와 Spring MVC 외부에 도달하기 전 조작할 수 있다.  
> HandlerInterceptor는 애플리케이션의 cross-cutting 관심사에 적용하기 적합하다. 대상 Handler 및 ModelAndView 객체에 대한 액세스를 제공하기 때문에 더 세분화 된 제어에 알맞다.

<br>

## 참고 자료

> - https://docs.oracle.com/javaee/6/api/javax/servlet/Filter.html
> - https://www.baeldung.com/spring-mvc-handlerinterceptor
> - https://www.baeldung.com/spring-mvc-handlerinterceptor-vs-filter
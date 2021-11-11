<details>
<summary>👉 서론</summary>

학부생 때 수강했던 과목은 많았지만 실제 현업에서의 프로그래밍에 대해 깊이있게 다뤘던 수업은 많지 않았다.

3학년 1학기 때 Model1 구조 기반으로 실습했었는데 그 때는 웹개발이란 원래 그런건 줄 알고 모든 프로젝트를 JSP를 사용해서 jsp 파일 안에 비즈니스 로직을 때려넣었었다...

심지어 4학년 2학기에 스프링을 처음 써봤는데 컨트롤러가 뭔지도 모르고 RequestMapping되는 방식을 몰라서 한참을 헤맸던 기억이 난다

물론 이번년도에 공부했던 것 처럼 JSP, 서블릿, MVC 패턴, Spring, Spring Boot에 대해 왜 이런 기술이 나왔는지 핵심 철학은 무엇인지 차근차근 배웠다면 더 좋았겠지만

오히려 '아 이 땐 이렇게 했었는데 잘못된 방식이었구나'를 느낄 수 있는 지금도 나쁘지 않다 ^^

</details>

## Web Application Server

JSP와 Servlet을 다루기 전, Web Application Server(WAS)에 대해 아래의 IBM 링크를 읽는 걸 추천 ❗😮

[Web Server 🆚 Application Server 🆚 Web Application Server](https://www.ibm.com/cloud/learn/web-server-vs-application-server)


간단하게 정리하면 다음과 같다

### 웹 서버

웹 브라우저의 HTTP 요청에 대한 응답으로 정적 웹 콘텐츠(HTML pages, file, image, video 등)를 제공하는 서버

### 애플리케이션 서버

웹 콘텐츠도 전달할 수 있지만 클라이언트와 서버 측 코드 간의 상호작용을 활성화시켜 동적 콘텐츠를 생성하고 전달하기 위한 서버

### 웹 애플리케이션 서버

실제 사용에서는 위의 두 서버의 경계가 모호해졌고, 웹 애플리케이션의 성능이 중요해지면서 등장함. 두 서버를 분리하지 않고 정적 웹 콘텐츠와 동적 응용 프로그램 컨텐츠를 함께 제공하는 서버

우리가 가장 많이 사용하는 WAS로는 [Nginx](https://nginx.org/en/)와 [Apache Tomcat](https://tomcat.apache.org/)이 있다.

Nginx는 리버스 프록시, 로드 밸런싱, HTTP 캐싱을 주로 지원하고
Apache Tomcat은 **자바 서블릿을 실행**하고 **JSP코드가 포함된 웹 페이지를 렌더링**/전달하고 Java EE(Enterprise Edition)애플리케이션을 제공한다.

<br>

## Servlet

<u>Java를 사용하여 웹 페이지를 동적으로 생성</u>할 수 있는 서버 프로그램으로, Java 클래스의 일종

### Servlet의 특징

- 서블릿 컨테이너가 **클라이언트의 요청을 받을 때 독립된 쓰레드를 생성**하여 다중 쓰레드를 제공한다
- 프로토콜에 관계 없이 여러 애플리케이션  기반의 서버 응용 프로그램을 구현할 수 있다
  - 주로 HTTP 프로토콜 기반의 서블릿 프로그램을 주로 구현
- 서블릿 표준 API에서 제공하는 추상 클래스와 인터페이스 기반의 클래스를 제공하기 때문에 특정 서블릿 컨테이너에 종속되지 않는다
- 대표적인 서블릿 컨테이너로 **Apache Tomcat**이 있다
- 하나의 HTTP Request 요청을 처리하는 서블릿 인스턴스는 컨테이너에 의해 **싱글톤**으로 관리된다.
  - Request와 관련된 작업을 마친 서블릿 인스턴스는 스레드풀에 들어가거나 (destroy메서드 호출 뒤) GC의 대상이 된다.


### HTTP protocol - Servlet 동작 과정

![image](https://www.notion.so/image/https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2F28fe562e-efea-4726-a6b7-b93f9d77aa84%2FUntitled.png?table=block&id=2bab2235-618c-45bc-b2db-786dc2d9fb31&spaceId=11280ec7-1c04-494f-8388-43ed5ddfcb0a&width=1600&userId=e567590d-ab63-42a6-869b-009383fc3d4f&cache=v2)

1. Web Client가 Web Server에게 **HTTP Request** 메시지를 보낸다.
     - 필요에 따라 매개변수, 쿠키 정보를 같이 보낼 수 있다
2. Web Server는 HTTP Request 메시지를 해석하여 서블릿에 대한 요청일 경우 **서블릿 컨테이너에게 (네트워크를 통해) Request를 전달**한다.
     - WS가 실행되는 컴퓨터와 서블릿 컨테이너가 실행되는 컴퓨터가 달라도 된다
3. 서블릿 컨테이너는 Web Server로부터 HTTP Request를 전달받아 해당 요청을 처리할 수 있도록 **서블릿을 생성하여 service() 메서드를 호출**한다.
4. 서블릿(컨테이너에 의해 인스턴스화된)은 필요에 따라 init() 메서드를 호출하여 초기화하고, 실제 서비스를 수행하기 위한 service() 메서드를 호출할 수 있다.
5. 서비스를 수행한 후 처리 결과를 알려주기 위한 **결과 페이지를 Web Server에게 전달**한다.
6. Web Server는 서블릿 컨테이너로부터 전달받은 **결과 페이지를 Web Client에게 HTTP Response로 보낸다**.

### Servlet 예시

![image](https://user-images.githubusercontent.com/30489264/136686866-3c040626-38af-4b79-8c56-9e4942d69795.png)

3학년 수업할 때 캡쳐한 사진?이 있길래 가져와봤다 ^^

이처럼 서블릿을 이용해서 Java 코드 안에 HTML을 넣어서 웹 페이지를 동적으로 생성할 수 있지만, `out.println`메서드가 중복되어 가독성이 심각하다.

그래서 Servlet으로 바로 웹 페이지를 만들기 보다는 JSP를 이용해서 웹 페이지를 동적으로 생성하고, 그것을 서블릿 형태로 변환해서 구현하는 게 일반적이다.

<br>

## JSP (Java Server Page)

HTML 문서에 Java 코드를 포함하여 웹 서버에서 동적인 웹 페이지를 생성할 수 있게 하는 도구

실행 시 서블릿으로 변환되어 실행되며, 서블릿과 달리 HTML 표준에 의해 작성되기 때문에 개발에 용이하다.

비슷한 구조로 PHP, ASP, ASP.NET 등이 있지만 현재는 많이 사용하지 않는다.

### JSP 특징

- Presentation Logic과 Business Logic 분리
  - 웹 페이지의 디자인이 변경되더라도 내부 비즈니스 로직을 처리하는 JSP와 `Javabeans` 코드는 변경 될 필요가 없다. 반대도 마찬가지
- 컴포넌트의 재사용 가능
  - 공통적으로 사용되는 컴포넌트를 만들어서 재사용할 수 있다.
  - ex) 주소 검색, 한글 처리, 데이터베이스 연결 등
- JSTL, EL, 커스텀 태그 들을 활용해서 개발을 용이하게 할 수 있다.

프레젠테이션 로직을 서버에서 분리하고, 다양한 클라이언트 플랫폼을 지원하기 위해 MVVM 모델을 사용하면서 JSP도 이제는 많이 사용하지 않는다!

### JSP 처리 과정

![image](https://www.notion.so/image/https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2Fe8cbf7c2-3b8a-489b-814c-20c6e9b22b68%2FUntitled.png?table=block&id=afa0bbfb-73ec-478d-887d-bb7e7381a56c&spaceId=11280ec7-1c04-494f-8388-43ed5ddfcb0a&width=1230&userId=e567590d-ab63-42a6-869b-009383fc3d4f&cache=v2)

JSP는 최초 요청 시 Servlet 파일로 변환되고 다시 클래스 파일로 컴파일된다.

서블릿 컨테이너인 톰캣이 클래스 파일을 실행시켜 결과를 웹 브라우저로 전달한다.

하지만 요청할 때 마다 매번 JSP 파일을 서블릿으로 변환하는건 비효율적이니까 맨 처음 요청에만 변환 과정이 존재한다.

한 번이라도 요청했던 JSP 파일을 다시 요청하면 이미 메모리에 적재된 클래스가 응답을 주기 때문에 대체로 평균 응답시간이 짧다.

<br>

## 정리

> WAS인 **Tomcat**은 서블릿 컨테이너로 동작하여 자바 서블릿을 실행하고, JSP 페이지를 렌더링해서 제공함으로써 동적인 웹 콘텐츠를 다룰 수 있다.  
> 
> **Servlet**은 Java를 이용해서 동적인 웹 콘텐츠를 만들 수 있는 프로그램이다. Java 코드 안에 HTML 코드를 삽입할 수 있다. 하지만 Servlet으로 직접 View 페이지를 생성하는 건 가독성이 떨어진다.  
> 
> **JSP**는 HTML 문서 안에 Java 코드를 삽입해서 웹 페이지를 동적으로 만들 수 있게 해준다. 요청 시 Servlet으로 변환된다.
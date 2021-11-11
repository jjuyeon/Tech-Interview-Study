# 6) Spring
- contributor : [김경원](https://github.com/shining8543) , [윤이진](https://github.com/483759) , [진수연](https://github.com/jjuyeon)
<hr/>

### :notebook_with_decorative_cover: Spring
1. Spring MVC 구조 흐름에 대해 과정대로 설명해주세요.

> 1️⃣ 클라이언트는 디스패처 서블릿에게 요청합니다. 
> 2️⃣ 디스패처 서블릿은 어떤 컨트롤러가 요청을 처리할 수 있을지 핸들러 매핑을 이용해서 URI를 분석해서 알아냅니다.
> 3️⃣ 해당 요청을 특정한 컨트롤러에게 전달합니다.
> 4️⃣ 컨트롤러는 비즈니스 로직을 통해 요청을 처리한 뒤 수행 결과를 반환합니다.
> 5️⃣ 디스패처 서블릿은 수행 결과를 표현할 뷰 이름을 뷰리졸버에게 전달합니다.
> 6️⃣ 뷰 리졸버는 이를 처리할 수 있는 경로와 확장자 정보를 담아 반환합니다.
> 7️⃣ 디스패처 서블릿은 뷰에게 응답 결과를 담아 전달합니다.
> 8️⃣ 뷰는 클라이언트에게 응답 결과를 담아 화면을 표현합니다.

<br>

2. Spring DI에 대해 설명해주세요.

> DI는 Dependency Injection의 약자로, 객체 인스턴스의 생성/관리/소멸을 사용자가 아닌 컨테이너가 관리하는 것을 말합니다.
> 인스턴스의 생성과 객체의 선언을 분리해서 결합도를 낮춥니다. 이는 프로그램의 유지보수성과 확장성을 증가시킬 수 있습니다.

<br>

3. Spring과 Spring Boot의 차이점에 대해 설명해주세요.

> 스프링 부트는 스프링 기반 애플리케이션을 더 쉽게 구현할 수 있도록 해주는 프레임워크입니다. 기존 xml 기반으로 설정을 해주어야 했던 부분을 빌드 파일을 이용해서 쉽게 관리해서 권장 버전으로 설정해줍니다. 또한 톰캣을 내장 서버로 가지고 있어 WAS 서버를 직접 지정해주지 않아도 됩니다.

<br>

4. Filter와 Interceptor의 차이점에 대해 설명해주세요.

> 

<br>

5. 영속성 컨텍스트에 대해 설명해주세요.
6. Spring에서 CORS 에러를 해결하기 위해 했던 방법은 무엇인가요?
7. Bean/Component 어노테이션은 무엇이며, 둘의 차이점은 무엇인가요?
8. Spring Triangle의 POJO와 AOP에 대해 설명해주세요
9.  Hibernate에 대해서 설명하세요

### :notebook_with_decorative_cover: JPA
1. JPA Propagation 전파단계를 설명해주세요.
2. N+1 문제가 발생하는 이유와 이를 해결하는 방법을 설명해주세요.

### :notebook_with_decorative_cover: Design Pattern
1. Builder 패턴에 대해서 설명하세요.
2. MVC1과 MVC2 패턴의 차이를 설명해 주세요.
3. (꼬리질문) 그렇다면, Spring MVC 는 무엇일까요?
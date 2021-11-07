# :question: Spring Bean

#### reference
https://github.com/WeareSoft/tech-interview/blob/master/contents/spring.md<br>
https://gmlwjd9405.github.io/2018/11/10/spring-beans.html<br>
https://cornswrold.tistory.com/100<br>
https://blog.naver.com/PostView.naver?isHttpsRedirect=true&blogId=gngh0101&logNo=222344239921&parentCategoryNo=&categoryNo=32&viewDate=&isShowPopularPosts=true&from=search<br>
https://docs.spring.io/spring-framework/docs/4.2.5.RELEASE/spring-framework-reference/html/beans.html#beans-factory-scopes
<hr>

## Question
1. [Bean의 생명주기에 대해 설명해주세요.](#2-bean-생명주기)
- 스프링 빈 객체가 생성되고 의존 관계가 설정됩니다. 그 후, 객체 초기화, 사용, 소멸 과정의 생명주기를 가지고 있습니다.
- Bean은 스프링 컨테이너에 의해 생명주기가 관리됩니다. Bean 초기화를 하기 위해서는 @PostConstruct 어노테이션을, 소멸에는 @PreDestroy 어노테이션을 사용합니다.
- (생성한 스프링 Bean 객체를 등록할 때는 ComponentScan을 이용하거나 @Configuration 의 @Bean 을 사용하여 Bean xml 설정파일에 직접 Bean 객체를 등록할 수 있습니다.)

2. [Bean scope에 대해 설명해주세요.](#5-bean-scope)
- Bean이 존재할 수 있는 범위를 뜻하며 singleton, prototype, request, session, application이 있습니다.
- singleton은 기본 설정 범위로 스프링 컨테이너의 시작과 종료까지 유지되는 가장 넓은 범위의 스코프입니다. 
- prototype은 bean의 생성과 의존관계 주입까지만 관여하고 더는 관리하지 않는 매우 짧은 범위의 스코프입니다.
- request는 웹 요청이 들어오고 나갈때까지 유지하는 스코프, session은 웹 세션이 생성, 종료할때까지, application은 웹 ServletContext와 같은 범위로 유지하는 스코프입니다.

<hr>

## :nerd_face:	What I study

### 1. Bean
- 애플리케이션의 핵심을 이루는 객체
- Spring IoC 컨테이너가 관리하는 자바 객체
- 컨테이너 안에 담겨있으며, 필요할 때마다 컨테이너에서 가져와서 사용한다.
- Bean으로 등록된 객체는 쉽게 주입하여 사용할 수 있다.
#### 1-1) Bean으로 등록하는 법
- `@Bean` 어노테이션을 사용한다.
- xml 파일 설정(설정 메타 데이터)을 통해 일반 객체를 Bean으로 등록한다.
  - 컨테이너는 이 메타 데이터를 통해 Bean의 생성, life cycle, 종속성 등을 확인할 수 있다.
#### 1-2) 주요 속성
- class(필수): 정규화된 자바 클래스 이름
- id: bean의 고유 식별자
- scope: 객체의 범위 (sigleton, prototype)
- constructor-arg: 생성 시 생성자에 전달할 인수
- property: 생성 시 bean의 setter에 전달할 인수
- init-method: 초기화 메소드
- destroy-method: 소멸 메소드

```xml
<!-- A simple bean definition -->
<bean id="world1" class="com.spring.Hello"></bean>

<!-- A bean definition with scope-->
<bean id="world2" class="com.spring.Hello" scope="singleton"></bean>

<!-- A bean definition with property -->
<bean id="world3" class="com.spring.Hello">
	<property name="msg" value="HI"/>
</bean>
```

<br><br>

### 2. Bean 생명주기
```
스프링 컨테이너 생성 -> 스프링 빈 객체 생성 -> 의존관계 주입
-> 초기화 -> 사용 -> 소멸 -> 스프링 종료
```
- 스프링 컨테이너에 의해 생명주기가 관리된다.
- 스프링 컨테이너 초기화 시, `빈 객체 생성, 의존 객체 주입 및 초기화`가 일어난다.
- 스프링 컨테이너 종료 시, `빈 객체 소멸`이 일어난다.

<br><br>

### 3. Bean 초기화
- Bean 객체가 생성되고 DI 작업까지 마친 다음 실행되는 메소드를 설정한다.

<br>

1. Bean 초기화 메소드에 `@PostConstruct` 어노테이션을 사용한다.
   - 사용 방법이 간결하며 코드에서 초기화 메소드가 존재함을 쉽게 파악할 수 있다.
   - xml 설정 방법보다 직관적이다.
2. 초기화 콜백 인터페이스를 사용한다.
  - `InitializingBean 인터페이스의 afterPropertiesSet() 메소드` 를 오버라이드한다.
  - property 설정까지 마친 뒤에 호출된다.
  - ***권장 X*** (Bean 코드에 스프링 인터페이스를 노출하기 때문)
3. init() 메소드를 커스텀해서 정의한다.
  - Bean 코드에 스프링 인터페이스는 노출되지 않지만, 코드만으로 초기화 메소드 호출 여부를 알 수 없다. 
  - Bean을 정의한 xml에 `init-method` 속성으로 메소드 이름을 지정한다.
    - ex) `<bean id="myBean" class="MyBean" init-method="initResource"/>`
    - DI 작업까지 마친 뒤에 initResource() 메소드가 실행되도록 선언함.
  - Bean 초기화 메소드에 `@Bean(init-method="init")` 을 지정한다.
    - ex) `@Bean(init-method="initResource") public void MyBean myBean()`

<br><br>

### 4. Bean 소멸
- 컨테이너가 종료될 때 호출돼서 Bean이 사용한 리소스를 반환하거나 종료 전에 처리해야 할 작업을 수행하는 메소드를 설정한다.

1. Bean 소멸 메소드에 `@PreDestroy` 어노테이션을 사용한다.
   - 사용 방법이 간결하며 코드에서 초기화 메소드가 존재함을 쉽게 파악할 수 있다.
   - xml 설정 방법보다 직관적이다.
2. `DisposableBean 인터페이스의 destroy() 메소드` 를 오버라이드한다.
3. destroy() 메소드를 커스텀해서 정의한다.
  - Bean 코드에 스프링 인터페이스는 노출되지 않지만, 코드만으로 초기화 메소드 호출 여부를 알 수 없다. 
  - Bean을 정의한 xml에 `destroy-method` 속성으로 메소드 이름을 지정한다.
  - Bean 소멸 메소드에 `@Bean(destroy-method="destroy")` 을 지정한다.

<br><br>

### 5. Bean Scope
- Bean이 존재할 수 있는 범위
- scope마다 동작 방법이 다르므로 주의해서 사용해야 한다!
- 모든 bean은 기본적으로 `singleton`으로 생성하여 관리된다.
- Web 관련 scope(`request`, `session`, `global session`, `application`)는 일반 Spring 어플리케이션이 아닌 Spring MVC Web Application에서만 사용된다.
  - 특별한 scope는 꼭 필요한 곳에만 최소화해서 사용해야 한다.
  - 무분별하게 사용하면 유지보수하기 어려워진다.

<br>

1. singleton (default)
- 기본 설정 범위로 스프링 컨테이너의 시작과 종료까지 유지되는 가장 넓은 scope
- Spring IoC Container 내에 단 한 개의 Bean 객체만을 생성한다.
- 컨테이너가 사라질 때, Bean도 제거된다.
- 컨테이너가 Bean을 주입할 때, 항상 같은 객체를 사용하는 것이 보장된다.
  - 생성된 Bean 객체는 single beans cache에 저장되고, 해당 bean에 대한 요청과 참조가 있으면 캐시된 객체를 반환한다.
- 메모리나 성능 최적화에 유리하다.

2. prototype
- bean의 생성과 의존관계 주입까지만 관여하고 더는 관리하지 않는 매우 짧은 scope
- 하나의 Bean에 대해서 다수의 객체가 존재할 수 있다.
- 컨테이너가 Bean을 가져다 주입할 때, 항상 새로운 객체를 생성하여 주입된다.
- 모든 요청에서 새로운 객체를 생성한다.
- GC(Garbage Collector)에 의해 Bean을 제거한다.
<br>

- xml로 설정하는 방법
```xml
<!-- A bean definition with 'singleton' scope-->
<bean id="world2" class="com.spring.Hello" scope="singleton"></bean>

<!-- A bean definition with 'prototype' scope-->
<bean id="world2" class="com.spring.Hello" scope="prototype"></bean>
```
- annotation으로 설정하는 방법
  - 대상 클래스 위에 `@Scope("singleton")` or `@Scope("prototype")`

<br>

3. request
- 하나의 Bean에 대해서, 하나의 HTTP request 생명주기 안에 단 하나의 Bean 객체만 존재한다.
- 각각의 HTTP request는 고유 Bean 객체를 가진다.
- Spring MVC Web Application에서만 사용한다.

4. session
- 하나의 Bean에 대해서, 하나의 HTTP session 생명주기 안에 단 하나의 Bean 객체만 존재한다.
- Spring MVC Web Application에서만 사용한다.

5. global session
- 하나의 Bean에 대해서, 하나의 global HTTP Session 생명주기 안에 단 하나의 Bean 객체만 존재한다.
- 일반적으로 portlet context 안에서 유효하다.
  - portlet: 재사용이 가능한 웹 구성요소
  - portlet context는 여러 개의 portlet으로 이루어져있고, 각 portlet은 session을 소유하고 있다.
  - 만약 전체적으로 변수를 저장하기 원한다면 global session을 사용해야 한다.
- Spring MVC Web Application에서만 사용한다.
- 참고: https://circlee7.medium.com/%ED%8F%AC%ED%8B%80%EB%A6%BF%EC%9D%B4%EB%9E%80-%EB%AC%B4%EC%97%87%EC%9D%B8%EA%B0%80-6b26036677ee

6. application
- 하나의 Bean에 대해서, ServletContext 생명주기 안에 단 하나의 Bean 객체만 존재한다.
- Spring MVC Web Application에서만 사용한다.

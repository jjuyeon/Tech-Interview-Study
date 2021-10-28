# :question: Spring Bean

#### reference
https://github.com/WeareSoft/tech-interview/blob/master/contents/spring.md<br>
https://gmlwjd9405.github.io/2018/11/10/spring-beans.html<br>
https://cornswrold.tistory.com/100
<hr>

## Question
1. Bean의 생명주기에 대해 설명해주세요.
- 스프링 빈 객체가 생성되고 의존 관계가 설정됩니다. 그 후, 객체 초기화, 사용, 소멸 과정의 생명주기를 가지고 있습니다.
- Bean은 스프링 컨테이너에 의해 생명주기가 관리됩니다. Bean 초기화를 하기 위해서는 @PostConstruct 어노테이션을, 소멸에는 @PreDestroy 어노테이션을 사용합니다.
- (생성한 스프링 Bean 객체를 등록할 때는 ComponentScan을 이용하거나 @Configuration 의 @Bean 을 사용하여 Bean xml 설정파일에 직접 Bean 객체를 등록할 수 있습니다.)

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

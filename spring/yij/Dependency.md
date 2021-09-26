> [💡](#ioc---inversion-of-control) Spring의 IoC, DI에 대해 설명해주세요  
> 👉 객체 간의 의존성을 코드 내에 구현하면, 클래스의 변경이 발생할 때 해당 클래스를 참조하는 클래스의 코드 또한 모두 변경해야 합니다. 따라서 유지보수성을 증가시키고, 객체 간의 의존 관계를 컴파일 시간이 아닌 런타임에 설정함으로서 객체 간 결합도를 떨어뜨려 유지보수성을 증가시키는 방식을 IoC라고 합니다. 스프링에서는 IoC 컨테이너가 의존성 주입을 통해 이를 구현했습니다.  
> 스프링의 의존성 주입 방식은 생성자 주입, 세터 주입 방식이 있습니다. 일반적으로 객체의 불변성 성질을 지킬 수 있고 필드의 NPE가 발생하는 것을 막기 위한 생성자 주입 방식이 권장됩니다.

<br>

## IoC - Inversion of Control

: 객체의 종속성을 다음의 방식을 통해서만 정의하는 방식을 말한다. Spring에서는 Dependency Injection으로 구현되었다.

1. 생성자 인수
2. 팩토리 메서드의 인수
3. 팩토리 메서드에서 생성되거나 반환되어 객체 인스턴스에 설정된 속성

컨테이너는 빈을 생성할 때 의존성을 주입하므로 Compile Time이 아닌 Runtime에 객체 간의 의존 관계가 결정된다. 따라서 객체 간의 관계가 느슨하게 연결된다(loose coupling).

`org.springframework.beans`와 `org.springframework.context` package는 Spring Framework의 IoC 컨테이너를 담당합니다.

- `BeanFactory` : 모든 유형의 객체를 관리할 수 있는 configuration 메커니즘 제공
- `ApplicationContext` : `BeanFactory`를 상속받은 Sub-Interface
  - 더 많은 엔터프라이즈별 기능 추가
  - AOP 기능과의 간편한 통합
  - 웹 애플리케이션에서 사용하기 위한 `WebApplicationContext`와 같은 애플리케이션 레이어의 계층별 컨텍스트 제공

<br>

> __Bean__  
> 
> - Spring IoC 컨테이너에 의해 인스턴스화, 조립되어 관리되는 객체  
> - Bean과 애플리케이션 객체 간의 종속성은 컨테이너에서 사용하는 configuration metadata에 의해 반영

<br>

### IoC Container

![image](https://user-images.githubusercontent.com/30489264/134152539-4a9fbe51-11c3-45cc-ac41-ce62b8321758.png)


- `org.springframework.context.ApplicationContext` 인터페이스는 Spring IoC 컨테이너를 나타내며, 인스턴스를 구성하고 빈을 생성한다. 
- 컨테이너는 configuration metadata를 읽어 어떤 객체를 인스턴스화/구성/조립할 지에 대한 정보를 얻는다.
- Configuration metadata는 `XML`, `Java Annotations`, `Java Code`로 나타낼 수 있습니다. 애플리케이션을 구성하는 객체와 이들간의 상호의존성을 표현하는 데 사용한다.

<br>

#### XML 기반 metadata

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xsi:schemaLocation="http://www.springframework.org/schema/beans
        https://www.springframework.org/schema/beans/spring-beans.xsd">

    <bean id="..." class="...">  
        <!-- collaborators and configuration for this bean go here -->
    </bean>

    <bean id="..." class="...">
        <!-- collaborators and configuration for this bean go here -->
    </bean>

    <!-- more bean definitions go here -->

</beans>
```

- `id` : 빈을 정의하는 식별자
- `class` : 빈의 타입을 결정하는 클래스 이름

<br>

### IoC를 사용하는 이유

> 객체 간의 결합도(coupling)를 낮춘다  
> 결합도가 높으면 해당 클래스가 유지보수 될 때 그 클래스와 결합된 다른 클래스들도 같이 수정해야 한다 !!

<br>

#### 클래스 호출 방식

![image](https://user-images.githubusercontent.com/30489264/134156785-3105089c-38cd-4fee-8bbd-090445c83c9c.png)

```java
public class StoreController {
  private PizzaService pizzaService = new PizzaServiceImpl();
  private BurgerService burgerService = new BurgerServiceImpl();

  public void orderSetMenu(Menu menu) {
    pizzaService.get(menu.pizza);
    burgerService.get(menu.burger);
  }
}
```

클래스에서 직접 다른 클래스를 호출하는 방식

클래스 내에 선언과 구현이 모두 되어 있기 때문에 참조중인 클래스에 변화가 생기면 참조한 클래스의 코드 또한 변경해줘야 한다 !!

<br>

#### 인터페이스 호출 방식

![image](https://user-images.githubusercontent.com/30489264/134157856-cb381a2f-5919-4655-be37-ce2cb3b74848.png)

- 구현 클래스의 교체가 용이해서 다양한 형태로 변화가 가능하다
- 인터페이스를 교체할 시 호출 클래스도 수정해야 한다

```java
public class StoreController {
  private ItemService pizzaService = new PizzaServiceImpl();
  private ItemService burgerService = new BurgerServiceImpl();

  public void orderSetMenu(Menu menu) {
    pizzaService.get(menu.pizza);
    burgerService.get(menu.burger);
  }
}

public interface PizzaService extends ItemService {
  ...
}

public interface BurgerService extends ItemService {
  ...
}
```

<br>

#### 팩토리 호출 방식

![image](https://user-images.githubusercontent.com/30489264/134158082-5aea8112-fec3-40b5-9343-52df489b7ce0.png)

- 팩토리가 구현 클래스를 생성하고 해당 팩토리를 호출하는 방식
- 인터페이스 변경 시 팩토리만 수정하면 되어 호출 클래스에 영향을 미치지 않음
- 하지만 클래스에 팩토리를 호출하는 코드가 삽입되기 때문에 팩토리 의존적

<br>

```java
public class StoreController {
  private ItemService pizzaService = ServiceFactory.getPizzaService();
  private ItemService burgerService = ServiceFactory.getBurgerService();

  public void orderSetMenu(Menu menu) {
    pizzaService.get(menu.pizza);
    burgerService.get(menu.burger);
  }
}

public class ServiceFactory {
  public static ItemService getPizzaService() {
    return new PizzaServiceImpl();
  }
  
  public static ItemService getBurgerService() {
    return new BurgerServiceImpl();
  }
}
```

- 각 Service를 생성하여 반환하는 Factory를 사용
- Service를 이용하는 쪽 에서는 Interface만 알고 있으면 어떤 구현체가 어떻게 생성되는지 몰라도 된다
- Factory 패턴이 적용된 것이 컨테이너의 기능이며 이 Container의 기능을 제공해주는 것이 IoC 모듈

<br>

#### Inversion of Control 방식

![image](https://user-images.githubusercontent.com/30489264/134159565-365a59d8-35e4-49ee-91b0-3b5f2c2077b4.png)

- IoC 호출 방식
- 팩토리 패턴의 장점을 더해 어떤 것에도 의존하지 않는 형태가 된다
- Compile Time이 아닌 Runtime에 클래스 간 의존 관계가 형성된다

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xsi:schemaLocation="http://www.springframework.org/schema/beans
        https://www.springframework.org/schema/beans/spring-beans.xsd">

    <bean id="pizzaService" class="com.store.service.PizzaServiceImpl" />
    <bean id="burgerService" class="com.store.service.BurgerServiceImpl" />
</beans>
```

```java
ApplicationContext context = new ClassPathXmlApplicationContext("com/store/resources/applicationContext.xml");
ItemService pizzaService = context.getBean("pizzaService", PizzaService.class);
ItemService burgerService = context.getBean("burgerService", BurgerService.class);
```

- Service 구현체에 대한 객체의 생성 및 관리를 Spring IoC Container에게 위임한다


<br>

## DI - Dependency Injection

일반적인 Enterprise Application은 단일 객체(Bean)로 구성되지 않는다. OOP의 관점에서 애플리케이션은 자율적인 객체 간의 협력으로 이루어진다. DI는 **애플리케이션의 동작**이라는 목표를 달성하기 위한 객체간의 협력이 실현되기 위한 객체 정의 방식을 설명한다.

### Spring IoC Container의 Bean 관리

- `ApplicationContext`는 관리하는 Bean에 대해 1️⃣생성자 기반 DI 2️⃣세터 기반 DI를 지원한다
  - 또는 일부 종속성이 생성자 주입 방식에 의해 주입된 후 세터 주입 방식을 지원하기도 한다
- 주로 XML metadata에 Bean을 정의하거나 Annotation이 적용된 클래스에 대해 Bean을 생성하고 관리한다.

<br>

#### Container가 Bean에 의존성을 주입하는 과정

1. `ApplicationContext`을 생성하고 Bean에 대해 정의한 configuration metadata을 참조해서 초기화한다.
2. 모든 빈에 대해 의존성은 속성/생성자 인수/static-factory 메서드의 인수에 의해 결정된다. 이러한 의존성은 빈이 실제로 생성될 때 주입된다.
3. 각각의 property나 생성자 인수는 실제 값이거나 다른 빈에 대한 참조여야 한다.
4. property 또는 생성자 인수는 전달 받은 실제 형식으로 변환된다. Spring은 기본적으로 문자열 형식으로 제공된 값을 모든 기본 유형으로 변환 가능하다.

Spring Container는 생성과 동시에 모든 Bean의 Configuration을 확인하지만 Bean의 property는 실제로 생성되기 전 까지 정해지지 않는다.

singleton으로 정의되고 사전 인스턴스화(default) 설정된 Bean이 Container 생성과 동시에 생성되고, 그렇지 않은 Bean은 요청될 때 만들어진다.

<br>

#### Bean Life Cycle

객체를 사용할 때 마다 새로운 객체를 생성하여 반환하는 것이 아닌 **Singleton** 전략을 사용한다(`@Scope` Annotation을 통해 Life Cycle을 지정할 수 있다).

> `lazy-init="true"` 속성을 지정하여 Bean이 호출될 때 생성하는 지연 로딩을 설정할 수 있다.

|범위|설명|
|-|-|
|singleton|스프링 컨테이너 당 하나의 인스턴스 빈을 생성|
|prototype|컨테이너에 빈을 요청할 때 마다 새로운 인스턴스 생성|
|request|HTTP Request별로 새로운 인스턴스 생성|
|session|HTTP Session별로 새로운 인스턴스 생성|

#### Bean 등록 방식

Spring IoC 컨테이너에서는 Component Scan의 대상이 되는 package에 대해 `xml` metadata를 파싱하거나,  `@Component` Annotation이 붙은 객체를 Component Scan하여 `Bean`으로 등록한다.

<br>

#### Stereotype Annotation

특정 계층의 Bean에 부가 기능을 부여할 수 있다

|Stereotype|적용 대상|
|-|-|
|@Repository|DAO 또는 Repository 클래스에 적용<br> `DataAccessException` 자동 변환과 같은 AOP의 적용 대상을 선정하기 위해 사용|
|@Service|Service Layer의 클래스에 사용|
|@Controller|MVC Controller에 사용<br>스프링 웹 서블릿에 의해 웹 요청을 처리하는 컨트롤러 빈으로 설정|
|@Component|위의 Layer 구분을 적용하기 어려운 일반적인 경우 설정|


<br>

### DI의 장점

- 코드의 재사용성을 높여 작성된 모듈을 여러 곳에서 소스코드의 수정 없이 사용할 수 있다
- 결합도의 분리는 객체를 생성하고 의존성을 주입하는 데 효과적이다
- 의존하는 객체의 위치나 구체화한 클래스를 알지 않아도 된다
- 의존 관계 설정이 Compile Time이 아닌 Runtime에 이루어져 모듈 간의 결합도를 낮춘다
- Stub이나 Mock 객체 등을 활용한 단위 테스트의 편의성을 높여준다

<br>

### Field Dependency Injection(@Autowired)

<details>
<summary>@AutoWired ❓</summary>

- Spring Framework에서 지원하는 Dependency 정의 용도의 Annotation
- Spring 종속적이지만 정밀한 Dependency Injection이 필요한 경우 유용함
- 해당 어노테이션을 사용해 Bean을 등록할 경우 Injection의 대상이 되는 클래스의 형식은 하나여야 한다(하지만 @Qualifier를 이용해 Injection할 Component의 대상을 지정해줄 수 있다)
</details>

<br>

```java
@Service
public class Item {
  @Autowired
  private final Pizza pizza;
  @Autowired
  private final Burger burger;
}
```

`@Autowired` 어노테이션을 사용해 객체에 의존성을 주입하는 방식이다. 사용이 매우 간결해서 많이 이용되는 방식이다.

하지만 Intellij에서 이 방식을 사용할 경우 `Field injection is not recommended` 라는 경고 문구가 나타난다.

[스프링 공식문서](https://docs.spring.io/spring-framework/docs/current/reference/html/core.html#beans-factory-autowire)에서도 명시적 Wiring 방식보다 정확하지 않고, 예상치 못한 결과를 초래할 수 있으니 사용을 제한하라고 권유한다.

<br>

#### Field Injection을 권장하지 않는 이유

1. 외부에서 변경이 불가능하다
2. 객체 간 순환 종속성 문제가 발생할 수 있다
3. DI 프레임워크가 동작해야 객체가 생성된다

<br>

#### Circular Dependencies - 순환 종속성 문제

```shell
***************************
APPLICATION FAILED TO START
***************************

Description:

The dependencies of some of the beans in the application context form a cycle:

┌─────┐
|  memberServiceImpl defined in file [/examples/.../memberServiceImpl.class]
↑     ↓
|  ItemServiceImpl defined in file [/examples/.../ItemServiceImpl.class]
└─────┘
```

Bean의 생성은 잠재적으로, Bean의 의존성이 주입되고 의존성의 의존성이 주입될 때 형성된다.

따라서 종속성 간의 불일치 문제는 늦게 발견될 수 있는데, 이러한 현상이 최초 생성하거나 그 영향을 받는 Bean에게서 나타날 수 있다.

순환 종속성은 클래스 A가 생성자 주입을 통해 클래스 B의 인스턴스를 요구하고, 클래스 B가 생성자 주입을 통해 클래스 A의 인스턴스를 요구할 때 Container가 순환 참조를 탐지하고 예외를 발생시킨다.

> Bean이 존재하지 않거나 순환 종속성이 존재할 경우 이를 컨테이너 로드 시간에 탐지한다. 스프링은 Bean이 실제로 생성될 때 속성을 설정하고 종속성을 주입하는데, 잘못 로딩된 컨테이너는 이후 객체를 요청할 때 예외를 발생시킬 수 있으므로 기본적으로 싱글톤 & 사전 인스턴스 전략을 사용하는 이유가 된다. 따라서 실제로 필요하기 전 미리 만들기 위해 약간의 초기 시간과 메모리를 들여 ApplicationContext가 생성될 때 구성의 문제점을 발견할 수 있다.

- 객체 생성 시점에서 순환 종속성이 발생하는 것
- 객체 생성 후 비즈니스 로직 상에서 순환 종속성이 발생하는 것

의 경우, 후자가 실제 개발 시 훨씬 위험한 문제를 초래하기 때문에 Constructor Injection 방식을 사용하자.

<br>

### Constructor-based Dependency Injection

```java
@Service
public class Item {
  private final Pizza pizza;
  private final Burger burger;

  public Item(Pizza pizza, Burger burger) {
    // ...
  }
}

// Lombok을 이용한 생성자 주입 방식
@Service
@RequiredArgsConstructor
public class Item {
  private final Pizza pizza;
  private final Burger burger;

  // ...
}
```

<br>

#### 생성자 주입 방식의 장점

1. Container의 생성 시점에 순환 참조 문제를 발견할 수 있다.
2. `final` keyword를 이용해 객체를 immutable하게 만들 수 있다.
3. 필요한 종속성이 `null`이 아닌지 체크할 수 있다.
4. 항상 완전히 초기화된 상태에서 코드로 반환된다.
5. 너무 많은 인자를 가진 생성자는 Code Smell을 유발한다 -> 클래스가 너무 많은 책임을 가지고 있으니 적절한 관심사 분리를 통해 인수를 분리해야 함을 경고한다

<br>

### Setter-based Dependency Injection

```java
@Service
public class Item {
  private Pizza pizza;
  private Burger burger;

  public void setPizza(Pizza pizza) {
    this.pizza = pizza;
  }

  public void setBurger(Burger burger) {
    this.burger = burger;
  }

  // ...
}

// Lombok을 이용한 세터 주입 방식
@Service
@Setter
public class Item {
  private Pizza pizza;
  private Burger burger;

  // ...
}
```

- 인수가 없는 생성자를 호출 or 팩토리 메서드를 호출한 후 `setter` 메서드를 호출하는 컨테이너에 의해 수행
- 필수 종속성을 생성자 주입 방식으로 의존성을 주입받고, 선택적 종속성을 세터 주입 방식으로 의존성을 주입받는 것을 권장한다.
- 합리적인 기본값을 할당할 수 있는 선택적 종속성에만 사용해야 한다.
  - 그렇지 않은 경우 코드가 해당 의존성을 사용하는 모든 곳에서 `null` 검사를 수행해야 한다

<br>

## 참고자료

> - https://docs.spring.io/spring-framework/docs/current/reference/html/core.html#spring-core
> - https://mangkyu.tistory.com/125
> - https://jongmin92.github.io/2018/02/11/Spring/spring-ioc-di/

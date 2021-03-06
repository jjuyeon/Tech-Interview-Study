> [π‘](#ioc---inversion-of-control) Springμ IoC, DIμ λν΄ μ€λͺν΄μ£ΌμΈμ  
> π κ°μ²΄ κ°μ μμ‘΄μ±μ μ½λ λ΄μ κ΅¬ννλ©΄, ν΄λμ€μ λ³κ²½μ΄ λ°μν  λ ν΄λΉ ν΄λμ€λ₯Ό μ°Έμ‘°νλ ν΄λμ€μ μ½λ λν λͺ¨λ λ³κ²½ν΄μΌ νλ€. μ΄λ μ μ§λ³΄μμ±μ λ¨μ΄λ¨λ¦¬λ μΌμ΄λ€. IoC(Inversion of Control)μ κ°μ²΄ κ°μ μμ‘΄ κ΄κ³λ₯Ό μ»΄νμΌ μκ°μ΄ μλ λ°νμμ μ€μ ν¨μΌλ‘μ κ°μ²΄ κ° κ²°ν©λλ₯Ό λ¨μ΄λ¨λ € μ μ§λ³΄μμ±μ μ¦κ°μν€λ λ°©μμ΄λ€. μ€νλ§μμλ IoC μ»¨νμ΄λκ° DI(Dependency Injection - μμ‘΄μ± μ£Όμ)μ ν΅ν΄ μ΄λ₯Ό κ΅¬ννλ€.    
> μ€νλ§μ μμ‘΄μ± μ£Όμ λ°©μμ μμ±μ μ£Όμ, μΈν° μ£Όμ λ°©μμ΄ μλ€. μΌλ°μ μΌλ‘ κ°μ²΄μ λΆλ³μ± μ±μ§μ μ§ν¬ μ μκ³  νλμ NPEκ° λ°μνλ κ²μ λ§κΈ° μν μμ±μ μ£Όμ λ°©μμ΄ κΆμ₯λλ€.  
> Annotationμ μ΄μ©ν νλ μ£Όμ λ°©μλ μ‘΄μ¬νμ§λ§, μ΄λ Spring IoC μ»¨νμ΄λκ° λ‘λλκ³  μ΄λΈνμ΄μμ μ€μ ν νλμ ν΄λμ€μ μΌμΉνλ μμ‘΄μ±μ΄ μ£Όμλλ€. μ¦, Spring IoC μ»¨νμ΄λκ° μ€νλμ§ μμΌλ©΄ κ°μ²΄λ₯Ό μμ±ν  μ μλ€.

<br>

## IoC - Inversion of Control

: κ°μ²΄μ μ’μμ±μ λ€μμ λ°©μμ ν΅ν΄μλ§ μ μνλ λ°©μμ λ§νλ€. Springμμλ Dependency InjectionμΌλ‘ κ΅¬νλμλ€.

1. μμ±μ μΈμ
2. ν©ν λ¦¬ λ©μλμ μΈμ
3. ν©ν λ¦¬ λ©μλμμ μμ±λκ±°λ λ°νλμ΄ κ°μ²΄ μΈμ€ν΄μ€μ μ€μ λ μμ±

μ»¨νμ΄λλ λΉμ μμ±ν  λ μμ‘΄μ±μ μ£Όμνλ―λ‘ Compile Timeμ΄ μλ Runtimeμ κ°μ²΄ κ°μ μμ‘΄ κ΄κ³κ° κ²°μ λλ€. λ°λΌμ κ°μ²΄ κ°μ κ΄κ³κ° λμ¨νκ² μ°κ²°λλ€(loose coupling).

`org.springframework.beans`μ `org.springframework.context` packageλ Spring Frameworkμ IoC μ»¨νμ΄λλ₯Ό λ΄λΉνλ€.

- `BeanFactory` : λͺ¨λ  μ νμ κ°μ²΄λ₯Ό κ΄λ¦¬ν  μ μλ configuration λ©μ»€λμ¦ μ κ³΅
- `ApplicationContext` : `BeanFactory`λ₯Ό μμλ°μ Sub-Interface
  - λ λ§μ μν°νλΌμ΄μ¦λ³ κΈ°λ₯ μΆκ°
  - AOP κΈ°λ₯κ³Όμ κ°νΈν ν΅ν©
  - μΉ μ νλ¦¬μΌμ΄μμμ μ¬μ©νκΈ° μν `WebApplicationContext`μ κ°μ μ νλ¦¬μΌμ΄μ λ μ΄μ΄μ κ³μΈ΅λ³ μ»¨νμ€νΈ μ κ³΅

<br>

> __Bean__  
> 
> - Spring IoC μ»¨νμ΄λμ μν΄ μΈμ€ν΄μ€ν, μ‘°λ¦½λμ΄ κ΄λ¦¬λλ κ°μ²΄  
> - Beanκ³Ό μ νλ¦¬μΌμ΄μ κ°μ²΄ κ°μ μ’μμ±μ μ»¨νμ΄λμμ μ¬μ©νλ configuration metadataμ μν΄ λ°μ

<br>

### IoC Container

![image](https://user-images.githubusercontent.com/30489264/134152539-4a9fbe51-11c3-45cc-ac41-ce62b8321758.png)


- `org.springframework.context.ApplicationContext` μΈν°νμ΄μ€λ Spring IoC μ»¨νμ΄λλ₯Ό λνλ΄λ©°, μΈμ€ν΄μ€λ₯Ό κ΅¬μ±νκ³  λΉμ μμ±νλ€. 
- μ»¨νμ΄λλ configuration metadataλ₯Ό μ½μ΄ μ΄λ€ κ°μ²΄λ₯Ό μΈμ€ν΄μ€ν/κ΅¬μ±/μ‘°λ¦½ν  μ§μ λν μ λ³΄λ₯Ό μ»λλ€.
- Configuration metadataλ `XML`, `Java Annotations`, `Java Code`λ‘ λνλΌ μ μμ΅λλ€. μ νλ¦¬μΌμ΄μμ κ΅¬μ±νλ κ°μ²΄μ μ΄λ€κ°μ μνΈμμ‘΄μ±μ νννλ λ° μ¬μ©νλ€.

<br>

#### XML κΈ°λ° metadata

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

- `id` : λΉμ μ μνλ μλ³μ
- `class` : λΉμ νμμ κ²°μ νλ ν΄λμ€ μ΄λ¦

<br>

### IoCλ₯Ό μ¬μ©νλ μ΄μ 

> κ°μ²΄ κ°μ κ²°ν©λ(coupling)λ₯Ό λ?μΆλ€  
> κ²°ν©λκ° λμΌλ©΄ ν΄λΉ ν΄λμ€κ° μ μ§λ³΄μ λ  λ κ·Έ ν΄λμ€μ κ²°ν©λ λ€λ₯Έ ν΄λμ€λ€λ κ°μ΄ μμ ν΄μΌ νλ€ !!

<br>

#### ν΄λμ€ νΈμΆ λ°©μ

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

ν΄λμ€μμ μ§μ  λ€λ₯Έ ν΄λμ€λ₯Ό νΈμΆνλ λ°©μ

ν΄λμ€ λ΄μ μ μΈκ³Ό κ΅¬νμ΄ λͺ¨λ λμ΄ μκΈ° λλ¬Έμ μ°Έμ‘°μ€μΈ ν΄λμ€μ λ³νκ° μκΈ°λ©΄ μ°Έμ‘°ν ν΄λμ€μ μ½λ λν λ³κ²½ν΄μ€μΌ νλ€ !!

<br>

#### μΈν°νμ΄μ€ νΈμΆ λ°©μ

![image](https://user-images.githubusercontent.com/30489264/134157856-cb381a2f-5919-4655-be37-ce2cb3b74848.png)

- κ΅¬ν ν΄λμ€μ κ΅μ²΄κ° μ©μ΄ν΄μ λ€μν ννλ‘ λ³νκ° κ°λ₯νλ€
- μΈν°νμ΄μ€λ₯Ό κ΅μ²΄ν  μ νΈμΆ ν΄λμ€λ μμ ν΄μΌ νλ€

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

#### ν©ν λ¦¬ νΈμΆ λ°©μ

![image](https://user-images.githubusercontent.com/30489264/134158082-5aea8112-fec3-40b5-9343-52df489b7ce0.png)

- ν©ν λ¦¬κ° κ΅¬ν ν΄λμ€λ₯Ό μμ±νκ³  ν΄λΉ ν©ν λ¦¬λ₯Ό νΈμΆνλ λ°©μ
- μΈν°νμ΄μ€ λ³κ²½ μ ν©ν λ¦¬λ§ μμ νλ©΄ λμ΄ νΈμΆ ν΄λμ€μ μν₯μ λ―ΈμΉμ§ μμ
- νμ§λ§ ν΄λμ€μ ν©ν λ¦¬λ₯Ό νΈμΆνλ μ½λκ° μ½μλκΈ° λλ¬Έμ ν©ν λ¦¬ μμ‘΄μ 

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

- κ° Serviceλ₯Ό μμ±νμ¬ λ°ννλ Factoryλ₯Ό μ¬μ©
- Serviceλ₯Ό μ΄μ©νλ μͺ½ μμλ Interfaceλ§ μκ³  μμΌλ©΄ μ΄λ€ κ΅¬νμ²΄κ° μ΄λ»κ² μμ±λλμ§ λͺ°λΌλ λλ€
- Factory ν¨ν΄μ΄ μ μ©λ κ²μ΄ μ»¨νμ΄λμ κΈ°λ₯μ΄λ©° μ΄ Containerμ κΈ°λ₯μ μ κ³΅ν΄μ£Όλ κ²μ΄ IoC λͺ¨λ

<br>

#### Inversion of Control λ°©μ

![image](https://user-images.githubusercontent.com/30489264/134159565-365a59d8-35e4-49ee-91b0-3b5f2c2077b4.png)

- IoC νΈμΆ λ°©μ
- ν©ν λ¦¬ ν¨ν΄μ μ₯μ μ λν΄ μ΄λ€ κ²μλ μμ‘΄νμ§ μλ ννκ° λλ€
- Compile Timeμ΄ μλ Runtimeμ ν΄λμ€ κ° μμ‘΄ κ΄κ³κ° νμ±λλ€

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

- Service κ΅¬νμ²΄μ λν κ°μ²΄μ μμ± λ° κ΄λ¦¬λ₯Ό Spring IoC Containerμκ² μμνλ€


<br>

## DI - Dependency Injection

μΌλ°μ μΈ Enterprise Applicationμ λ¨μΌ κ°μ²΄(Bean)λ‘ κ΅¬μ±λμ§ μλλ€. OOPμ κ΄μ μμ μ νλ¦¬μΌμ΄μμ μμ¨μ μΈ κ°μ²΄ κ°μ νλ ₯μΌλ‘ μ΄λ£¨μ΄μ§λ€. DIλ **μ νλ¦¬μΌμ΄μμ λμ**μ΄λΌλ λͺ©νλ₯Ό λ¬μ±νκΈ° μν κ°μ²΄κ°μ νλ ₯μ΄ μ€νλκΈ° μν κ°μ²΄ μ μ λ°©μμ μ€λͺνλ€.

### Spring IoC Containerμ Bean κ΄λ¦¬

- `ApplicationContext`λ κ΄λ¦¬νλ Beanμ λν΄ 1οΈβ£μμ±μ κΈ°λ° DI 2οΈβ£μΈν° κΈ°λ° DIλ₯Ό μ§μνλ€
  - λλ μΌλΆ μ’μμ±μ΄ μμ±μ μ£Όμ λ°©μμ μν΄ μ£Όμλ ν μΈν° μ£Όμ λ°©μμ μ§μνκΈ°λ νλ€
- μ£Όλ‘ XML metadataμ Beanμ μ μνκ±°λ Annotationμ΄ μ μ©λ ν΄λμ€μ λν΄ Beanμ μμ±νκ³  κ΄λ¦¬νλ€.

<br>

#### Containerκ° Beanμ μμ‘΄μ±μ μ£Όμνλ κ³Όμ 

1. `ApplicationContext`μ μμ±νκ³  Beanμ λν΄ μ μν configuration metadataμ μ°Έμ‘°ν΄μ μ΄κΈ°ννλ€.
2. λͺ¨λ  λΉμ λν΄ μμ‘΄μ±μ μμ±/μμ±μ μΈμ/static-factory λ©μλμ μΈμμ μν΄ κ²°μ λλ€. μ΄λ¬ν μμ‘΄μ±μ λΉμ΄ μ€μ λ‘ μμ±λ  λ μ£Όμλλ€.
3. κ°κ°μ propertyλ μμ±μ μΈμλ μ€μ  κ°μ΄κ±°λ λ€λ₯Έ λΉμ λν μ°Έμ‘°μ¬μΌ νλ€.
4. property λλ μμ±μ μΈμλ μ λ¬ λ°μ μ€μ  νμμΌλ‘ λ³νλλ€. Springμ κΈ°λ³Έμ μΌλ‘ λ¬Έμμ΄ νμμΌλ‘ μ κ³΅λ κ°μ λͺ¨λ  κΈ°λ³Έ μ νμΌλ‘ λ³ν κ°λ₯νλ€.

Spring Containerλ μμ±κ³Ό λμμ λͺ¨λ  Beanμ Configurationμ νμΈνμ§λ§ Beanμ propertyλ μ€μ λ‘ μμ±λκΈ° μ  κΉμ§ μ ν΄μ§μ§ μλλ€.

singletonμΌλ‘ μ μλκ³  μ¬μ  μΈμ€ν΄μ€ν(default) μ€μ λ Beanμ΄ Container μμ±κ³Ό λμμ μμ±λκ³ , κ·Έλ μ§ μμ Beanμ μμ²­λ  λ λ§λ€μ΄μ§λ€.

<br>

#### Bean Life Cycle

κ°μ²΄λ₯Ό μ¬μ©ν  λ λ§λ€ μλ‘μ΄ κ°μ²΄λ₯Ό μμ±νμ¬ λ°ννλ κ²μ΄ μλ **Singleton** μ λ΅μ μ¬μ©νλ€(`@Scope` Annotationμ ν΅ν΄ Life Cycleμ μ§μ ν  μ μλ€).

> `lazy-init="true"` μμ±μ μ§μ νμ¬ Beanμ΄ νΈμΆλ  λ μμ±νλ μ§μ° λ‘λ©μ μ€μ ν  μ μλ€.

|λ²μ|μ€λͺ|
|-|-|
|singleton|μ€νλ§ μ»¨νμ΄λ λΉ νλμ μΈμ€ν΄μ€ λΉμ μμ±|
|prototype|μ»¨νμ΄λμ λΉμ μμ²­ν  λ λ§λ€ μλ‘μ΄ μΈμ€ν΄μ€ μμ±|
|request|HTTP Requestλ³λ‘ μλ‘μ΄ μΈμ€ν΄μ€ μμ±|
|session|HTTP Sessionλ³λ‘ μλ‘μ΄ μΈμ€ν΄μ€ μμ±|

#### Bean λ±λ‘ λ°©μ

Spring IoC μ»¨νμ΄λμμλ Component Scanμ λμμ΄ λλ packageμ λν΄ `xml` metadataλ₯Ό νμ±νκ±°λ,  `@Component` Annotationμ΄ λΆμ κ°μ²΄λ₯Ό Component Scanνμ¬ `Bean`μΌλ‘ λ±λ‘νλ€.

<br>

#### Stereotype Annotation

νΉμ  κ³μΈ΅μ Beanμ λΆκ° κΈ°λ₯μ λΆμ¬ν  μ μλ€

|Stereotype|μ μ© λμ|
|-|-|
|@Repository|DAO λλ Repository ν΄λμ€μ μ μ©<br> `DataAccessException` μλ λ³νκ³Ό κ°μ AOPμ μ μ© λμμ μ μ νκΈ° μν΄ μ¬μ©|
|@Service|Service Layerμ ν΄λμ€μ μ¬μ©|
|@Controller|MVC Controllerμ μ¬μ©<br>μ€νλ§ μΉ μλΈλ¦Ώμ μν΄ μΉ μμ²­μ μ²λ¦¬νλ μ»¨νΈλ‘€λ¬ λΉμΌλ‘ μ€μ |
|@Component|μμ Layer κ΅¬λΆμ μ μ©νκΈ° μ΄λ €μ΄ μΌλ°μ μΈ κ²½μ° μ€μ |


<br>

### DIμ μ₯μ 

- μ½λμ μ¬μ¬μ©μ±μ λμ¬ μμ±λ λͺ¨λμ μ¬λ¬ κ³³μμ μμ€μ½λμ μμ  μμ΄ μ¬μ©ν  μ μλ€
- κ²°ν©λμ λΆλ¦¬λ κ°μ²΄λ₯Ό μμ±νκ³  μμ‘΄μ±μ μ£Όμνλ λ° ν¨κ³Όμ μ΄λ€
- μμ‘΄νλ κ°μ²΄μ μμΉλ κ΅¬μ²΄νν ν΄λμ€λ₯Ό μμ§ μμλ λλ€
- μμ‘΄ κ΄κ³ μ€μ μ΄ Compile Timeμ΄ μλ Runtimeμ μ΄λ£¨μ΄μ Έ λͺ¨λ κ°μ κ²°ν©λλ₯Ό λ?μΆλ€
- Stubμ΄λ Mock κ°μ²΄ λ±μ νμ©ν λ¨μ νμ€νΈμ νΈμμ±μ λμ¬μ€λ€

<br>

### Field Dependency Injection(@Autowired)

<details>
<summary>@AutoWired β</summary>

- Spring Frameworkμμ μ§μνλ Dependency μ μ μ©λμ Annotation
- Spring μ’μμ μ΄μ§λ§ μ λ°ν Dependency Injectionμ΄ νμν κ²½μ° μ μ©ν¨
- ν΄λΉ μ΄λΈνμ΄μμ μ¬μ©ν΄ Beanμ λ±λ‘ν  κ²½μ° Injectionμ λμμ΄ λλ ν΄λμ€μ νμμ νλμ¬μΌ νλ€(νμ§λ§ @Qualifierλ₯Ό μ΄μ©ν΄ Injectionν  Componentμ λμμ μ§μ ν΄μ€ μ μλ€)
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

`@Autowired` μ΄λΈνμ΄μμ μ¬μ©ν΄ κ°μ²΄μ μμ‘΄μ±μ μ£Όμνλ λ°©μμ΄λ€. μ¬μ©μ΄ λ§€μ° κ°κ²°ν΄μ λ§μ΄ μ΄μ©λλ λ°©μμ΄λ€.

νμ§λ§ Intellijμμ μ΄ λ°©μμ μ¬μ©ν  κ²½μ° `Field injection is not recommended` λΌλ κ²½κ³  λ¬Έκ΅¬κ° λνλλ€.

[μ€νλ§ κ³΅μλ¬Έμ](https://docs.spring.io/spring-framework/docs/current/reference/html/core.html#beans-factory-autowire)μμλ λͺμμ  Wiring λ°©μλ³΄λ€ μ ννμ§ μκ³ , μμμΉ λͺ»ν κ²°κ³Όλ₯Ό μ΄λν  μ μμΌλ μ¬μ©μ μ ννλΌκ³  κΆμ νλ€.

<br>

#### Field Injectionμ κΆμ₯νμ§ μλ μ΄μ 

1. μΈλΆμμ λ³κ²½μ΄ λΆκ°λ₯νλ€
2. κ°μ²΄ κ° μν μ’μμ± λ¬Έμ κ° λ°μν  μ μλ€
3. DI νλ μμν¬κ° λμν΄μΌ κ°μ²΄κ° μμ±λλ€

<br>

#### Circular Dependencies - μν μ’μμ± λ¬Έμ 

```shell
***************************
APPLICATION FAILED TO START
***************************

Description:

The dependencies of some of the beans in the application context form a cycle:

βββββββ
|  memberServiceImpl defined in file [/examples/.../memberServiceImpl.class]
β     β
|  ItemServiceImpl defined in file [/examples/.../ItemServiceImpl.class]
βββββββ
```

Beanμ μμ±μ μ μ¬μ μΌλ‘, Beanμ μμ‘΄μ±μ΄ μ£Όμλκ³  μμ‘΄μ±μ μμ‘΄μ±μ΄ μ£Όμλ  λ νμ±λλ€.

λ°λΌμ μ’μμ± κ°μ λΆμΌμΉ λ¬Έμ λ λ¦κ² λ°κ²¬λ  μ μλλ°, μ΄λ¬ν νμμ΄ μ΅μ΄ μμ±νκ±°λ κ·Έ μν₯μ λ°λ Beanμκ²μ λνλ  μ μλ€.

μν μ’μμ±μ ν΄λμ€ Aκ° μμ±μ μ£Όμμ ν΅ν΄ ν΄λμ€ Bμ μΈμ€ν΄μ€λ₯Ό μκ΅¬νκ³ , ν΄λμ€ Bκ° μμ±μ μ£Όμμ ν΅ν΄ ν΄λμ€ Aμ μΈμ€ν΄μ€λ₯Ό μκ΅¬ν  λ Containerκ° μν μ°Έμ‘°λ₯Ό νμ§νκ³  μμΈλ₯Ό λ°μμν¨λ€.

> Beanμ΄ μ‘΄μ¬νμ§ μκ±°λ μν μ’μμ±μ΄ μ‘΄μ¬ν  κ²½μ° μ΄λ₯Ό μ»¨νμ΄λ λ‘λ μκ°μ νμ§νλ€. μ€νλ§μ Beanμ΄ μ€μ λ‘ μμ±λ  λ μμ±μ μ€μ νκ³  μ’μμ±μ μ£Όμνλλ°, μλͺ» λ‘λ©λ μ»¨νμ΄λλ μ΄ν κ°μ²΄λ₯Ό μμ²­ν  λ μμΈλ₯Ό λ°μμν¬ μ μμΌλ―λ‘ κΈ°λ³Έμ μΌλ‘ μ±κΈν€ & μ¬μ  μΈμ€ν΄μ€ μ λ΅μ μ¬μ©νλ μ΄μ κ° λλ€. λ°λΌμ μ€μ λ‘ νμνκΈ° μ  λ―Έλ¦¬ λ§λ€κΈ° μν΄ μ½κ°μ μ΄κΈ° μκ°κ³Ό λ©λͺ¨λ¦¬λ₯Ό λ€μ¬ ApplicationContextκ° μμ±λ  λ κ΅¬μ±μ λ¬Έμ μ μ λ°κ²¬ν  μ μλ€.

- κ°μ²΄ μμ± μμ μμ μν μ’μμ±μ΄ λ°μνλ κ²
- κ°μ²΄ μμ± ν λΉμ¦λμ€ λ‘μ§ μμμ μν μ’μμ±μ΄ λ°μνλ κ²

μ κ²½μ°, νμκ° μ€μ  κ°λ° μ ν¨μ¬ μνν λ¬Έμ λ₯Ό μ΄λνκΈ° λλ¬Έμ Constructor Injection λ°©μμ μ¬μ©νμ.

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

// Lombokμ μ΄μ©ν μμ±μ μ£Όμ λ°©μ
@Service
@RequiredArgsConstructor
public class Item {
  private final Pizza pizza;
  private final Burger burger;

  // ...
}
```

<br>

#### μμ±μ μ£Όμ λ°©μμ μ₯μ 

1. Containerμ μμ± μμ μ μν μ°Έμ‘° λ¬Έμ λ₯Ό λ°κ²¬ν  μ μλ€.
2. `final` keywordλ₯Ό μ΄μ©ν΄ κ°μ²΄λ₯Ό immutableνκ² λ§λ€ μ μλ€.
3. νμν μ’μμ±μ΄ `null`μ΄ μλμ§ μ²΄ν¬ν  μ μλ€.
4. ν­μ μμ ν μ΄κΈ°νλ μνμμ μ½λλ‘ λ°νλλ€.
5. λλ¬΄ λ§μ μΈμλ₯Ό κ°μ§ μμ±μλ Code Smellμ μ λ°νλ€ -> ν΄λμ€κ° λλ¬΄ λ§μ μ±μμ κ°μ§κ³  μμΌλ μ μ ν κ΄μ¬μ¬ λΆλ¦¬λ₯Ό ν΅ν΄ μΈμλ₯Ό λΆλ¦¬ν΄μΌ ν¨μ κ²½κ³ νλ€

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

// Lombokμ μ΄μ©ν μΈν° μ£Όμ λ°©μ
@Service
@Setter
public class Item {
  private Pizza pizza;
  private Burger burger;

  // ...
}
```

- μΈμκ° μλ μμ±μλ₯Ό νΈμΆ or ν©ν λ¦¬ λ©μλλ₯Ό νΈμΆν ν `setter` λ©μλλ₯Ό νΈμΆνλ μ»¨νμ΄λμ μν΄ μν
- νμ μ’μμ±μ μμ±μ μ£Όμ λ°©μμΌλ‘ μμ‘΄μ±μ μ£Όμλ°κ³ , μ νμ  μ’μμ±μ μΈν° μ£Όμ λ°©μμΌλ‘ μμ‘΄μ±μ μ£Όμλ°λ κ²μ κΆμ₯νλ€.
- ν©λ¦¬μ μΈ κΈ°λ³Έκ°μ ν λΉν  μ μλ μ νμ  μ’μμ±μλ§ μ¬μ©ν΄μΌ νλ€.
  - κ·Έλ μ§ μμ κ²½μ° μ½λκ° ν΄λΉ μμ‘΄μ±μ μ¬μ©νλ λͺ¨λ  κ³³μμ `null` κ²μ¬λ₯Ό μνν΄μΌ νλ€

<br>

## μ°Έκ³ μλ£

> - https://docs.spring.io/spring-framework/docs/current/reference/html/core.html#spring-core
> - https://mangkyu.tistory.com/125
> - https://jongmin92.github.io/2018/02/11/Spring/spring-ioc-di/

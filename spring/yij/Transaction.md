> [💡] JPA의 Propagation에 대해 설명해주세요

## Transaction of Spring

Spring에서 Transaction 설정을 하기 위해서는

1. Annotation
2. AOP

를 활용할 수 있다.

`@Transactional` 어노테이션을 활용하면 트랜잭션 설정을 할 수 있으며, 몇 가지 주의 사항이 존재한다.

### Configure Transaction

<details>
<summary>Java Configuration 코드</summary>

```java
@Configuration
@EnableTransactionManagement
public class PersistenceJPAConfig{

   @Bean
   public LocalContainerEntityManagerFactoryBean
     entityManagerFactoryBean(){
      //...
   }

   @Bean
   public PlatformTransactionManager transactionManager(){
      JpaTransactionManager transactionManager
        = new JpaTransactionManager();
      transactionManager.setEntityManagerFactory(
        entityManagerFactoryBean().getObject() );
      return transactionManager;
   }
}
```

</details>

<details>
<summary>XML Configuration 코드</summary>

```xml
<bean id="txManager" class="org.springframework.orm.jpa.JpaTransactionManager">
   <property name="entityManagerFactory" ref="myEmf" />
</bean>
<tx:annotation-driven transaction-manager="txManager" />
```
</details>
<details>
<summary>Annotation Configuration 코드</summary>

```java
@Service
@Transactional
public class FooService {
    //...
}
```
</details>

<br>

Spring 3.1에서는 `@EnableTransactionManagement` 어노테이션과 `@Configuration` 어노테이션을 사용해서 트랜잭션을 사용할 것 이라고 명시해야 한다.

하지만 Spring Boot를 사용하면 `spring-data-` 혹은 `spring-tx` 의존성을 사용하면 트랜잭션 설정을 default로 할 수 있다.

<br>

#### Annotation Configuration 

`@Transactional` Annotation을 사용하면 다음과 같은 설정도 할 수 있다.

- 트랜잭션의 **Propagation Type**
- 트랜잭션의 **Isolation Level**
- 트랜잭션에 의해 래핑된 작업의 **Time Out**
- **readOnly** flag - 트랜잭션이 읽기 전용인지
- **Rollback** 규칙

default 설정에 의해 rollback이 runtime에 발생하기 때문에, **unchecked exception**이 발생할 수 있다(checked exception은 rollback을 트리거하지 않는다❗❗). `rollbackFor`와 `noRollbackFor`를 통해 이를 제어할 수 있다.

<br>

### Transaction Propagation

비즈니스 로직과 트랜잭션의 경계를 정의하는 수준

Spring은 propagation setting에 따라 트랜잭션을 시작하고 중단하며 관리한다.

propagation 단계에 따라 트랜잭션을 가져오거나 생성하기 위해 `TransactionManager::getTransaction`을 호출한다.

<br>

### Annotation 사용의 잠재적 위험

#### Transaction과 Proxy

Spring은 `@Transactional`이 붙은 클래스(또는 메소드)에 대해 프록시를 생성한다. 이 프록시는 트랜잭션의 시작과 커밋을 위해 메서드를 실행하기 전 트랜잭션 로직을 주입한다.

중요한 것은 만약 Transactional Bean이 **Interface를 implements**할 경우, 기본으로 `Java Dynamic Proxy`를 사용하기 때문에 프록시를 통해 들어오는 외부 메서드 호출만 처리하게 된다. 메서드에 `@Transactional`가 있는 경우에도 클래스 내부 호출은 트랜잭션을 시작하지 않는다.

또한 `public` 메서드에만 `@Transactional`이 적용된다. 다른 visibility를 가진 메서드는 프록시 되지 않기 때문에 설정이 무시된다.

<br>

#### Isolation Level 변경

```java
@Transactional(isolation = Isolation.SERIALIZABLE)
```

isolation 레벨 설정은 Spring 4.1 이후에만 적용 가능하다.

<br>

#### Read-Only Transaction

> `readOnly`가 write access시도가 반드시 실패하는 것은 아니다 ❗❗ transaction manager는 read-only를 해석하지 못할 때 read-only 요청에 대해 예외를 throw하지 않는다.

즉, `readOnly`가 설정되었다고 삽입 또는 수정이 발생하지 않을 것이라 확신할 수 없다. 

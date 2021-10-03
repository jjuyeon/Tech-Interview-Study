# Aspect-Oriented Programming

> Aspect-oriented Programming(AOP) complements Object-oriented Programming(OOP) by providing another way of thinking about program structure  
> *관점 지향 프로그래밍은 프로그램 구조에 대한 다른 사고방식을 제공해서 객체 지향 프로그래밍을 보완한다*    
> 👉 [Aspect Oriented Programming with Spring](https://docs.spring.io/spring-framework/docs/current/reference/html/core.html#aop)

Spring Framework Docs의 AOP를 설명하는 첫 문단이다

절차 지향 프로그래밍과 객체 지향 프로그래밍의 관계처럼 OOP와 AOP를 분리하지 말라는 뜻이기도 하다

OOP의 모듈화 단위가 **Class**라면 AOP의 모듈화 단위는 **Aspect**이다

Aspect는 여러 서비스 혹은 객체에 공통적으로 적용될 수 있는 관심사(로깅, 트랜잭션 등)를 모듈화한다(이렇게 공통적으로 적용될 수 있는 관심사를 *cross-cutting concern*이라고 한다)

AOP는 Spring Triangle을 이루는 핵심 요소다. Spring IoC 컨테이너는 AOP에 의존하진 않지만(AOP를 꼭 사용해야 하는건 아니지만) AOP는 IoC에 적용할 수 있는 유용한 미들웨어 솔루션을 제공한다

> Spring은 schema-based 접근 혹은 `@AspectJ` 어노테이션을 이용해 aspect 작성을 제공한다

<br>

## AOP Terminology

AOP는 Spring Framework에서만 존재하는 개념은 아니다

아래에서 AOP에서 사용하는 대표적인 용어들을 정의한다(직관적이진 않다)

### Aspect

여러 클래스에 공통적으로 적용할 수 있는 모듈화 된 관심사를 의미한다

Transaction Management는 자바 기반 애플리케이션에서 cross-cutting 관심사의 대표적인 예시다

Spring AOP에서는 `@Aspect` 어노테이션이 적용된 클래스를 통해 aspect가 구현된다

### Join Point

*메서드 실행* or *예외 처리*와 같은 프로그램 실행 시점을 의미한다(Spring에서는 항상 메서드 실행)

### Advice

특정 **Join Point**에서 **Aspect**가 수행하는 행위

Advice의 종류에는 `around`, `before`, `after`가 존재한다

스프링을 포함한 많은 AOP Framework는 advice를 interceptor로 모델링해서 join point를 interceptor 체인으로 감싼다

#### Advice 종류

- Before advice: Join Point 이전에 실행되지만 (exception을 throw하지 않는 한) 진행중인 Join Point의 flow를 끊을 수 있는 권한은 없는 Advice
- After returning advice: Join Point가 정상적으로 완료된 후 실행할 Advice(메서드가 exception을 throw하지 않고 정상 반환될 경우)
- After throwing advice: exception을 throw할 경우 실행할 Advice
- After (finally) advice: Join Point가 반환되는 수단(정상이던 exception throw이던)에 관계없이 실행할 Advice
- Around advice: 메서드 호출과 같은 Join Point 실행 전후를 둘러싼 Advice. 
  - 메서드 호출 전후 사용자 지정 동작을 수행할 수 있음
  - 자체 값을 반환하거나 예외를 발생시켜 메서드를 실행함으로써 어떤 Join Point를 수행할 것 인지 결정할 수 있음

### Pointcut

Join Point와 일치하는 수식

특정 **Pointcut** 수식에 일치하는 **Join Point**(ex. 특정 이름 메서드 실행)에서 **Advice**가 실행된다

Pointcut expression과 일치하는 Join Point의 개념은 AOP의 핵심 개념이고, Spring은 기본적으로 AspectJ pointcut expression 언어를 사용한다.

👉 [Pointcut Expression 문법 및 예시](https://docs.spring.io/spring-framework/docs/current/reference/html/core.html#aop-pointcuts)

### 이외의 용어

- Target Object: 하나 이상의 Aspect에서 Advice가 적용된 객체. Spring AOP는 Runtime Proxy를 사용하여 구현되므로 항상 Proxy 객체가 된다
- [AOP Proxy](https://docs.spring.io/spring-framework/docs/current/reference/html/core.html#aop-understanding-aop-proxies): (Advice 메서드 실행 등등)의 Aspect 행위를 구현하기 위해 AOP Framework에서 생성한 프록시 객체
- Weaving
- Introduction

### AOP 적용 예시

![image](https://user-images.githubusercontent.com/30489264/135737686-e837c6e0-8eaa-4c23-8c50-25385a7dbd56.png)

- AspectJ 프레임워크를 이용해 AOP를 구현했다
- `logging` 메서드의 실행 시점은 Advice가 적용될 Join Point이다
- `@Before` 어노테이션은 Join Point에서 적용될 Advice의 종류를 의미한다
  - 해당 Join Point(메서드 실행) 이전에 Advice(로깅)이 실행된다
- `"execution(* com.ssafy.~~)"`는 Pointcut으로 해당 aspectj pointcut expression을 만족하는 Join Point에 Advice를 적용시키겠다는 의미이다

<br>

## Spring AOP의 가용성과 목표

- 순수 Java로 구현되어 서블릿 컨테이너 혹은 애플리케이션 서버에서 사용하기에 적합하다
- 현재 메서드 실행에 대한 Join Point만 지원한다(필드 Interception과 Join Point에 대한 더 많은 지원을 원한다면 AspectJ를 고려하라)
- Spring AOP의 목표는 AOP 구현과 Spring IoC 컨테이너 간의 밀접한 통합을 제공해서 엔터프라이즈 애플리케이션의 일반적 문제를 해결하는 것이다
  - Bean을 이용해 Aspect가 구성되기 때문에 Java 애플리케이션의 일반적 문제에 대한 솔루션에는 적합
  - 도메인 객체에 대한 세분화된 Advice 작업은 AspectJ가 적합

<br>

## AspectJ 기반 AOP 사용

일반 Java 클래스에 `@AspectJ`어노테이션을 적용하면 Aspect를 선언할 수 있다

Spring은 Pointcut 구문 분석 및 매칭을 위해 AspectJ의 라이브러를 사용하여 해석한다

AOP 런타임은 순수 Spring AOP이기 때문에 AspectJ 컴파일러나 위버에 의존하지 않는다

<br>

### Enabling @AspectJ

@AspectJ의 Aspect를 Spring Configuration으로 등록하기 위해 AspectJ 기반의 Spring AOP와 auto-proxy(오토프록시) bean을 활성화해야 한다

Auto-proxying은 스프링이 하나 이상의 aspect에서 advice된다고 결정했을 때 자동으로 해당 bean에 대해 프록시를 생성한다. 이는 메서드 호출을 가로채고 필요에 따라 advice가 실행되는 것을 보장한다.

#### Java Configuration

```java
@Configuration
@EnableAspectJAutoProxy
public class AppConfig {

}
```

#### XML Configuration

```xml
<aop:aspectj-autoproxy/>
```

이후의 설정은 모두 Java 기반으로 설명한다

<br>

### Declaring an Aspect

@Aspect support를 활성화하면 애플리케이션의 컨텍스트에서 @Aspect로 정의된 빈이 자동으로 감지되며, Spring AOP를 구성하는 데 사용된다

예시는 최소한의 AOP 설정이다

```java
package org.xyz;
import org.aspectj.lang.annotation.Aspect;

@Aspect
public class NotVeryUsefulAspect {

}
```

다른 클래스와 마찬가지로 메서드와 필드를 가질 수 있으며 Pointcut, Advice, Introduction을 설정할 수 있다

### AOP 예시

```java
@Aspect
public class ConcurrentOperationExecutor implements Ordered {

    private static final int DEFAULT_MAX_RETRIES = 2;

    private int maxRetries = DEFAULT_MAX_RETRIES;
    private int order = 1;

    public void setMaxRetries(int maxRetries) {
        this.maxRetries = maxRetries;
    }

    public int getOrder() {
        return this.order;
    }

    public void setOrder(int order) {
        this.order = order;
    }

    @Around("com.xyz.myapp.CommonPointcuts.businessService()")
    public Object doConcurrentOperation(ProceedingJoinPoint pjp) throws Throwable {
        int numAttempts = 0;
        PessimisticLockingFailureException lockFailureException;
        do {
            numAttempts++;
            try {
                return pjp.proceed();
            }
            catch(PessimisticLockingFailureException ex) {
                lockFailureException = ex;
            }
        } while(numAttempts <= this.maxRetries);
        throw lockFailureException;
    }
}
```
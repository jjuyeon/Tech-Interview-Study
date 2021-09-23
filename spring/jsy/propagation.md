# :question: JPA propagation

#### reference
https://www.youtube.com/watch?v=e9PC0sroCzc<br>
https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/transaction/annotation/Propagation.html<br>
https://stackoverflow.com/a/27184785<br>
https://deveric.tistory.com/86<br>
https://n1tjrgns.tistory.com/266
<hr>

## Question
1. JPA Propagation 전파단계를 설명해주세요.
- JPA Propagation은 트랜잭션 동작시, 다른 트랜잭션을 호출하는 상황에 선택할 수 있는 옵션입니다. @Transactional의 propagation 속성을 통해 호출 당하는 트랜잭션의 입장에서는 호출한 쪽의 트랜잭션을 그대로 사용할 수도 있고, 새롭게 트랜잭션을 생성할 수도 있습니다.
- Propagation 전파단계에는 7가지의 단계가 존재합니다.
  1. **REQUIRED**: 부모 트랜잭션이 존재한다면 그 트랜잭션 안에 포함되어 동작하며, 부모 트랜잭션이 없을 경우 새로운 트랜잭션을 생성합니다. (default 속성)
  2. **REQUIRES_NEW**: 부모 트랜잭션의 존재 여부와 상관없이 매번 새로운 트랜잭션을 실행합니다.
  3. **NESTED**: 부모 트랜잭션이 존재하면 부모 트랜잭션 안에 중첩 트랜잭션을 만듭니다. 부모 트랜잭션의 커밋과 롤백에 영향을 받지만, 자신의 커밋과 롤백은 부모 트랜잭션에 영향을 주지 않습니다.
  4. **MANDATORY**: 부모 트랜잭션이 존재하면 그 트랜잭션 안에 포함되어 동작하며, 부모 트랜잭션이 없다면 예외를 발생시킨다.
  5. **SUPPORT**: 부모 트랜잭션이 존재하면 그 트랜잭션 안에 포함되어 동작하며, 부모 트랜잭션이 없다면 트랜잭션 없이 동작한다.
  6. **NOT_SUPPORTED**: 부모 트랜잭션이 존재하면 부모 트랜잭션을 일시정지하고 기능을 수행한다.
  7. **NEVER**: 트랜잭션을 사용하지 않도록 강제한다. 부모 트랜잭션이 없다면 예외를 발생시킨다.
<hr>

## :nerd_face:	What I study
### Propagation
- `@Transactional`를 가진 함수가 중첩으로 호출되었을 때, 어떻게 동작 시킬 것인지에 대해 결정할 수 있는 옵션
- 트랜잭션이 시작하거나, 참여하는 방법에 대해서도 설정할 수 있다.

```java
public class Car {

	private Taxi taxi;
        
    @Transactional
    public void drive() {
    	taxi.run(); // 트랜잭션 중첩 발생
    }
}

public class Taxi {
	@Transactional
    public void run() {...}
}
```

#### cf) @Transactional
- 선언적 트랜잭션 지원(스프링에서 트랜잭션을 처리하는 방식)
- 스프링 프레임워크는 내부적으로 AOP를 통해서 해당 어노테이션을 인지하고 프록시를 생성해서 트랜잭션을 자동 관리한다.
1. @Transactional을 메소드 또는 클래스에 지정하면, 해당 메소드 호출시 지정된 트랜잭션이 작동하게 된다.
  - 특정 메소드에 지정하면, 해당 메소드가 단일 트랜잭션 내에서 실행된다.
  - 특정 클래스에 지정하면, 해당 클래스의 모든 메소드를 단일 트랜잭션 내에서 실행한다.
2. 단, 해당 클래스의 Bean이 다른 클래스의 Bean에서 호출될 때만 @Transactional을 인지하고 작동하게 된다.
  - 같은 Bean 내에서 @Transactional이 명시된 다른 메소드를 호출해도 작동하지 않는다.

<br><br>

![propagation](https://media.vlpt.us/images/syleemk/post/eb89d47f-7498-4657-af97-6c6251361488/image.png)

### 1. Propagation.REQUIRED
- default 값이므로, 생략이 가능하다.
- 부모 트랜잭션(자신을 호출한 메소드의 트랜잭션)이 존재한다면 부모 트랜잭션으로 합류하고, 부모 트랜잭션이 없다면 새로운 트랜잭션을 생성한다.
- 즉, 모든 처리 방식이 **하나의** 트랜잭션으로 진행이 된다.
- 해당 메소드를 호출한 곳에 별도의 트랜잭션이 설정되어 있지 **않았다면**, 트랜잭션을 **새로 시작한다.** (새로운 연결을 생성하고 실행)
- 만약 호출한 곳에 이미 트랜잭션이 설정되어 **있었다면**, **기존의 트랜잭션 내**에서 로직을 실행한다. (동일한 연결 안에서 실행)
- 예외가 발생하면 롤백이 되고, 호출한 곳에도 롤백이 전파된다.
<br><br>

### 2. Propagation.REQUIRES_NEW
- 매번 새로운 트랜잭션을 시작한다. = 새로운 연결(DB 커넥션)을 생성하고 실행한다.
- 부모 트랜잭션은 상관하지 않는다.
- 호출한 곳에 이미 트랜잭션이 설정되어 있다면(기존의 연결이 존재한다면), 기존의 트랜잭션은 메소드가 종료될 때까지 잠시 **대기 상태**로 두고, 자신의 트랜잭션을 실행한다.
- 각각의 트랜잭션에 예외가 발생하여 롤백이 되더라도, 서로 영향을 주지 않는다. (롤백이 전파되지 않는다.)
- 즉, 2개의 트랜잭션은 별개의 단위로서 독립적으로 작동한다.
<br><br>

### 3. Propagation.NESTED
- 부모 트랜잭션이 존재한다면 중첩 트랜잭션을 생성하고, 부모 트랜잭션이 없다면 새로운 트랜잭션을 생성한다.
- 메소드가 부모 트랜잭션 안에서 진행될 때, 별개로 커밋되거나 롤백될 수 있다.
  - 중첩 트랜잭션에서 롤백이 발생하면, 해당 트랜잭션의 시작 지점까지만 롤백된다.
  - 중첩 트랜잭션은 부모 트랜잭션이 커밋될 때 같이 커밋된다.
- `Propagation.REQUIRED`와 비슷하지만, **SAVEPOINT를 지정한 시점까지 부분 롤백이 가능하다**는 차이점이 있다.
- 데이터베이스가 SAVEPOINT 기능을 지원해야한다.
<br><br>

### 4. Propagation.MANDATORY
- 부모 트랜잭션 내에서 실행된다.
- 부모 트랜잭션이 존재한다면 부모 트랜잭션 안으로 합류하고, 부모 트랜잭션이 없다면 Exception을 발생시킨다.
<br><br>

### 5. Propagation.SUPPORT
- 부모 트랜잭션이 존재한다면 부모 트랜잭션으로 합류하여 동작하고, 부모 트랜잭션이 없다면 트랜잭션을 생성하지 않는다.
- non-transactinal하게 실행된다.
<br><br>

### 6. Propagation.NOT_SUPPORT
- 부모 트랜잭션이 존재하면 부모 트랜잭션을 일시정지하고 기능을 수행한다.
- 부모 트랜잭션이 없다면 트랜잭션을 생성하지 않는다.
- non-transactinal하게 실행된다.
<br><br>

### 7. Propagation.NEVER
- 트랜잭션을 허용하지 않는다.
- 진행중인 부모 트랜잭션이 존재하면 Exception을 발생시킨다.
- 부모 트랜잭션이 없다면 트랜잭션을 생성하지 않고 기능을 진행한다.
- non-transactinal하게 실행된다.

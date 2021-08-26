# Exception

Java에는 <code>Checked Exception</code>과 <code>Unchecked Exception</code>이 존재한다.

![image](https://user-images.githubusercontent.com/30489264/130772149-4bb9df32-1dd0-442e-8a6a-d42bb439c2d5.png)


## Checked Exception

<code>RuntimeException</code>을 상속하지 않는 클래스. 

- Exception 처리 코드 여부를 컴파일러가 Check
  - 프로그램 실행 흐름 상 에러 발생 가능이 있을 때 try-catch / throws를 이용해 예외 처리
- (Spring) Transaction의 기본 Rollback 대상이 아니기 때문에 트랜잭션 관리를 위한 추가적인 처리를 해주어야 한다

<br>

## Unchecked Exception

<code>RuntimeException</code>을 상속하는 클래스.

  

###  언제 발생할까?

- 프로그래머의 실수로 발생하는 예외 상황(0으로 나누기, NPE, OutOfIndex 등)
- 컴파일 시점에는 에러가 발생하지 않지만 실행 시험에서 에러가 발생할 수 있음 ❗❗

<br>

## Checked 🆚 Unchecked Exception

||Checked|Unchecked|
|-|-|-|
|확인 시점|Compile 시점|Runtime 시점|
|처리 여부|반드시 예외 처리해야 한다|명시적으로 처리하지 않아도 된다|
|트랜잭션 처리|예외 발생 시 Rollback 하지 않는다|예외 발생 시 Rollback 해야 한다|
|종류|Exception class를 상속받는 하위 클래스 중 RuntimeException을 제외한 모든 예외<br>ex) IOException, ClassNotFoundException 등|RuntimeException의 하위 예외들<br>NPT(NullPointerException), ClassCastException 등|

가장 주요한 차이점은 **예외 처리를 꼭 해주어야 하느냐**라고 할 수 있다.  

<br>

## Exception Handling

예외를 처리하는 방법에는 예외 복구, 예외 처리 회피, 예외 전환 방법이 존재한다.

<br>

### 예외 복구

예외가 발생해도 애플리케이션의 실행은 정상적으로 진행된다(일정 시간 대기, 재시도 회수 제한 등).

``` java
final int MAX_RETRY = 100;
public Object someMethod() {
    int maxRetry = MAX_RETRY;
    while(maxRetry > 0) {
        try {
            ...
        } catch(SomeException e) {
            // 로그 출력. 정해진 시간만큼 대기
        } finally {
            // 리소스 반납 및 정리 작업
        }
    }
    // 최대 재시도 횟수를 넘기면 직접 예외를 발생시킨다.
    throw new RetryFailedException();
}
```

<br>

### 예외 처리 회피

예외가 발생하면 호출한 메서드 쪽으로 예외를 던지는 기법  
호출한 쪽에서는 반드시 예외를 받아 처리해야 한다

``` java
public void add() throws SQLException {
    // ...
}
```

<br>

### 예외 전환

예외가 발생하는 메서드에서 Exception을 Catch해서 다른 예외를 발생시키는 방법  
호출한 메서드 측에서 예외를 받아 처리할 때 더욱 명확하게 인지할 수 있도록 돕기 위한 방법  

``` java
public void add(User user) throws DuplicateUserIdException, SQLException {
    try {
        // ...
    } catch(SQLException e) {
        if(e.getErrorCode() == MysqlErrorNumbers.ER_DUP_ENTRY) {
          //Error Code가 MySQL의 Duplicate Entry면 예외를 전환
            throw DuplicateUserIdException();
        }
        else throw e;
    }
}

public void someMethod() {
    try {
        // ...
    }
    catch(NamingException ne) {
        throw new EJBException(ne);
        }
    catch(SQLException se) {
        throw new EJBException(se);
        }
    catch(RemoteException re) {
        throw new EJBException(re);
        }
}
```

세 가지 방법을 사용할 때, 예외를 잡고 아무 처리도 하지 않는 것은 굉장히 위험하다.  
따라서 예외가 발생했을 때의 catch문이나, throws 처리된 메서드를 호출하는 측에서 반드시 예외 및 에러에 대해 처리해주어야 한다.

<br>

## ➕ Error 🆚 Exception

### Error

- 시스템에 의도하지 않은 비정상적인 상황이 생겼을 때 발생
- Error의 서브 클래스를 throws 선언할 필요는 없다
- Error 및 서브 클래스는 unchecked exception으로 분류된다

### Exception

- Exception 및 서브 클래스는 응용 프로그램이 catch할 수 있는 조건을 나타내는 throwable 형식
- Exception, RuntimeException 및 서브 클래스는 checked exception으로 분류된다

<br>

## Spring Exception Handling

> 참고 자료
> - https://bcp0109.tistory.com/303
> - https://velog.io/@dion/%EB%8F%84%EB%8C%80%EC%B2%B4-Checked-Exception%EC%9D%B4%EB%9E%91-Unchecked-Exception%EC%9D%80-%EC%96%B8%EC%A0%9C%EC%93%B0%EB%8A%94%EA%B1%B0%EC%95%BC


<br>

## 참고 자료

> - https://madplay.github.io/post/java-checked-unchecked-exceptions
> - https://steady-hello.tistory.com/55
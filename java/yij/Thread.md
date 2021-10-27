# Thread

## 프로세스와 쓰레드

**프로세스는 실행 중인 프로그램의 단위**를 말한다. 현재 OS의 특성 상 동영상을 보면서 동시에 문서 작업을 하는 것과 같은 다중 동시 작업을 지원해야 할 때 여러 프로세스가 메모리에 동시에 적재되고 번갈아가며 실행된다. **프로세스 또한 여러 작업 단위를 동시에 실행**해야 할 수 있다. **프로세스는 그래서 한 개 이상의 쓰레드**로 이루어져 있다. 각각의 쓰레드는 고유한 `stack` 영역을 가지며, `code, data, heap` 영역을 공유한다.

<br>

## Java에서의 Thread

JVM은 프로그램의 실행 동안 여러 개의 쓰레드를 동시에 실행할 수 있다.

각각의 Thread는 **priority**를 가지며, 이는 실행의 우선순위를 말한다. 모든 쓰레드는 데몬 쓰레드(사용자가 직접 제어하지 않고 백그라운드에서 실행. 대개 부모 프로세스를 갖지 않는다)가 될 수 있으며, 부모 프로세스가 데몬일 경우에만 자식 프로세스가 데몬이 된다. 자식 쓰레드의 priority는 부모 쓰레드의 priority를 따른다.
JVM이 시작되면 single non-daemon thread(main 메소드가 있는 클래스)가 실행되며, 아래 상황이 발생하면 쓰레드를 종료한다.

- `Runtime`클래스의 `exit`메소드가 호출되고 종료가 승인될 때
- 1️⃣`run`메소드를 반환하거나  2️⃣`run`메소드가 처리할 수 있는 범위를 벗어난 예외를 throws해서 데몬이 아닌 모든 쓰레드가 종료되었을 때

<br>

Java에서는 아래의 두 가지 방법을 통해 쓰레드를 이용할 수 있다.

<br>

### extends Thread

: `Thread` Class를 상속받는 SubClass를 정의한다. 해당 클래스는 `run` 메서드를 Override해야한다.

``` java
     class PrimeThread extends Thread {
         long minPrime;
         PrimeThread(long minPrime) {
             this.minPrime = minPrime;
         }

         public void run() {
             // compute primes larger than minPrime
              . . .
         }
     }
```

아래의 코드를 이용해 쓰레드를 생성하고 시작할 수 있다.

``` java
     PrimeThread p = new PrimeThread(143);
     p.start();
```

<br>

### implements Runnable

: `Runnable` Interface를 구현한다. 해당 클래스는 반드시 `run` 메서드를 구현해야 한다. 참고로 `Thread` Class는 `Runnable` Interface를 implements한다.

``` java
     class PrimeRun implements Runnable {
         long minPrime;
         PrimeRun(long minPrime) {
             this.minPrime = minPrime;
         }

         public void run() {
             // compute primes larger than minPrime
              . . .
         }
     }
```

`Thread` 클래스를 생성하면서 인자를 넘겨주는 방식으로 쓰레드를 실행할 수 있다.

``` java
     PrimeRun p = new PrimeRun(143);
     new Thread(p).start();
```

<br>

## Thread-Safe

Java 환경에서는 개발자가 쉽게 다중의 Thread를 생성하고 사용할 수 있다. 여러 쓰레드의 바이트 코드를 동시에 실행시키는 멀티 쓰레딩은 애플리케이션 성능을 향상시킬 수 있지만, Resource를 공유하기 때문에 안전한 접근 방식이 필요하다.
즉, **Multiple Thread를 사용할 때 에는 Thread-Safe하게 사용해야 한다**.

대부분의 경우 멀티 쓰레드 애플리케이션의 오류는 여러 쓰레드 간에 자원을 잘못 공유했을 때 발생한다.

### Stateless Implementations

```java
public class MathUtils {
    
    public static BigInteger factorial(int number) {
        BigInteger f = new BigInteger("1");
        for (int i = 2; i <= number; i++) {
            f = f.multiply(BigInteger.valueOf(i));
        }
        return f;
    }
}
```

`factorial()` 메서드는 상태를 저장하지 않고, 입력 값이 같을 경우 반드시 동일한 값을 반환한다.

이 방법은 메서드의 실행이 외부 상태에 의존하지 않으며, 상태 필드를 변화시키지도 않기 때문에 멀티 쓰레드 환경에서도 안전하게 호출할 수 있다.
즉, 모든 쓰레드가 안전하게 `factorial()` 메서드를 호출할 수 있고, 서로 간섭하지 않으며 원하는 예상 결과를 얻어낼 수 있다.

**stateless하게 구현**하면 쉽게 thread-safe를 달성할 수 있다.

<br>

### Immutable Implementations

서로 다른 쓰레드들 간에 상태를 공유해야 한다면, immutable하게 만듦으로써 thread-safe class를 만들 수 있다.

클래스 인스턴스가 생성되고 나면 내부 상태를 변경할 수 없는 것을 immutable 하다고 한다.

immutable 클래스를 만드는 방법은 ***모든 필드를 private, final로 만들고 setter를 제거***하면 된다.

```java
public class MessageService {
    
    private final String message;

    public MessageService(String message) {
        this.message = message;
    }
    
    // standard getter
    
}
```

`MessageService` 클래스는 (하나밖에 없지만) 필드를 모두 private final로 등록하고 setter를 제거했다. 따라서 클래스 인스턴스가 생성될 때 이외에는 필드의 값을 쓸 수 없다.

'클래스가 immutable 하다 👉 thread-safe 하다'면 이도 성립할까?
즉, '클래스가 mutable 하다 👉 thread-safe 하지 않다'일까?

immutable하게 구현하지 않아도 여러 쓰레드가 MessageService 클래스에 대해 read-only 액세스 권한만 있어도 thread-safe 하다고 할 수 있다.

불변성만이 thread-safety를 달성하는 유일한 방식은 아니라는 뜻이다.

<br>

### Thread-Local Fields

OOP에서는 객체가 필드를 이용해서 상태를 표현하고 메서드를 이용해서 행위를 나타내야 한다.

상태를 유지해야 할 때 필드를 쓰레드 로컬화해서 쓰레드 간 상태를 공유하지 않게 하면 thread-safe 클래스를 만들 수 있다.

Thread 클래스를 상속받은 클래스에서 ***필드를 `private`로 선언하면 해당 필드를 thread-local로 만들 수 있다***.

```java
public class ThreadA extends Thread {
    
    private final List<Integer> numbers = Arrays.asList(1, 2, 3, 4, 5, 6);
    
    @Override
    public void run() {
        numbers.forEach(System.out::println);
    }
}
```

이 예시에서 클래스 인스턴스를 생성하면 해당 인스턴스만의 고유한 상태를 가질 수 있고, private 제어자를 사용하기 때문에 다른 쓰레드와 공유하지 않는다. 따라서 이 클래스는 thread-safe하다.

```java
public class StateHolder {
    
    private final String state;

    // standard constructors / getter
}

public class ThreadState {
    
    public static final ThreadLocal<StateHolder> statePerThread = new ThreadLocal<StateHolder>() {
        
        @Override
        protected StateHolder initialValue() {
            return new StateHolder("active");  
        }
    };

    public static StateHolder getState() {
        return statePerThread.get();
    }
}
```

private 키워드를 사용하지 않아도 `ThreadLocal` 인스턴스를 할당해서 쓰레드 로컬 필드를 만들 수 있다.

이 방법에서 각각의 쓰레드들은 constructor/getter를 통해 쓰레드 로컬 필드에 접근한다. 그리고 고유한 상태를 가질 수 있게 필드를 초기화해 복사한다.

<br>

### Synchronized Collections

Collection 프레임워크에 포함된 Synchronized Wrapper Set을 통해 Thread-safe한 Collection을 만들 수 있다.

```java
Collection<Integer> syncCollection = Collections.synchronizedCollection(new ArrayList<>());
Thread thread1 = new Thread(() -> syncCollection.addAll(Arrays.asList(1, 2, 3, 4, 5, 6)));
Thread thread2 = new Thread(() -> syncCollection.addAll(Arrays.asList(7, 8, 9, 10, 11, 12)));
thread1.start();
thread2.start();
```

synchronized된 collection은 동기화를 위해 locking을 사용한다.

즉, 한 번에 한 쓰레드만 메서드에 접근할 수 있고, 나머지 쓰레드는 첫 쓰레드의 lock이 풀릴 때 까지 blocked된다.

따라서 synchronize 방식은 수행을 직렬화 할 수 있어서 성능 저하의 원인이 된다.

<br>

### Concurrent Collections

위의 synchronized collection를 사용하는 대신 concurrent collection을 사용해도 thread-safe collection을 만들 수 있다.

Java는 `ConcurrentHashMap`처럼 여러 concurrent collection을 포함하는 `java.util.concurrent` 패키지를 제공한다.

```java
Map<String,String> concurrentMap = new ConcurrentHashMap<>();
concurrentMap.put("1", "one");
concurrentMap.put("2", "two");
concurrentMap.put("3", "three");
```

동기화 방식과 달리 동시 방식은 **데이터를 세그먼트로 분할**하여 thread-safe를 구현한다.

ConcurrentHashMap에서는 여러 쓰레드가 서로 다른 맵 세그먼트에서 lock을 획득할 수 있기 때문에 동시에 Map에 액세스 하는 것이 가능하다.

synchronized 방식과 concurrent 방식 모두 ***Collection을 Thread-safe하게 만드는 것이고, 내부 Elements를 Thread-Safe하게 만드는 것은 아니다*** ❗❗

<br>

<!-- ### Atomic Objects

`AtomicInteger`, `AtomicLong`, `AtomicReference` 같은 Atomic Class들을 이용해서 thread-safe를 구현할 수 있다.

동기화를 사용하지 않고도 쓰레드에 안전한 원자 단위 연산을 수행할 수 있다.

```java
public class Counter {
    
    private int counter = 0;
    
    public void incrementCounter() {
        counter += 1;
    }
    
    public int getCounter() {
        return counter;
    }
}
``` -->

### Synchronized Methods

앞에서 소개한 접근 구현 방식들은 collection, data type에 적용할 수 있는 방법이다. 그보다 더 큰 범주에서 제어하기 위해 synchronized method를 만들어서 thread-safe를 구현할 수 있다.

synchronized method는 **한 번에 하나의 쓰레드만 동기화된 메서드에 접근** 할 수 있도록 허용하고, 다른 쓰레드는 해당 메서드의 접근 권한이 없다.

다른 쓰레드는 첫 번째 쓰레드의 수행이 완료되었거나 메서드가 예외를 throw할 때 까지 접근이 차단된 상태를 유지한다.

<br>

```java
public synchronized void incrementCounter() {
    counter += 1;
}
```

`synchronized` 키워드를 붙이면 동기화 메서드를 만들 수 있다. 

이 예시에서는 한 번에 하나의 쓰레드만이 `incrementCounter` 메서드에 접근할 수 있으므로 중복 실행은 발생하지 않는다.

쓰레드가 동기화 메서드를 호출하면 lock을 얻으며, 실행을 완료하면 반납한다. 다른 쓰레드는 다시 lock을 얻어서 액세스 권한을 얻을 수 있다.

<br>

### Synchronized Statements

메서드 전체를 synchronized 하는 것은 수행을 불필요하게 직렬화할 수 있으므로 일부만을 thread-safe하게 구현할 수 있다.

<br>

```java
public void incrementCounter() {
    // additional unsynced operations
    synchronized(this) {
        counter += 1; 
    }
}
```

synchronized statements를 이용해서 `incrementCounter` 메서드를 구현하면 내부에서 동기화가 필요한 부분과 필요하지 않은 부분을 분리할 수 있다.

단, `this`를 이용해서 lock을 제공하는 객체를 지정해야 한다.

<br>

### Read/Write Locks

Thread-safe를 구현하기 위한 강력한 방법 중 하나는 `ReadWriteLock`을 사용하는 것 이다.

읽기 전용 작업과 쓰기 전용 작업에 대한 Lock pair를 제공해서, 상황에 따른 접근 권한을 분리할 수 있다.

리소스에 쓰는 쓰레드가 없으면 여러 쓰레드가 리소스의 읽기 권한을 가질 수 있고, 있으면 다른 쓰레드는 읽기 권한을 가질 수 없다.

<br>

```java
public class ReentrantReadWriteLockCounter {
    
    private int counter;
    private final ReentrantReadWriteLock rwLock = new ReentrantReadWriteLock();
    private final Lock readLock = rwLock.readLock();
    private final Lock writeLock = rwLock.writeLock();
    
    public void incrementCounter() {
        writeLock.lock();
        try {
            counter += 1;
        } finally {
            writeLock.unlock();
        }
    }
    
    public int getCounter() {
        readLock.lock();
        try {
            return counter;
        } finally {
            readLock.unlock();
        }
    }

   // standard constructors
   
}
```

<br>

## 정리

> Multi-Thread 환경에서는 자원의 동시 접근으로 인한 문제가 발생할 수 있다.  
> Thread-safe를 적용하기 위한 대상은 Field(Collection, Object), Method, Statements가 있다.  
> `synchronized` keyword를 사용하는 것은 강력한 thread-safe를 지원하지만 수행을 직렬화하기 때문에 동시성의 장점이 떨어진다.
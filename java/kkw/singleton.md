## 싱글톤 패턴(Singleton Pattern)
- 애플리케이션이 시작될 때, 어떤 클래스가 최초 한 번만 메모리를 할당하고 해당 메모리에 인스턴스를 만들어 사용하는 패턴
- JAVA의 싱글톤 패턴은 private한 생성자와 static method를 사용한다는 특징이 있다.

### 사용하는 이유
- 객체를 생성할 때마다 메모리 영역을 할당 받고, new를 통해 매번 새로운 객체를 생성하는 것으로 메모리 낭비를 방지할 수 있다.
- 싱글톤으로 구현된 인스턴스는 static 하므로, 공유가 가능하다는 장점이 있다.
- 인스턴스가 절대적으로 한 개만 존재하는 것을 보장해줄 수 있음

### 단점
- 싱글톤 인스턴스가 너무 많은 작업을 하게 되면, 결합도가 높아진다는 단점이 있다
-> 결합도가 높아지면 유지보수가 힘들고, 테스트 진행이 어려울 수 있음
- 멀티 스레드 환경에서 동기화 처리를 해주어야함


### 구현
- Eager Initialization(이른 초기화, Thread-Safe)
  ```Java
  public class Singleton{
    private static Singleton instance = new Singleton();
    private Singleton(){}
    public static Singleton getInstance(){
      return instance;
    }
  }
  ```
  - 클래스 로더에 의해 클래스가 초기화 되는 시점에서 Static binding을 통해 인스턴스를 메모리에 등록해서 사용하는 방식
  - 클래스 로더에 의해 클래스가 최초로 로딩될 때 객체가 생성됨


- Lazy Initialization with synchronized
  ```Java
  public class Singleton {
    private static Singleton instance;

    private Singleton() {}

    public static synchronzied Singleton getInstance() {
      if(instance == null) {
         instance = new Singleton();
      }
        return uniqueInstance;
    }
  }
  ```
  - 인스턴스가 필요한 시점에 요청하여 Dynamic Binding을 통해 인스턴스를 생성하는 방식 
  - 인스턴스가 생성여부와 상관 없이 동기화 블록을 거치기 때문에 성능이 떨어짐

- Lazy Initialization (Double Checking Locking, DCL)
  ```JAVA
  public class Singleton {
    private volatile static Singleton instance;

    private Sigleton() {}

    public Singleton getInstance() {
      if(instance == null) {
        synchronized(Singleton.class) {
            if(instance == null) {
              instance = new Singleton(); 
            }
        }
      }
      return instance;
    }
  }
  ```
  - volatile 키워드를 사용하는 방식
  - volatile 사용하는 이유
    - 멀티 스레드 환경에서 작업을 수행하는 동안 성능 향상을 위해 Main Memory에서 읽은 변수 값을 CPU Cache에 저장하게 됨
    - 멀티 스레드 환경에서는 각각의 스레드가 변수 값을 읽어올 때, 각각 CPU Cache에 저장된 값이 다르기 때문에 변수값 불일치 문제가 발생할 수 있음
    - volatile 키워드를 사용하여 변수가 Main Memory에 있는 것이 보장되므로 위의 문제를 해결 할 수 있음

  Lazy Initialization (Lazy Holder)
  ```Java
  public class Singleton {
    private Singleton() {}

    private static class LazyHolder() {
        private static final Singleton instance = new Singleton();
    }

    public static Singleton getInstance() {
        return LazyHolder.instance;
    }    
  }
  ```
  - static 영역에서 초기화를 해주지만, 객체가 필요한 시점까지 초기화를 미루는 방식
  - static 멤버 클래스라도, 내부 클래스에 변수가 없기 때문에 클래스 로더가 초기화할 때 초기화 하지 않고 메소드를 호출할 때 Dynamic binding 하게 됨
  - 위의 방법들보다 성능이 뛰어남
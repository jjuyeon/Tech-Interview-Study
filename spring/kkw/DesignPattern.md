### 점층적 생성자 패턴(Telescoping Constructor Pattern)

- 클래스 내에 오버로딩을 통해서 생서자를 여러 개 작성하는 방법

```Java
User user = new User("uid12345", "kim", 20, Male, "01012345678");
User user = new User("uid12345", "kim" ,"male");
```

- 특정 파라미터가 어떠한 값을 나타내는지 확인하기가 어려움
- 사용자가 설정하길 원치 않는 매개변수까지 설정을 해주어야 함
- 매개변수 조합에 따라 생성자의 수가 쓸데없이 많이 늘어날 수 있음
- 매개변수가 늘어날 수록 코드 작성 및 가독성이 저하됨

### 자바빈즈 패턴(JavaBeans Pattern)

- 매개변수가 없는 생성자로 객체를 만든 후 Setter 메소드들을 호출해 원하는 매개변수 값을 설정하는 방식

```Java
User user = new User();

user.setUid("uid12345");
user.setName("kim");
user.setPhoneNumber("01012345678");
```

- 가독성이 좋으며 인스턴스를 만들기 쉬움
- 객체 하나를 만들기 위해 메소드를 여러 개 호출 하여야 함
- 객체가 완전히 생성되기 전까지는 일관성(Consistency)가 무너진 상태에 놓이게 된다.
- 클래스를 불변(immutable) 상태로 만들 수 없음
  -> frezz 방식으로 완성된 객체를 immutable 상태로 만들 수는 있음

### 빌더 패턴(Builder Pattern)

- 생성과 표현의 분리를 위해 디자인된 패턴
- 클라이언트 코드에서 필요한 객체를 직접 생성하는 대신, 그 전에 필수 인자들을 전달하여 빌더 객체를 만들 뒤, 빌더 객체에 정의된 설정 메소드들을 호출하여 인스턴스를 생성하는 방법

```Java
  User user = new User.userBuilder("uid12345")
    .name("kim")
    .phoneNumber("01012345678")
    .build();
```

- 인스턴스를 생성하는 필수 인자와 선택 인자를 구분할 수 있다
- 가독성이 좋고, 일관성을 유지할 수 있다.
- Effective Java의 빌더 패턴
  - 객체를 만들 때 변수를 각각 개별 함수로 세팅하고, 변수 설정이 끝나면 객체를 만들어 반환하는 방법
  - 매개변수가 많을 때 사용하면 좋음
  - 만들어진 객체는 멤버변수를 수정할 수 없다.
  - lombok의 @Builder 어노테이션을 이용해서도 구현 가능
  ```Java
  @Builder
  public class User {
    private final String uid;
    private final String name;
    private final String phoneNumber;
  }
  ```
- GoF 디자인 빌더 패턴
  ![problem1](https://user-images.githubusercontent.com/43779730/134389675-cc3aee43-2378-4f33-a494-189470406bed.png)

  - 복잡한 객체를 생성하는 방법과 표현하는 방법을 정의하는 클래스를 별도로 분리하여, 서로 다른 표현이라도 이를 생성할 수 있는 동일한 절차를 제공할 수 있도록 하는 것
  - 각기 다른 기능을 가진 객체를 생성하는데 있어서 통일된 방식을 제공한다는 것
  - `Builder` : 객체의 일부 요소들을 생성하기 위한 추상 인터페이스를 제공
  - `ConcreteBuilder` : Builder 클래스에 정의된 인터페이스를 구현하며, 제품의 부품들을 모아 빌더를 복합한다. 생성한 요소의 표현을 정의하고 관리함. 제품을 검색하는데 필요한 인터페이스도 제공함
  - `Director` : Builder 인터페이스를 사용하는 객체를 합성함
  - `Product` : 생성할 복합 객체를 표현함
  - 과정
    - 사용자가 `Director` 객체를 생성하고, 생성한 객체를 자신이 원하는 `Builder` 객체로 합성해 나감
    - 제품의 일부가 구축될 때마다 `Director`는 `Builder`에 통보한다
    - `Builder`는 `Director`의 요청을 처리하며 제품에 부품을 추가한다
    - 사용자는 `Builder`에서 제품을 검색한다

### 참고 사이트

- https://projectlombok.org/features/Builder
- https://johngrib.github.io/wiki/design-pattern/builder-pattern/
- https://refactoring.guru/design-patterns/builder

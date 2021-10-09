# POJO(Plain Old Java Object)

특정 프레임워크에 종속되지 않은 순수한 자바 객체

속성, 메서드에 대한 어떠한 네이밍 규칙도 없다.

Java EE와 같은 무거운 프레임워크들이 서비스 시장을 점유했을 때, 해당 프레임워크에 종속된 (무거운) 객체를 사용해야 했던 것에 반발해서 나오게 된 개념이다.

Spring Framework는 POJO를 기본 개념으로 채택했다.

```java
public class EmployeePojo {

    public String firstName;
    public String lastName;
    private LocalDate startDate;

    public EmployeePojo(String firstName, String lastName, LocalDate startDate) {
        this.firstName = firstName;
        this.lastName = lastName;
        this.startDate = startDate;
    }

    public String name() {
        return this.firstName + " " + this.lastName;
    }

    public LocalDate getStart() {
        return this.startDate;
    }
}
```

이 클래스는 프레임워크에 종속되지 않으므로 모든 Java 프로그램에서 쉽게 사용할 수 있다.

하지만 클래스의 상태를 구성하고, 접근하고, 수정하는 어떠한 컨벤션도 없기 때문에 다음의 문제를 발생시킬 수 있다.

1. 사용방법을 이해하려는 개발자의 러닝커브 상승
2. 프레임워크의 활용성을 저해할 수 있음

<br>

## POJO의 단점

예시로 `commons-beanutils` 혹은 `Jackson`과 같은 의존성을 사용하여 인스턴스의 property에 접근한다고 했을 때, 위의 코드에서는 `{start}`라는 property밖에 찾을 수 없다.

이상적으로는 `{firstName, lastName, startDate}`를 가져와야 한다. 이를 위해 POJO의 기본적 성질을 해치지 않으면서, 많은 Java 라이브러리에서 사용할 수 있는 `JavaBeans` 컨벤션이 존재한다.

<br>

## JavaBeans

JavaBean이란 구현 방법에 대해 다음의 컨벤션을 적용한 POJO이다.

- Access Level
  - Access Modifier는 private
  - getter/setter를 노출
- Method Names: getter/setter는 `getX`, `setX` 규칙을 따름(boolean의 경우 `isX`도 가능)
- Default Constructor: 기본 생성자(인수가 없는)가 존재해야 함(역직렬화 시에 인수 없이 인스턴스 생성할 수 있도록)
- Serializable: Serializable 인터페이스를 implements하여 상태를 저장할 수 있게 함

<br>

### JavaBean Example

```java
public class EmployeeBean implements Serializable {

    private static final long serialVersionUID = -3760445487636086034L;
    private String firstName;
    private String lastName;
    private LocalDate startDate;

    public EmployeeBean() {
    }

    public EmployeeBean(String firstName, String lastName, LocalDate startDate) {
        this.firstName = firstName;
        this.lastName = lastName;
        this.startDate = startDate;
    }

    public String getFirstName() {
        return firstName;
    }

    public void setFirstName(String firstName) {
        this.firstName = firstName;
    }

    //  additional getters/setters

}
```

리플렉션으로 빈을 검사하면 모든 속성을 얻을 수 있다.

<br>

## JavaBeans의 단점

- Mutability: `setter` 메서드로 인해 mutable하다. mutable 속성은 동시성 혹은 일관성 문제로 이어질 수 있다.
- Boilerplate: 모든 속성에 대해 getter를 적용하고, 대부분의 속성에는 setter를 적용해야 한다. 이는 필수적인 상황이 아니면 권장하지 않는다.
- Zero-argument Constructor: 객체가 필수 의존성에 대해 nullable 속성을 가지지 않도록 인수가 없는 생성자의 설정은 권장하지 않는다. 하지만 JavaBean 표준은 직렬화를 위해 이를 요구한다.

<br>

## 결론

- POJO: 특정 프레임워크에 바인딩되지 않은 Java 객체
- JavaBean: 엄격한 컨벤션을 지키는 특수 유형의 POJO

JavaBean 인스턴스를 활용했을 때 일부 프레임워크와 라이브러리를 적용하기에 좋다. 단점에 대해서는 이를 보완할 수 있는 방법을 추가로 적용해야 할 것이다.
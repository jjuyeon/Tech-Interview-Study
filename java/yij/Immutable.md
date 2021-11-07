# Immutable Object

Spring의 Constructor Dependency Injection, Java Collection의 String, Multi-Thread에서 Thread-Safe를 구현하는 방법, Builder Pattern의 장점 등등에서 공통적으로 언급하는 단어는 바로 **Immutable**이다.

Java와 Java 기반 프레임워크의 많은 원칙들은 객체를 Immutable하게 만드는 것을 중요한 목표로 삼는다.

그럼 객체가 어떨 때 Immutable하다고 하는 것인지, 그것의 장점은 무엇인지를 알아야 자바에 대해 잘 이해하고 있는게 아닐까 🙄 라는 생각에 또 조사해봤다.

<br>

## Immutable Object란?

**객체가 처음 생성된 이후, 상태가 변하지 않고 일정하게 유지되는 것**을 Immutable Object(이후 불변 객체)라고 한다.

실제 개발 시, 객체가 Immutable하게 정의되면 API를 사용했을 때 해당 인스턴스의 라이프 사이클 동안 완전히 같은 방식으로 작동할 것을 보장한다.

`java.lang.String`은 메서드를 통해 객체의 값을 변경할 수 있는 것 처럼 보여도, 원본 문자열 자체는 변하지 않는다(자세한 내용은 [링크](https://www.javatpoint.com/immutable-string) 참조).

<br>

```java
String name = "baeldung";
String newName = name.replace("dung", "----");

assertEquals("baeldung", name);
assertEquals("bael----", newName);
```

<br>

이를 위해 객체는 Read-Only 메서드를 제공하며 객체의 내부 상태를 변경하는 메서드를 포함해서는 안된다.

<br>

## *final* Keyword in Java

자바에서 불변성을 언급할 때에는 `final` keyword가 항상 등장한다.

변수는 기본적으로 mutable하기 때문에 값을 자유롭게 변경할 수 있다. 단, `final` 키워드를 함께 사용해서 선언할 때는 변경을 시도했을 때 자바 컴파일러가 컴파일 오류를 보고한다.

```java
final String name = "baeldung";
name = "bael...";   // compile error

final List<String> strings = new ArrayList<>();
assertEquals(0, strings.size());
strings.add("baeldung");
assertEquals(0, strings.size());    // fail
```

하지만 `final`은 정확하게는 **변수가 가지고 있는 레퍼런스를 변경하는 것을 금지**하기 때문에, 객체의 내부 상태를 변경하는 것은 보호할 수 없다.

그래서 위의 예제에서 `strings` 리스트에는 새 element가 들어갈 수 있고, size는 1로 증가해서 두 번째 `assertEquals()` 메서드는 실패한다. 따라서 `strings`는 Immutable 객체가 아니다.

<br>

### List를 Immutable Object로 만들기

```java
List<>
```

<br>

## Immutability in Java

변수의 value를 변경하는 것에 주의해야 한다는 점을 알면 불변 객체의 API를 구현할 수 있다.

이것은 **API의 사용에 관계없이 내부 상태가 절대 변하지 않음**을 보장해야 함을 의미한다.

많은 경우에서 이는 필드에 `final`을 사용해서 구현한다.

```java
class Money {
    private final double amount;
    private final Currency currency;

    // ...
}
```

(모든 Primitive Data Type의 경우를 포함) amount의 값은 변하지 않는다. 단, `currency` 인스턴스가 변하지 않음을 보장하지만, Currency 클래스의 API에 의존해서 불변을 보장하는지 함께 검증해야 한다.

```java
class Money {
    // ...
    public Money(double amount, Currency currency) {
        this.amount = amount;
        this.currency = currency;
    }

    public Currency getCurrency() {
        return currency;
    }

    public double getAmount() {
        return amount;
    }
}
```

불변 API의 요구사항을 만족하기 위해 `Money` 클래스는 Read-Only 메서드만 가진다. reflection API(모름)를 이용하면 불변성을 깰 수 있는데, 이것은 불변 객체의 성질을 위반하기 때문에 조심해야 한다.

<br>

## Benefits

불변 객체가 가지는 가장 큰 장점은 Multi-Thread 환경에서의 안정성을 보장하는 것 이라고 생각한다. Multi-Thread 환경에서 여러 쓰레드가 공유 자원에 접근할 때, 해당 필드가 변할 가능성이 있다면 일관성이 깨질 수 있다. 필드를 불변하게 만들어주는 것은 Thread-safe를 구현하는 가장 쉬운 방법 중 하나이다.

객체지향 관점에서 봤을 때는 외부에서 객체의 상태를 변경할 수 있는 권한을 축소하기 때문에 더 자율적인 객체를 만들 수 있다.

<br>

## 참고자료

> https://www.baeldung.com/java-immutable-object
> https://tecoble.techcourse.co.kr/post/2020-05-18-immutable-object/

<br>

## 정리

> Immutable이 무엇이며 왜 중요한가?
> Java에서 Immutable이란 객체가 처음 생성된 이후 상태가 변하지 않음을 의미한다. 이는 여러 쓰레드가 불변 객체에 동시에 접근을 해도 값이 변하지 않기 때문에 Thread-Safe를 구현하는 유용한 방법이 될 수 있다.

<br>
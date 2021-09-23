> 💡 Q) 빌더 패턴에 대해 설명하고, 장점을 말해주세요.
> [점층적 생성자 패턴](#telescoping-constructor-pattern), [자바빈 패턴](#javabeans-pattern), [빌더 패턴](#builder-pattern)
> A) 빌더 클래스를 구현하여 빌더 메서드를 이용하여 객체를 생성하는 디자인 패턴입니다. 점층적 생성자 패턴과 비교했을 때 각각의 빌더 메서드가 어떤 필드에 매치되는지 명확히 알 수 있고, 자바빈 패턴과 비교했을 때 setter 메서드의 생성을 막고 한 번에 객체를 생성해서 객체의 불변성을 지킬 수 있습니다.

<br>

## Telescoping Constructor Pattern

```java
Pizza hawaiian = new Pizza("cross");
Pizza hawaiian = new Pizza("cross", "mild");
Pizza hawaiian = new Pizza("cross", "mild", "ham+pineapple");
// Dough, Sauce, Topping
```

필수 인자를 받는 생성자와 선택적 인자를 받는 생성자들을 객체가 생성될 수 있는 케이스 숫자대로 만들어 객체를 생성하는 패턴

구현이 간단하지만 인자가 추가될 때 마다 생성자를 계속 수정해야 하는 번거로움이 있으며 코드 가독성이 떨어진다.

위의 코드를 봐도 각각의 인자들이 어떤 필드에 대응되는지 알기 어렵다.

<br>

## JavaBeans Pattern

```java
Pizza pizza = new Pizza();
pizza.setDough("cross");
pizza.setSauce("mild");
pizza.setTopping("ham+pineapple");
```

setter 메서드를 이용해 객체를 생성하는 메서드의 인자와 필드 간의 연결성을 높이는 패턴

하지만 객체를 생성하는 구문이 필요 이상으로 분리되어 있고, setter 메서드의 호출로 인해 객체가 mutable한 속성을 가지게 되었다.

## Builder Pattern

```java
public class BuilderExample {
  public static void main(String[] args) {
    Cook cook = new Cook();
    PizzaBuilder hawaiianPizzaBuilder = new HawaiianPizzaBuilder();
    PizzaBuilder spicyPizzaBuilder = new SpicyPizzaBuilder();

    cook.setPizzaBuilder(hawaiianPizzaBuilder);
    //cook.setPizzaBuilder(spicyPizzaBuilder);
    cook.constructPizza();

    Pizza pizza = cook.getPizza();
    System.out.println(pizza);
  }
}
```

<br>

```java
public class Pizza {

  private String dough = "";
  private String sauce = "";
  private String topping = "";

  @Override
  public String toString() {
    return "Pizza dough is " + this.dough + 
    ", the sauce is " + this.sauce + 
    ", the topping is " + this.topping;
  }

  public Pizza(PizzaBuilder builder) {
    this.dough = builder.dough;
    this.sauce = builder.sauce;
    this.topping = builder.topping;
  }

}
```

<br>

```java
public abstract class PizzaBuilder {

  protected final String dough;
  protected final String sauce;
  
  protected String topping;

  public Pizza getPizza(){
    return new Pizza(this);
  }

  public PizzaBuilder(String dough, String sauce) {
    this.dough = dough;
    this.sauce = sauce;
  }

  public abstract PizzaBuilder buildTopping();

}
```

<br>

```java
public class HawaiianPizzaBuilder extends PizzaBuilder{

  public HawaiianPizzaBuilder() {
    super("cross", "mild");
  }

  @Override
  public PizzaBuilder buildTopping() {
    this.topping = new String("ham+pineapple");
    return this;
  }
  
}
```

<br>

```java
public class SpicyPizzaBuilder extends PizzaBuilder{

  public SpicyPizzaBuilder() {
    super("pan baked", "hot");
  }

  @Override
  public PizzaBuilder buildTopping() {
    this.topping = new String("pepperoni+salami");
    return this;
  }
  
}
```

<br>

```java
public class Cook {
  private PizzaBuilder pizzaBuilder;

  public void setPizzaBuilder(PizzaBuilder pizzaBuilder) {
    this.pizzaBuilder = pizzaBuilder;
  }

  public Pizza getPizza() {
    return pizzaBuilder.getPizza();
  }

  public void constructPizza() {    
    pizzaBuilder
          .buildTopping();
  }
}
```

<br>

빌더 패턴은 복합적인 객체의 생성 과정과 표현 방법을 분리해서 하나의 생성 절차로 다양한 객체 표현 결과를 만들어내는 패턴이다. 각각의 생성 체인이 어떤 필드에 어떤 값을 저장하는지 명확하게 알 수 있고, setter 메서드가 없으므로 immutable 객체를 만들 수 있다. 한 번에 객체를 생성해서 일관성 있는 객체를 만들 수 있다는 장점 또한 있다.

피자 예시에서는 필수 필드로 dough/sauce를, 선택 필드로 topping을 지정했다. PizzaBuilder의 생성자에서 필수 필드들을 인자로 받아 초기화하고, 선택 필드들은 build____ 메서드를 통해(Cook 클래스의 constructPizza메서드처럼) 빌드할 수 있도록 했다.


 
실제 구현에서는 인자의 개수가 훨씬 많고, 임의의 값을 입력받아 저장하는 경우가 많다. Pizza 예시처럼 필드가 3개밖에 존재하지 않는 경우에는 점층적 생성자 패턴이 더 적합하다(코드의 복잡성). 하지만 인자의 변경 가능성이 있을 때, 매개 변수의 대부분이 선택적 사항일 때 빌더 패턴의 장점인 가독성과 유지보수성이 증가한다.

Java에서는 Lombok의 @Builder 어노테이션을 이용해 Builder패턴을 사용할 수 있다.

<br>

## 참고 자료

> - https://projectlombok.org/features/Builder  
> - https://www.informit.com/articles/article.aspx?p=1216151&seqNum=2  
> - https://johngrib.github.io/wiki/builder-pattern/
> π‘ Q) λΉλ ν¨ν΄μ λν΄ μ€λͺνκ³ , μ₯μ μ λ§ν΄μ£ΌμΈμ.
> [μ μΈ΅μ  μμ±μ ν¨ν΄](#telescoping-constructor-pattern), [μλ°λΉ ν¨ν΄](#javabeans-pattern), [λΉλ ν¨ν΄](#builder-pattern)
> A) λΉλ ν΄λμ€λ₯Ό κ΅¬ννμ¬ λΉλ λ©μλλ₯Ό μ΄μ©νμ¬ κ°μ²΄λ₯Ό μμ±νλ λμμΈ ν¨ν΄μλλ€. μ μΈ΅μ  μμ±μ ν¨ν΄κ³Ό λΉκ΅νμ λ κ°κ°μ λΉλ λ©μλκ° μ΄λ€ νλμ λ§€μΉλλμ§ λͺνν μ μ μκ³ , μλ°λΉ ν¨ν΄κ³Ό λΉκ΅νμ λ setter λ©μλμ μμ±μ λ§κ³  ν λ²μ κ°μ²΄λ₯Ό μμ±ν΄μ κ°μ²΄μ λΆλ³μ±μ μ§ν¬ μ μμ΅λλ€.

<br>

## Telescoping Constructor Pattern

```java
Pizza hawaiian = new Pizza("cross");
Pizza hawaiian = new Pizza("cross", "mild");
Pizza hawaiian = new Pizza("cross", "mild", "ham+pineapple");
// Dough, Sauce, Topping
```

νμ μΈμλ₯Ό λ°λ μμ±μμ μ νμ  μΈμλ₯Ό λ°λ μμ±μλ€μ κ°μ²΄κ° μμ±λ  μ μλ μΌμ΄μ€ μ«μλλ‘ λ§λ€μ΄ κ°μ²΄λ₯Ό μμ±νλ ν¨ν΄

κ΅¬νμ΄ κ°λ¨νμ§λ§ μΈμκ° μΆκ°λ  λ λ§λ€ μμ±μλ₯Ό κ³μ μμ ν΄μΌ νλ λ²κ±°λ‘μμ΄ μμΌλ©° μ½λ κ°λμ±μ΄ λ¨μ΄μ§λ€.

μμ μ½λλ₯Ό λ΄λ κ°κ°μ μΈμλ€μ΄ μ΄λ€ νλμ λμλλμ§ μκΈ° μ΄λ ΅λ€.

<br>

## JavaBeans Pattern

```java
Pizza pizza = new Pizza();
pizza.setDough("cross");
pizza.setSauce("mild");
pizza.setTopping("ham+pineapple");
```

setter λ©μλλ₯Ό μ΄μ©ν΄ κ°μ²΄λ₯Ό μμ±νλ λ©μλμ μΈμμ νλ κ°μ μ°κ²°μ±μ λμ΄λ ν¨ν΄

νμ§λ§ κ°μ²΄λ₯Ό μμ±νλ κ΅¬λ¬Έμ΄ νμ μ΄μμΌλ‘ λΆλ¦¬λμ΄ μκ³ , setter λ©μλμ νΈμΆλ‘ μΈν΄ κ°μ²΄κ° mutableν μμ±μ κ°μ§κ² λμλ€.

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

λΉλ ν¨ν΄μ λ³΅ν©μ μΈ κ°μ²΄μ μμ± κ³Όμ κ³Ό νν λ°©λ²μ λΆλ¦¬ν΄μ νλμ μμ± μ μ°¨λ‘ λ€μν κ°μ²΄ νν κ²°κ³Όλ₯Ό λ§λ€μ΄λ΄λ ν¨ν΄μ΄λ€. κ°κ°μ μμ± μ²΄μΈμ΄ μ΄λ€ νλμ μ΄λ€ κ°μ μ μ₯νλμ§ λͺννκ² μ μ μκ³ , setter λ©μλκ° μμΌλ―λ‘ immutable κ°μ²΄λ₯Ό λ§λ€ μ μλ€. ν λ²μ κ°μ²΄λ₯Ό μμ±ν΄μ μΌκ΄μ± μλ κ°μ²΄λ₯Ό λ§λ€ μ μλ€λ μ₯μ  λν μλ€.

νΌμ μμμμλ νμ νλλ‘ dough/sauceλ₯Ό, μ ν νλλ‘ toppingμ μ§μ νλ€. PizzaBuilderμ μμ±μμμ νμ νλλ€μ μΈμλ‘ λ°μ μ΄κΈ°ννκ³ , μ ν νλλ€μ build____ λ©μλλ₯Ό ν΅ν΄(Cook ν΄λμ€μ constructPizzaλ©μλμ²λΌ) λΉλν  μ μλλ‘ νλ€.


 
μ€μ  κ΅¬νμμλ μΈμμ κ°μκ° ν¨μ¬ λ§κ³ , μμμ κ°μ μλ ₯λ°μ μ μ₯νλ κ²½μ°κ° λ§λ€. Pizza μμμ²λΌ νλκ° 3κ°λ°μ μ‘΄μ¬νμ§ μλ κ²½μ°μλ μ μΈ΅μ  μμ±μ ν¨ν΄μ΄ λ μ ν©νλ€(μ½λμ λ³΅μ‘μ±). νμ§λ§ μΈμμ λ³κ²½ κ°λ₯μ±μ΄ μμ λ, λ§€κ° λ³μμ λλΆλΆμ΄ μ νμ  μ¬ν­μΌ λ λΉλ ν¨ν΄μ μ₯μ μΈ κ°λμ±κ³Ό μ μ§λ³΄μμ±μ΄ μ¦κ°νλ€.

Javaμμλ Lombokμ @Builder μ΄λΈνμ΄μμ μ΄μ©ν΄ Builderν¨ν΄μ μ¬μ©ν  μ μλ€.

<br>

## μ°Έκ³  μλ£

> - https://projectlombok.org/features/Builder  
> - https://www.informit.com/articles/article.aspx?p=1216151&seqNum=2  
> - https://johngrib.github.io/wiki/builder-pattern/
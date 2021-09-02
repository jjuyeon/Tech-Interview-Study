> [💡#](#casting) 업캐스팅과 다운캐스팅의 차이에 대해 설명해주세요.
> Upcasting이란 부모 클래스를 상속받은 자식 클래스의 객체를 부모 클래스 타입 레퍼런스 변수가 가리키도록 하는 것을 의미합니다. 이 때, 자식 클래스의 필드에는 접근할 수 없습니다. 
> Downcasting이란 Upcasting된 객체를 다시 자식 클래스 타입의 레퍼런스 변수가 가리키도록 하는 것을 의미합니다. 이 때 명시적 형변환을 해주어야 하며, 자식 클래스의 모든 필드에 접근 가능합니다.
> 
> [💡#](#string_pool) String형 객체를 ""로 만들었을 때와 new 키워드를 이용해서 만들었을 때의 차이점은? (String Pool)
> 큰따옴표를 이용해서 String 객체를 만들었을 때 에는 String Pool에 저장됩니다. 이 때, 이미 등록되었던 String인지 확인하는 과정을 거칩니다. new 키워드를 이용했을 때 에는 String Pool이 아닌 Heap에 String 객체가 저장되며, 같은 String 값을 가지고 있더라도 다른 객체로 생성됩니다.

# Casting

: Type 변환을 말한다. Java의 Reference Data Type의 변환에는 **Upcasting, Downcasting**이 존재한다. 기본적으로 관련이 없는 참조형 데이터 타입끼리는 변환이 불가능하다. 상속 관계(extends)를 가지거나, 구현 관계(implements)를 가졌을 때 Casting이 가능하다.

<br>

    Super Class
        ▲
        |
     Sub Class

<br>

## Upcasting

Super Class를 상속받은 클래스를 Sub Class라고 할 때, **Upcasting은 Super Class 형 레퍼런스 변수에 Sub Class 객체를 저장하는 것을 말한다.** Sub Class는 Super Class에 비해 항상 같거나 더 많은 필드를 가진다.

<br>

    Class Item {
      String name;
      Item(String name) {
        this.name = name;
      }
    }

    Class Book {
      String isbn;
      Book(String name, String isbn) {
        super(name);
        this.isbn = isbn;
      }
    }

    ...
    public static void main(String[] args){
      Item item = new Book("책입니다", "a1234");

      System.out.println(item.name);  //책입니다
      System.out.println(item.isbn);  //Compile Error
    }

Sub Class형 객체가 생성되어 힙에 저장되고, Super Class 타입의 레퍼런스 변수가 해당 객체를 가리킨다. Super Class 타입의 변수는 Sub Class에서 새롭게 정의된 필드에 접근할 수 없기 때문에 위의 코드의 마지막 줄은 컴파일 에러가 발생한다.

<br>

## Downcasting

DownCasting은 Upcasting의 반대 용어로, **Super Class형으로 Upcasting되었던 Sub Class 타입 객체를 다시 Sub Class형의 레퍼런스 변수가 가리키게 만드는 것**을 의미한다.
이 때, Upcasting의 반대라고 해서 Super Class형 객체를 생성해서 Sub Class 타입 레퍼런스 변수에 저장하면 안 된다. 위에서 강조한 것 처럼, Sub Class는 항상 Super Class보다 같거나 많은 필드를 가지기 때문이다.

    ...
    public static void main(String[] args){
      Item item = new Book("책입니다", "a1234");
      Book book = (Book)item;

      System.out.println(book.name);  //책입니다
      System.out.println(book.isbn);  //a1234
    }

명시적 형변환을 해주지 않아도 Upcasting을 해줄 수 있지만, Downcasting은 반드시 명시적으로 형변환을 해주어야 한다.
또한 Upcasting 된 변수에 대해서만 진행할 수 있다.

# String Pool

Java String API(java.lang.String)는 Java에서 Primitive Data Type과 Reference Data Type의 중간 성격을 띄는, 특별한 클래스이다.

String의 value는 immutable 특성을 가진다. 즉, 서로 다른 객체에 할당되는 같은 String 객체에 대해 별도의 공간을 지정하지 않아도 된다는 것 이다.

## String Interning
따라서 JVM은 Java String Pool이라는 특별한 메모리 공간을 두어 String 문자열을 관리한다

String Pool이라는 메모리 공간에 중복되지 않는 String 문자열을 저장하고, 해당 문자열에 접근할 때 마다 동일한 메모리 주소를 반환하면 할당해야 하는 메모리 공간을 절약할 수 있다.

이러한 프로세스를 interning이라고 한다.

 

실제로 우리가 String 변수를 생성하고 값을 할당하면 JVM은 String pool에서 동일한 value를 가진 String이 있는지 탐색한다

👉 이미 존재한다면, 새로운 메모리 공간을 할당하는 것이 아닌 해당 String의 reference를 반환한다


 
👉 존재하지 않는다면, pool에 새로운 String이 할당되고 해당 reference를 반환한다

    String constantString1 = "2jin";
    String constantString2 = "2jin";

    assertThat(constantString1)
      .isSameAs(constantString2);

## 생성자를 통해 할당된 String 객체
""(큰따옴표)를 이용해 할당했을 때와는 다르게, new 연산자를 이용한 String 객체 생성은 String Pool에 저장되지 않고 Java 컴파일러가 새로운 객체를 생성하여 Heap에 할당한다.

똑같은 String value를 가진 객체를 여러 개 생성해도, String pool로 관리할 때와 달리 별도의 메모리 공간을 계속해서 할당하는 것이다.

    String constantString = "2jin";
    String newString = new String("2jin");
    
    assertThat(constantString).isNotSameAs(newString);

## String Literal 🆚 String Object
new 키워드를 사용한 String Object가 생성될 때, Heap 메모리에는 항상 새로운 Object가 할당된다.

반면 큰따옴표를 이용한 String Literal을 생성할 때 에는, String Pool에 존재하는 기존의 객체를 반환하고 없을 시 새로운 객체를 생성하여 재사용을 위해 String Pool에 저장한다.

String Pool과 Heap 둘다에 저장되는 value는 둘다 String Object로 동일하지만, 가장 큰 차이점은 **new 키워드를 사용할 시 항상 새로운 객체가 생성된다**는 것이다.

![image](https://user-images.githubusercontent.com/30489264/131838576-fb5c6253-21c9-4e88-9915-733920fbdc45.png)

String Literal과 String Object의 차이점은 아래의 compare 예제를 통해 확인할 수 있다

    String first = "2jin"; 
    String second = "2jin"; 
    System.out.println(first == second); // True

    String third = new String("2jin");
    String fourth = new String("2jin"); 
    System.out.println(third == fourth); // False

    String fifth = "2jin";
    String sixth = new String("2jin");
    System.out.println(fifth == sixth); // False

first와 second를 비교했을 때, 둘 다 String Literal을 사용했으므로 String Pool의 같은 객체를 참조한다.

따라서 피연산자의 identity를 비교하는 == 연산자를 사용했을 때 True를 반환한다.
 
third와 fourth를 비교했을 때, 둘 다 new 키워드를 사용하여 String Object를 새롭게 생성했으므로 Heap에는 두 개의 String Object가 생성되고 당연히 다른 메모리를 참조하고 있다.

firth와 sixth를 비교했을 때 에도 firth는 String Pool, sixth는 Heap에 있는 객체를 참조하므로 동일하지 않다.

<br>

# 참고자료

> https://m.blog.naver.com/PostView.naver?isHttpsRedirect=true&blogId=dlaxodud2388&logNo=221642221204
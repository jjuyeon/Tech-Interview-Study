> [💡#](#casting) 업캐스팅과 다운캐스팅의 차이에 대해 설명해주세요.

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

<br>

# 참고자료

> https://m.blog.naver.com/PostView.naver?isHttpsRedirect=true&blogId=dlaxodud2388&logNo=221642221204
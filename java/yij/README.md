# :notebook_with_decorative_cover: JAVA

1. 객체지향에 대해서 설명하세요.

> 객체지향이란 상태와 행동을 지닌 객체를 설계하고 이를 구현하는 시스템을 의미합니다. 객체는 다른 객체들과 메시지를 통한 요청/응답으로 전체 시스템을 만들 수 있도록 협력적이어야 합니다. 또 그 과정에서 협력하는 방법을 간섭받지 않고 스스로 결정할 수 있도록 자율적이어야 합니다.  

1-1. 객체지향의 네 가지 특성에 대해 말해주세요.

> 상속, 추상화, 캡슐화, 다형성입니다.  
> 상속은 이미 존재하는 클래스를 다른 클래스가 물려받아 확장할 수 있는 것입니다. java에서는 extends, implements를 사용해서 상속을 구현합니다.  
> 추상화는 애플리케이션에 필요한 객체의 정보를 추출해서 표현하는 것입니다.   
> 캡슐화는 Java의 Access Modifier처럼 상태와 행동에 접근을 제한하는 것입니다.  
> 다형성은 하나의 기능을 다양한 방식으로 구현할 수 있게끔 하는 것을 의미합니다. java에서는 이를 Interface를 이용해서 구현했습니다.  


<br>

2. Java의 구동원리(컴파일 과정)를 설명해주세요.

> 자바 코드를 작성하면 자바 컴파일러가 이를 바이트코드로 변환합니다. 그리고 변환된 바이트코드를 JVM 클래스 로더에 전달합니다. 클래스 로더는 동적 로딩을 통해 클래스가 참조될 때 JVM 메모리의 메서드 영역에 저장합니다. JVM이 이것을 실행 엔진에 전달해서 바이트 코드를 실행합니다.

<br>

3. Garbage Collection에 대해 설명해주세요.

[Java Garbage Collection](https://2jinishappy.tistory.com/290)

> Garbage Collection이란 더 이상 참조되지 않는 객체를 찾아내어 메모리에서 삭제하는 과정을 말합니다.  
> C/C++같은 언어에서는 프로그래머가 직접 메모리를 할당하고 해제해야 했습니다. 이 과정에서 메모리를 제 때 해제하지 않으면 메모리 누수가 발생할 수 있는데, Java에서는 이를 JVM이 관리해줍니다.

3-1. Garbage Collection의 단점은 무엇이 있을까요?

> 'stop-the-world'가 발생하는 시점에서 애플리케이션의 실행이 멈춥니다. JVM은 가비지 컬렉션이 일어날 때 GC를 실행하는 쓰레드를 제외한 모든 쓰레드의 동작을 멈춥니다. 따라서 'stop-the-world' 시간이 길어지면 애플리케이션의 성능에 문제가 발생할 수 있습니다.


3-2. Garbage Collection 동작에 대해 구체적으로 설명해주세요.

> Java는 GC를 위해 메모리를 Young/Old Generation으로 나누고 Young Generation을 다시 Eden/Survival 0/Survival 1영역으로 나눕니다.  
> Eden영역은 새롭게 할당된 메모리 영역을 의미하고, 이 영역에서 메모리가 차면 Minor GC가 발생, **현재 사용중인 Survival 영역**으로 개체들을 이동시킵니다.
> 해당 Survival 영역이 다 차면 또 GC가 발생하고, **사용중이지 않던 Survival 영역**으로 개체들을 이동시키는 과정을 반복합니다.  
> 이 과정에서 Age가 커진 개체들은 Old Generation 영역으로 이동합니다.

3-3. Major GC 알고리즘에 대해 설명해주세요.

> Parallel GC  
> : Mark-Sweep-Compact 알고리즘을 사용해서 살아남은 객체를 식별하고, 참조되지 않는 객체는 해제한 뒤, 선택적으로 메모리를 재배열합니다. 이 과정을 여러개의 쓰레드를 이용해서 가비지 컬렉션을 하는 알고리즘이 Parallel GC입니다.  
> 
> G1GC
> : 힙을 일정한 크기의 영역들로 나누어서 사용하던 영역이 꽉 차면 GC를 실행하고 다른 영역으로 이동시키는 알고리즘입니다. 자바 9부터 JVM의 기본 GC 알고리즘으로 사용하고 있으며, 다른 GC 알고리즘에 비해 STW 시간이 짧다는 장점이 있습니다.

<br>

4. 쓰레드란 무엇이고, 싱글쓰레드와 멀티쓰레드의 차이를 설명해주세요.

> 쓰레드란 한 프로세스 안에서 존재하는 여러 개의 실행 단위를 의미합니다.  
> 프로세스는 코드, 힙, 데이터, 스택영역을 가지고 있는데 각각의 쓰레드마다 독립적인 스택 영역을 지닙니다. 멀티 쓰레드를 사용하면 프로그램을 병렬적으로 실행해서 빠르게 작업을 처리할 수 있지만 공유 자원의 접근을 제한하지 않으면 문제가 발생할 수 있습니다.

4-1. 멀티쓰레드의 문제점과 해결방안을 얘기해주세요.

> 여러 쓰레드는 자원을 공유하기 때문에 동시에 접근하면 문제가 발생할 수 있습니다. 이를 막기 위해 Thread-safe하게 프로그램을 설계해야 합니다.

4-2. Thread-safe가 무엇인가요?

> 쓰레드를 사용한 병렬 프로그래밍을 하면서도 공유 자원을 안전하게 사용하는 방식입니다. Java에서는 필드를 불변객체로 만들거나, 상태를 저장하지 않는 클래스를 이용해서 쓰레드 세이프를 구현할 수 있습니다.

<br>

5. 클래스는 무엇이고, 객체는 무엇인지 설명해주세요.

> 클래스는 객체가 가질 수 있는 상태와 행동에 대해 구체적으로 정의한 것을 말합니다. 객체는 클래스의 특징을 가져서 구현된 실체를 의미합니다. 인스턴스는 애플리케이션 내에서 살아있고, 식별자를 가진 각각의 객체를 의미합니다.

5-1. 객체와 인스턴스의 차이가 뭘까요?

> Java를 예를 들자면, 클래스 타입으로 선언했을 때 객체, 실제 메모리에 올라간 식별 가능한 대상을 인스턴스라고 정의할 수 있습니다. 하지만 이를 명확하게 분리하기는 어렵다고 생각합니다.

<br>

6. 인터페이스와 추상클래스의 차이점은 무엇인지 설명해주세요.

[Inerface vs Abstract Class](https://2jinishappy.tistory.com/281?category=936901)

> 인터페이스와 추상클래스 모두 인스턴스화 할 수 없고, 구현 여부에 상관 없이 메서드 집합을 가질 수 있습니다. 단, 인터페이스를 이용하면 다중 상속이 가능하지만 추상 클래스는 이것이 불가능합니다. 인터페이스는 다형성, 추상클래스는 상속의 개념에 더욱 가깝습니다.

6-1. 인터페이스가 무엇인가요?

> 필드 상수와 메서드 정의부를 가진 타입을 의미합니다. 인터페이스를 상속받은 클래스는 메서드 정의부를 구현할 의무를 가집니다. Java는 인터페이스를 이용해서 다중 상속을 구현할 수 있습니다.

6-2. 추상클래스가 무엇인가요?

> 하위 클래스가 상속받아 재정의할 수 있도록 일부 또는 전체 메서드의 정의부를 구현하지 않은 클래스를 의미합니다. 상속받은 클래스는 이것을 다양하게 정의하고 확장할 수 있습니다.

6-3. 어떤 때 인터페이스를 사용하고 추상클래스를 사용해야 할까요?

> 해당 개념이 하위 클래스의 상위 분류에 속한다면 추상클래스, 그것이 아니라 공통된 기능을 구현하기 위해서라면 인터페이스를 사용해야 한다고 생각합니다. 예를 들면, 포유류의 특징을 추상화해서 하위클래스가 상속받는다면 추상클래스가 더 적합합니다. 하지만 사람과 물고기가 수영하는 법이 다르듯 공통된 행동을 다르게 구현하기 위해서는 인터페이스가 더 적합합니다.

<br>

7. 직렬화가 무엇인지 설명하세요.

> 객체를 영속화 하기 위한 방법으로써, 자바에서는 객체를 바이트 스트림으로 변환하는 것을 의미합니다. 시스템 간의 데이터 교환을 위해 사용합니다.

<br>

8. Call by Value와 Call by Reference의 차이에 대해 설명해주세요.

<details><summary>기존</summary>
> 정의부 -> [Return type] Method ([DataType] parameter, ..)
> 
> 호출부 -> Method(argument, ..)
> 
> 메서드의 정의 및 호출이 위와 같은 형식으로 이루어져 있다고 가정합니다.
> 
> **Call by Value**: 메서드를 호출할 때 argument로 전달한 인자를 복사해서 Method 내의 parameter로 전달하는 방식 
> - Method 내에서 접근하는 parameter는 지역 변수로 새롭게 복사되어 사용하는 값입니다.
> - Method 내의 parameter의 값을 변경해도 호출부의 argument에 영향이 가지 않습니다.
> - Method 내의 parameter의 Life Time은 Method의 return문이 수행될 때 까지입니다.(메서드 수행이 끝난 뒤 소멸됩니다)
> 
> **Call by Reference**: 메서드를 호출할 때 argument로 전달한 인자의 주소값을 전달하여 Method 내에서 실제 argument를 접근하는 방식
> - Method 내에서 접근하는 parameter는 실제 argument가 위치하는 메모리를 가리킵니다
> - Method 내의 parameter의 값을 변경하면 호출부의 argument가 가리키는 변수 또한 변경됩니다.
> - Method의 수행이 종료되어도 사라지지 않습니다.</details>

<br>

> Call By Value는 메서드로 인자를 전달할 때 값을 복사해서 넘겨주는 방식, Call By Reference는 실제 값을 복사하는 것이 아니라 해당 인자를 참조하는 레퍼런스를 넘겨주는 방식입니다. 따라서 전달받은 메서드에서 실제 객체의 값을 변경할 수 있느냐의 차이가 존재합니다.

8-1. Java에서 Call By Reference를 사용하는 방식에 대해 얘기해주세요.

> Java는 Pass By Reference 방식을 사용합니다. 메서드에 인자를 전달할 때 변수를 새로 할당해서 해당 힙 객체의 레퍼런스를 전달하게 됩니다. 해당 변수에 new 키워드를 이용해 객체를 할당하면 변수가 새로운 객체를 가리키게 되고, 기존의 변수에는 영향이 없습니다.

<br>

9.  Catched Exception과 Uncatched Exception의 차이를 설명해주세요.

[Catched and Uncatched Exception](https://2jinishappy.tistory.com/292)

<details><summary>기존</summary>
> Uncatched Exception
> : Throwable 클래스를 상속받는 클래스 중 Error 클래스와, Exception - RuntimeException를 포함한 서브 클래스를 포함하는 Exception을 가리킵니다.
> - 코드 작성 시점에는 에러가 발생하지 않지만, Runtime 시점에 에러가 발생합니다.
> - Transaction의 대상이 되어 에러가 발생할 시 Rollback이 발생합니다.
> - NPE, ClassCastException 등이 대표적인 예시입니다.
> 
> Catched Exception
> : Exception의 서브 클래스 중 RuntimeException의 서브 클래스가 아닌 클래스들을 포함하는 Exception을 가리킵니다.
> - Compile 시점에 에러가 발생하기 때문에 에러 핸들링을 해주어야 합니다.
> - Transaction의 default 대상에 포함되지 않아 에러가 발생해도 Rollback이 자동으로 일어나지 않습니다.
> - ClassNotFoundException, IOException 등이 대표적인 예시입니다.</details>

<br>

> Catched Exception이란 컴파일 타임에 에러가 발생하는 예외를 의미합니다. ClassNotFoundException이 대표적입니다. Uncatched Exception이란 런타임 중 발생하는 에외를 의미합니다. NPE, ClassCastException이 대표적입니다. 트랜잭션의 대상이 되어서 에러가 발생할 시에 롤백이 일어납니다.

<br>

### :notebook_with_decorative_cover: JAVA 자료구조


2. Java의 HashMap과 HashTable의 차이점을 설명해주세요.

[Java HashMap vs HashTable](https://2jinishappy.tistory.com/233?category=936901)

|                             | HashMap                                                                                                 | HashTable                                  |
| --------------------------- | ------------------------------------------------------------------------------------------------------- | ------------------------------------------ |
| 등장 시기                   | Java 2                                                                                                  | JDK 1.0                                    |
| Map Interface 구현 여부     | O                                                                                                       | O                                          |
| associative array 지칭 용어 | Map                                                                                                     | Dictionary                                 |
| 특징                        | - Java Collection Framework<br> - 보조 해시 함수를 이용하여 collision을 덜 발생시킴<br> - 지속적인 개선 | JRE 1.0, JRE 1.1 대상으로 하위 호환성 제공 |

<br>

3. Java의 원시타입들은 무엇이 있으며 각각 몇 바이트를 차지하는지 설명해주세요.

[Java Primitive Data Type](https://2jinishappy.tistory.com/288?category=936901)

| data type | size    |
| --------- | ------- |
| byte      | 1byte   |
| short     | 2byte   |
| int       | 4byte   |
| long      | 8byte   |
| float     | 4byte   |
| double    | 8byte   |
| boolean   | ⭐1byte |
| char      | 2byte   |

> why boolean은 1byte일까?  
> 운영하는 측에서 1bit를 관리할 수 있는 능력이 없음

<br>

4. String, StringBuffer, StringBuilder의 차이를 설명하세요.

[String vs StringBuffer vs StringBuilder](https://2jinishappy.tistory.com/259?category=936901)

<br>

![image](https://user-images.githubusercontent.com/30489264/130449865-da5859ef-6e45-45c3-9913-71b0f827cf50.png)

<br>

5. 오버라이딩과 오버로딩이 무엇이며 어떤 차이점이 있는지 설명해주세요.

[Overriding vs Overloading](https://2jinishappy.tistory.com/284?category=936900)

> Overriding: 자식(서브) 클래스가 부모(슈퍼) 클래스에 정의된 메소드를 재정의 할 수 있는 기능.
> Overloading: 같은 함수 이름, 다른 함수 파라미터를 가지는 여러 메소드를 정의할 수 있는 기능.
> 기본적으로 오버로딩은 상속, 오버라이딩은 다형성에 관련되어 있습니다.

<br>

6. Generic에 대해 설명해주세요.

<br>


### :notebook_with_decorative_cover: JAVA 라이브러리 & 프레임워크

1. '=='과 'equals()'의 차이에 대해 설명하세요.

> 기본적으로 '=='은 동일성(identity), 'equals()'는 동등성(equality)에 기반합니다.  
> 
> '=='은 비교하고 있는 두 피연산자가 같은 식별자인지, 'equals()'는 비교하고 있는 두 피연산자의 상태(value)가 동일한지 비교합니다.
> 
> 따라서 
> 
> ``` java
> String a = "Hello";
> String b = "Hello";
> 
> System.out.println(a==b);   //print false
> System.out.println(a.equals(b));  //print true
> ```
> 같은 상태를 가진 서로 다른 두 객체를 
> - '=='로 비교하면, 두 객체의 식별자가 다르기 때문에 false를 return합니다.
> - 'equals()'로 비교하면, 두 객체의 현재 상태인 value가 같기 때문에 true를 return합니다.
> 
> ``` java
> String a = new String("Hello");
> String b = new String("Hello");
> ```
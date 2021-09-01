# :question: JAVA 의 자료구조

#### reference
https://velog.io/@gillog/%EC%9B%90%EC%8B%9C%ED%83%80%EC%9E%85-%EC%B0%B8%EC%A1%B0%ED%83%80%EC%9E%85Primitive-Type-Reference-Type<br>
https://preamtree.tistory.com/15<br>
https://blog.naver.com/PostView.nhn?isHttpsRedirect=true&blogId=thdansgur&logNo=220910298503<br>
https://stage-loving-developers.tistory.com/8<br>
https://github.com/WooVictory/Ready-For-Tech-Interview/blob/master/Java/%5BJava%5D%20%EC%98%A4%EB%B2%84%EB%9D%BC%EC%9D%B4%EB%94%A9%EA%B3%BC%20%EC%98%A4%EB%B2%84%EB%A1%9C%EB%94%A9.md<br>
https://ltk3934.tistory.com/81<br>
https://github.com/HyeminNoh/Tech-Stack/blob/master/docs/Java/Generics.md
<hr>

## Question
1. [Java의 HashMap과 HashTable의 차이점을 설명해주세요.]()
<br><br>

2. [Java의 원시타입들은 무엇이 있으며 각각 몇 바이트를 차지하는지 설명해주세요.](#2-1-원시타입기본타입)
<br><br>

3. [Wrapper Class에 대해 설명해주세요.](#3-wrapper-class)
- 기본타입에 해당하는 데이터를 객체로 포장해주는 클래스입니다. null처리가 가능하므로, 직접적인 산술 연산이 불가능합니다. 즉, 참조타입끼리의 연산은 불가능하기 때문에 기본타입으로 변환하여 연산해야한다는 주의점이 있습니다.
<br><br>

4. [오토 박싱과 언박싱에 대해 설명해주세요.](#4-1-auto-boxing--auto-unboxing)
- 박싱은 원시타입을 Wrapper class로 바꿔주는 것이고, 언박싱은 반대로 Wrapper class에서 원시타입으로 바꿔주는 것을 의미합니다. JAVA 1.5 부터 오토 박싱/오토 언박싱 기능이 추가되었는데, 명시적으로 원시타입을 참조타입으로 감싸주지 않아도 컴파일러가 자동으로 박싱/언박싱합니다.
<br><br>

5. [업캐스팅과 다운캐스팅의 차이에 대해 설명해주세요.](#5-upcasting--downcasting)
- 업캐스팅은 수퍼 클래스의 변수에 서브 클래스의 객체가 들어가는 것을 의미하고, 업캐스팅된 변수의 타입을 서브 클래스로 다시 되돌리는 것을 다운캐스팅이라고 합니다.
- 서브 클래스 객체는 수퍼 클래스의 메소드를 상속 받기 때문에 수퍼 클래스의 변수에 들어가 수퍼 클래스인 것처럼 사용될 수 있습니다. 또한, 업캐스팅된 변수의 타입이 다시 서브 클래스로 돌아와 본인의 클래스 객체인 것처럼 사용할 수 있습니다.
<br><br>

6. [오버라이딩과 오버로딩이 무엇이며 어떤 차이점이 있는지 설명해주세요.](#2-오버라이딩-vs-오버로딩)
- 오버라이딩은 상속 받은 메소드의 내용만 변경하는 것이고, 오버로딩은 기존에 없던 새로운 메소드를 정의하는 것입니다.
- 큰 차이점으로는 오버라이딩은 부모 클래스로부터 메소드를 그대로 받아 재사용하는 것이기 때문에 메소드 이름, 매개변수, 반환타입이 같아야합니다. 반면, 오버로딩은 메소드의 이름은 같으나, 매개변수의 개수, 자료형, 선언 순서가 다른 새로운 함수를 정의한다는 것입니다.
<br><br>

7. [Generic에 대해 설명해주세요.](#3-generic)
- 제네릭은 메소드나 클래스에서 사용할 내부 데이터 타입을 컴파일 시에 미리 지정하는 방법입니다. 타입변수 ```<T>```를 사용하면서 다운 캐스팅이 필요없게 되었고, 클래스나 메소드 내부에서 사용되는 객체의 타입 안정성을 높일 수 있습니다.
<hr>

## :nerd_face:	What I study
### 2. Java의 데이터 타입
- 원시타입(Primitive Type): 실제 데이터 값을 저장하는 타입
- 참조타입(Reference Type): 메모리 번지 값을 통해 객체를 참조하는 타입
#### 2-1) 원시타입(기본타입)
|종류|데이터형|크기 byte|크기 bit|
|:---:|:---:|:---:|:---:|
|논리형|boolean|1|8|
|문자형|char|2|16|
|정수형|byte|1|8|
|정수형|short|2|16|
|정수형|int|4|32|
|정수형|long|8|64|
|실수형|float|4|32|
|실수형|double|8|64|

- boolean
  - Java가 데이터를 다루는 최소 범위가 1byte이기 때문에 낭비적이지만 1byte를 사용한다.
- char
  - Java의 경우 Unicode를 사용하므로, 동양의 글자를 사용하기 위해 2byte를 사용한다.
  - Java에서 유일하게 제공하는 unsigned 형태의 데이터 타입이다.
    - unsigned: 0부터 시작하여 양수 값만 가지는 데이터 형태
- 정수형 데이터 타입
  - JVM의 피연산자 stack이 피연산자를 4byte 단위로 저장하기 때문에, 정수형 데이터를 사용하게 되면 JVM에서 기본적으로 int형 데이터타입의 데이터로 인식을 해주게 된다.
  - int보다 작은 자료형의 값을 계산시 int형으로 형변환 되서 연산이 수행된다.
  - int형 데이터 타입의 범위를 넘어서는 경우에는 정수 데이터 맨 뒤 쪽에, 접미사 'l' 또는 'L'을 붙여야한다.
- 실수형 데이터 타입
  - double형 데이터 타입이 기본 데이터타입이다.
  - float형 데이터 타입보다 double형 데이터 타입이 2배정도 더 정밀한 데이터를 표현할 수 있다.
  - float형 데이터 타입을 사용하고자 하는 경우에는 실수 데이터 맨 뒤 쪽에, 접미사 'f' 또는 'F'를 붙여야한다.
#### 2-2) 참조타입
- 원시타입을 제외한 모든 데이터 타입 ex) 배열, 클래스, 인터페이스 ...
- Java에서 실제 객체는 Heap 영역에 저장된다.
- 참조타입은 Stack 영역에 실제 객체들의 주소를 저장하여, 객체를 사용할 때마다 참조변수에 저장된 객체의 주소를 불러와 사용한다.
- 원시타입 변수들과는 다르게 크기가 정해져있지 않다.
- 프로그램 실행 시 메모리에 동적으로 할당된다.
- 참조하지 않는 변수가 있으면 GC(Garbage Collector)가 해당 메모리 공간을 해제한다.
- 정적 메모리 Stack 영역
  - 원시타입 변수가 할당되고 변수의 실제 값들이 저장된다.
  - 참조타입 변수에 대해서는 Heap 영역에서 생성된 객체들의 주소값을 저장한다.
  - 객체 안의 메소드의 작업이 종료되면 할당되었던 Stack 공간은 지워진다.
- 동적 메모리 Heap 영역
  - 객체와 배열이 생성된다.
  - 참조타입 변수들이 생성된 객체, 배열의 주소를 Stack 영역에 저장한다.
#### 2-3) 원시타입 vs 참조타입
||원시타입|참조타입|
|:---:|:---:|:---:|
|NULL|X|O|
|Generic Type|X|O|
|접근속도|빠름(스택 한번 참조)|느림(스택, 힙 참조 & 값이 필요할 때마다 언박싱 과정 필요)|
|메모리 양|적음|많음|
- cf) 엄청 큰 숫자를 복사해야 한다면, 참조값만 넘길 수 있는 참조타입이 속도면에서 좋을 수도 있다.

<br><br>

### 3. Wrapper Class
- 기본타입(원시타입)의 자료형을 객체로 포장해주는 클래스이다.
  - ex) 메소드의 매개변수로 객체타입만이 요구될 때 (generic, collection ...)
- 각각의 타입에 해당하는 데이터를 인수로 받아, 해당 값을 가지는 객체로 변환해준다.
- null 처리가 가능하다.
  - SQL과 연동할 경우 처리가 용이하다.
  - 직접적인 산술 연산은 불가능하다.
- 산술 연산을 위한 클래스가 아니기 때문에, 인스턴스에 저장된 값을 변경할 수 없다.
  - 값에 대한 변경은 불가능하지만, 새로운 값(객체)의 할당이나 참조는 가능하다.
  - 참조타입끼리의 연산은 불가능하기 때문에 기본타입으로 변환하여 연산해야한다.
  - cf) 기본타입은 선언 시 stack 영역에 저장되며, 산술 연산이 가능하다.
- ```==``` 연산자를 사용하여 동등 연산을 하고자 할 때는, ```.longValue()```, ```.intValue()``` ... 메소드 등을 통해 원시타입으로 변경해서 비교 연산을 수행해야 한다.
  - Wrapper Class는 객체이므로 ```==``` 연산자를 사용하면 값을 비교하는 것이 아니라, 두 인스턴스의 주소값을 비교한다.
- 그러므로, 인스턴스에 저장된 값의 비교 연산을 제대로 사용하려면 ```equals()``` 메소드를 사용해야 한다.
- 모두 ```java.lang``` package에 포함되어 제공된다.

|원시타입|Wrapper 클래스|
|:---:|:---:|
|byte|Byte|
|short|Short|
|int|Integer|
|long|Long|
|float|Float|
|double|Double|
|char|Character|
|boolean|Boolean|

<br><br>

### 4. Boxing & Unboxing
- Boxing: 원시타입 ->  Wrapper Class (객체)
- Unboxing:  Wrapper Class (객체) -> 원시타입
```java
// Boxing
int i = 10;
Integer num = new Integer(i);

// Unboxing
Integer num = new Integer(10);
int i = num.intValue();
```
#### 4-1) Auto Boxing & Auto UnBoxing
- JAVA 1.5부터 Auto Boxing/Unboxing 기능이 추가되어, **명시적으로 원시타입을 참조타입으로 감싸주지 않아도 컴파일러가 자동으로 Boxing/Unboxing을 해준다.**
- 내부적으로 추가 연산 작업이 존재하므로, 불필요한 Auto Casting이 이루어지지 않도록 조심해야한다.
- 메모리 누수의 원인이 될 수도 있다.
  - Wrapper Class는 ```Immutable```한 특징을 가진다. 그러므로, Auto Boxing이 일어날 때마다 새로운 객체가 생성된다. (불필요한 객체가 생성될 가능성이 있다.)
```java
int i = 10;
Integer num = i; // Auto Boxing

List<Integer> list = new ArrayList<>();
lists.add(10); // Auto Boxing

Integer num = new Integer(10);
int i = num; // Auto UnBoxing


Integer num2 = new Integer(20);
Integer num3 = new Integer(10);
System.out.println(num1 < num2); // true
System.out.println(num1 == num3); // false
System.out.println(num1.equals(num3)); // true
```
<br><br>

### 5. Upcasting & Downcasting
- 변수의 데이터 타입을 바꿔야 할 때가 있다. (ex. int + double)
- 변수의 데이터 타입을 바꿔주는 작업을 **타입의 형변환**이라고 한다.
  - **Upcasting** : 자동 형변환, 묵시적 타입 변환
  - **Downcasting** : 강제 형변환, 명시적 타입 변환

#### 5-1) Upcasting
- 프로그램 실행 도중에 자동으로 형변환이 일어나는 것
- ***작은*** 메모리 크기의 데이터 타입 **->** ***큰*** 메모리 크기의 타입으로 변환
  - 단, **메모리 크기에 상관없이 정수는 모든 실수 데이터 타입에 자동 형변환이 가능하다.**
- 객체타입의 경우는 ***자식클래스***의 객체 **->** ***부모클래스*** 타입으로 변환
  - 부모클래스로 변환되면, 부모클래스 본인의 메소드에는 모두 접근 가능하지만 자식클래스에는 접근이 불가능하다.
- 별다른 문법이 적용되지 않는다.
- ***[주의]*** 작은 크기에서 큰 크기의 데이터 타입으로 변환한다하더라도, **타입 범위가 다르다면 자동 형변환이 불가능하다 ! !**
<br>

![Upcasting](https://github.com/GimunLee/tech-refrigerator/raw/master/Language/JAVA/resources/java-promotion-casting-001.png)

```java
byte bNum = 10; // byte 데이터 타입
int num = bNum; // byte 데이터 타입 변수 bNum을 int 데이터 타입 변수인 num에 저장

char ch = 'A'; // char 데이터 타입
int num = ch; // char 데이터 타입 변수 ch를 int 데이터 타입 변수 num에 저장 (65)

byte bNum = 10; // byte 데이터 타입
char ch = bNum; // char 데이터 타입 (Compile Error - char는 음수 저장이 불가능하다.)

float fNum = 10.0; // float 데이터 타입
long lNum = fNum; // long 데이터 타입 (Compile Error - 실수형에서 정수형으로 자동 형변환은 불가능하다.)

// 상속 관계에서의 자동 형변환
Parent parent = new Child(); // Child 클래스에는 Parent 클래스의 속성이 포함되어 있다.
```

#### 5-2) Downcasting
- Upcasting에 반대되는 캐스팅
- ***큰*** 메모리 크기의 타입 **->** ***작은*** 메모리 크기의 타입으로 변환
- 객체타입의 경우는 ***부모클래스***의 객체 **->** ***자식클래스*** 타입으로 변환
- 명시적으로 타입을 지정해야 한다.
```java
int num = 1; // int 데이터 타입
byte bNum = (byte) num; // byte 데이터 타입 범위 안에 충분히 들어가는 값(1)

int num = 2222222222; // int 데이터 타입
byte bNum = (byte) num; // byte 타입의 범위를 넘어감 (Error X, 그러나 실제 결과 값은 overflow로 엉뚱한 값이 나온다.)

// 상속 관계에서의 형변환
Parent p = new Child(); // 업캐스팅
Child c = (Child) p; // 다운캐스팅 (업캐스팅된 특성을 되돌리기 위해 다운캐스팅을 진행한다.)
```
#### cf) 헷갈리는 상속 관계에서의 Casting [참고](https://github.com/gyoogle/tech-interview-for-developer/blob/master/Language/%5Bjava%5D%20Casting(%EC%97%85%EC%BA%90%EC%8A%A4%ED%8C%85%20%26%20%EB%8B%A4%EC%9A%B4%EC%BA%90%EC%8A%A4%ED%8C%85).md)
```java
class Parent {
	int age;

	Parent() {}

	Parent(int age) {
		this.age = age;
	}

	void printInfo() {
		System.out.println("Parent Call!!!!");
	}
}

class Child extends Parent {
	String name;

	Child() {}

	Child(int age, String name) {
		super(age);
		this.name = name;
	}

	@Override 
	void printInfo() {
		System.out.println("Child Call!!!!");
	}

}

public class Main {
    public static void main(String[] args) {
        Parent p = new Child(10. "student"); // Parent 클래스의 변수, 메소드만 접근할 수 있다.
        System.out.println(p.age); // 10
        System.out.println(p.name); // Error 발생, Parent 클래스의 변수만 접근 가능
        p.printInfo(); // Child Call!!!!

        Child c = (Child) p; // 다운캐스팅
        System.out.println(c.age); // 10
        System.out.println(c.name); // student
        
        Child c = (Child) new Parent(); // Runtime Error(ClassCastException) 발생, 컴파일할 때는 데이터형의 일치만 확인한다.
    }
}
```

#### 5-3) 형변환 연산
- 기본적인 사칙연산은 같은 타입의 피연산자 간에만 수행된다.
- 서로 다른 데이터 타입의 피연산자가 있을 경우, 두 피연산자 중 크기가 큰 타입으로 자동 형변환이 된 후에 연산이 수행된다.
```java
// int 타입 변수값과 double 타입 변수값을 더해서 double 타입의 결과가 나옴.
int num = 10;
double dNum = 10.5;
double result = num + dNum; 

// int 타입의 결과를 얻고 싶다면, double 타입 변수를 다운캐스팅하면 된다.
int num = 10;
double dNum = 10.5;
int result = num + (int) dNum;
```
<br><br>

### 6. 오버라이딩 vs 오버로딩
- 다형성과 관련있는 용어: 이를 사용하면 다형성을 높여준다.
- 같은 이름의 다른 함수를 호출한다.

||오버라이딩(Overriding)|오버로딩(Overloading)|
|:---:|:---:|:---:|
|메소드 이름|동일|동일|
|매개변수의 개수, 타입, 순서|동일|다름|
|반환 타입|동일|상관없음|
#### 6-1) 오버라이딩(Overriding)
- 상위 클래스 혹은 인터페이스가 가지고 있는 메소드를 하위 클래스에서 재정의해서 사용하는 것
- 자식 클래스가 부모 클래스로부터 메소드를 그대로 받아서 재사용하기 위해 쓰는 것이다. 
- 상속 관계에 있는 클래스 간에 같은 이름의 메소드를 정의한다.
- 메소드 이름, 매개변수, 반환타입이 같아야한다.
- **접근지정자는 부모 클래스의 메소드보다 같거나 접근범위가 더 큰 접근지정자를 사용해야 한다.**
- 상속 시, 상위 클래스의 private 멤버를 제외한 모든 멤버를 상속받는다.
- 예외 처리 시(try-catch), 부모 메소드보다 예외의 개수가 더 많으면 안된다.
- 생산성 & 유지보수에 효과적이다. 
- 개발자의 실수를 방지하기 위해, 오버라이딩할 때 @Override 어노테이션을 쓰는 것을 권장한다.

#### 6-2) 오버로딩(Overloading)
- 여러 개의 메소드가 같은 이름을 갖고 있으나, **매개변수의 자료형이나 인자의 수 또는 선언 순서를 다르게 하여 정의하는 것**
-  한 클래스 내에서 같은 이름의 메소드를 여러번 정의한다.
- 매개변수(자료형, 개수, 선언 순서)는 같고 **반환 타입이나 접근 제한자가 다른 경우는 오버로딩이 성립하지 않는다.**
- 즉, 오버로딩된 메소드들은 **매개변수에 의해서만 구별될 수 있다.**
- 다양한 상황에서 메소드가 호출될 수 있도록 한다.

<br><br>

### 7. Generic
![generic](https://gmlwjd9405.github.io/images/basic-concepts-of-development/generics.png)
- 메소드나 클래스에서 사용할 내부 데이터 타입을 컴파일 시에 미리 지정하는 방법이다.
- 다양한 타입의 객체들을 다루는 메소드나 컬렉션 클래스에서 사용하며, 컴파일 과정에서 타입을 지정한다.
  - 기존에는 Object 객체를 사용했지만, 이제는 ```<T>```를 사용하여 타입을 지정한다.
- 객체의 타입을 컴파일 시에 체크하기 때문에, 잘못된 타입이 사용될 수 있는 문제를 컴파일 과정에서 제거할 수 있다.
  - 다운 캐스팅이 필요 없다.

#### 7-1) 장점
- 메소드나 클래스 내부에서 사용되는 타입의 안정성을 제공한다.
- API를 설계하는데 있어 보다 명확한 의사전달이 가능하다.
- 형 변환을 생략할 수 있으므로 코드가 간결해진다.
  - 컬렉션 내부에서 들어온 값이 내가 원하는 값인지 별도의 로직 처리를 구현할 필요가 없다.

#### 7-2) Generic 클래스 선언
```java
// 제네릭 타입 T를 선언
class Box<T> { // class<Object>
  T item; // object

  void setItem(T item) { // void method(Object)
    this.item = item;
  }

  T getItem() { // return Object
    return item;
  }
}

Box<String> b = new Box<String>(); // T 대신 실제 타입 지정

b.setItem(new Object()); // ERROR: String타입 이외의 타입은 지정불가
b.setItem("ABC"); // OK: String타입만 가능

// String item = (String)b.getItem(); 
String item = b.getItem(); // 형변환 필요없음

/* 호환성 - T를 사용 */
Box b2 = new Box(); // OK: T는 Object로 간주된다.
b2.setItem("ABC"); // WARN: unchecked or unsafe operation.
b2.setItem(new Object()); // WARN: unchecked or unsafe operation.
```
- Box 클래스
  - 어떤 타입이든 한 가지 타입을 정하여 넣을 수 있다.
- T: 타입 변수
  - T가 아닌 다른 것도 사용가능하다.
  - 상황에 맞게 의미 있는 문자를 선택해서 사용하는 것이 좋다. (T: type, E: element, K: key, V: value, ...)

#### 7-3) 제한 사항
- 모든 객체에 동일하게 동작해야하는 static 멤버에는 타입변수 T를 사용할 수 없다.
  - T는 인스턴스 변수로 간주되기 떄문에, static에는 인스턴스 변수를 참조할 수 없다.
- 제네릭 타입의 배열은 선언은 가능하나, 생성은 불가능하다.
  - ```new``` 연산자, ```instacneof``` 연산자는 컴파일 시점에 타입 T가 뭔지 정확하게 알아야 하기 때문에 T를 피연산자로 사용할 수 없다.
- 제네릭 타입에 ```extends```를 사용하면, 특정 타입의 자손들만 대입할 수 있도록 제한할 수 있다.
  ```java
  class FruitBox<T extends Fruit>{ // Fruit의 자손만 타입으로 지정 가능
    ArrayList<T> list = new ArrayList<T>();
  }
  FruitBox<Apple> appleBox = new FruitBox<Apple>(); // Apple은 Fruit의 자손
  FruitBox<Toy> toyBox = new FruitBox<Toy>(); //error. Toy는 Fruit의 자손이 아님
  ```

#### 7-4) 제네릭 메소드
- 메소드의 선언부에 제네릭 타입이 선언된 메소드이다.
```java 
static <T> void sort(List<T> list, Comparator<? super T> c)
```
- 제네릭 클래스의 타입 매개변수 T와 제네릭 메서드의 타입 매개변수 T는 서로 같은 문자를 쓰더라도 ***별개의 것***이라는 것을 잊지말자.
- 제네릭 메서드는 제네릭 클래스가 아닌 일반 클래스에도 정의할 수 있다.
- static 멤버에는 타입 매개변수를 사용할 수 없지만, static 메소드에서는 사용할 수 있다.
- 메소드에 선언된 타입은 메소드 내에서만 지역적으로 사용되는 local value의 특성을 가진다.
# Kotlin

# Kotlin이란?

## Kotlin의 특징

### Statically Typed

기본이 정적 타입 언어이므로 컴파일 시 변수의 Data Type이 결정되어 **안전**하다

(컴파일러가 문제를 컴파일 시점에 발견하므로)

Statically Typed vs. Dynamically Typed

- 정적 타입: C, C++, Java, Kotlin, ...
- 동적 타입: Javascript, Python, Object-C, PHP, ...

동적 타입 언어는 유연하게 데이터 타입을 바꿀 수 있다는 장점이 있지만 컴파일 시점에 발견할 수 없기 때문에 논리적 에러가 발생할 수 있다

```python
# Python 코드 예시
m = 13         # int
m = "Hello"    # str
m = 13.0       # float
```

반면, 코틀린은 데이터 타입을 명시해주기 때문에 정수형이 아닌 데이터는 할당할 수 없다

```kotlin
var m: Int = 13
// var m = 13
// type inference를 통해 위와 같은 표현도 가능! 

m = 15         # ok
m = "Hello"    # error
m = 15.0       # error
```

🎱 단, Kotlin/JS를 쓸 때는 동적 타입 지원을 위해 `dynamic` keyword 제공

자료형을 명시하지 않아도 **자료형 추론**이 가능 !!

### Interoperable

Java와 100% 호환 가능! (Kotlin/JVM)

- 코틀린 컴파일러는 자바 중간코드(Java ByteCode) 생성
- 이미 존재하는 자바 라이브러리를 그대로 이용
- Java와 Kotlin을 섞어서 써도 됨

```java
public class Customer {
	public static final String LEVEL = "BASIC";

	public static void login() {
		System.out.println("Login");
	}
}
```

```kotlin
fun main() {
	println(Customer.LEVEL)
	Customer.login()
}
```

### Null Safety

NPE(Null Pointer Exception)를 방지할 수 있는 안정성

(NPE가 발생하면 프로그램이 중지한다 ! → 메모리조차 할당되지 않은 상태)

`?` keyword를 이용해서  Nullable Type과 Non-Null Type 자료형을 구분한다

```java
var a: String? = null
var b: String = "Hello"

b = null      // Error
```

아래의 코드는 Nullable Variable의 Property에 접근하기 때문에 컴파일이 되지 않는다

```java
var name: String? = null
var len = name.length        // 에러: null 가능성 있는 경우 length에 접근 불가
```

아래의 코드는 위와 거의 유사하지만 Safe Call을 사용하기 때문에 컴파일이 가능하다

```java
var name: String? = null
var len = name?.length        // name이 null이 아닐 경우에만 length에 접근 허용
```

가능하면 Non-Null Type을 사용하자 !

### Immutable

상태를 바꾸지 않는 불변성 제공

`val` (Value) = Immutable = Final variable = 할당 후 변경 불가

`var` (Variable) = Mutable = Non-Final variable = 언제든 변경 가능

```kotlin
val one: Int = 10     // Java: final int one = 10
one = 12     // Error

var two: Int = 2
two = 5      // OK
```

```kotlin
val mutableList = mutableListOf<Int>(1, 2, 3, 4)
// .add()나 .remove()를 사용해 요소 추가 삭제 가능

val immutableList = listOf<Int>
// .add()나 .remove()를 사용해 요소 추가 삭제 불가능
```

자바는 불변성을 제공하는 컬렉션이 없었지만 Java9에서 부터 제공한다

### Concise

자바처럼 장황하지 않고 깔끔한 코드를 통해 간결성을 제공. 보일러플레이트 코드를 최소화

```java
public class Address {
	private String city;
	private Country country;

	public Address(String city, Country country) {
		this.city = city;
		this.country = country;
	}

	public String getCity() {
		return city;
	}

	public void setCity(String city) {
		this.city = city;
	}

	// ...
}
```

```kotlin
data class Address(var city: String, var country: Country)
```

### Extension Functions

확장 함수 → 클래스 상속이나 디자인 패턴을 사용하지 않고도 새로운 기능 확장 가능

단, 너무 많이 사용하면 기능이 남용 되므로 가독성이 떨어진다

```kotlin
class Original {
	fun onlyOneFunction() {
		println("Only One")
	}
}

fun Original.myExtension(): String {
	return "Original 클래스에 마치 멤버 메서드가 추가된 느낌"
}

fun main() {
	val originalObj = Original()
	var result = originalObj.myExtension()
	println(result)
}
```

```kotlin
fun String.lastChar(): Char = this.get(this.length-1)
// Receiver type          Receiver Object

println("12345".lastChar())
```

### Functional Programming

함수의 유기적 연결을 통한 프로그래밍 방식을 제공한다

함수가 일급 객체(First-class citizens)으로 사용할 수 있게 된다

람다식을 통해 선언되지 않고도 익명 함수기능을 식에 전달할 수 있다 !

```kotlin
fun add(a: Int, b: Int) = a + b

fun subtract(a: Int, b: Int) = a - b

fun main() {
	val functions = mutableListOf(::add, ::subtract)

	println(functions[0])
	// fun add(kotlin.Int, kotlin.Int): kotlin.Int

	println(functions[0](12, 30))
	// 12+30 = 42

	println(functions[1](57, 15))
	// 57-15 = 42
}
```

일급 객체는 함수의 인자, 반환값, 심지어 자료구조에도 넣을 수 있다 !!

```kotlin
fun calculator(a: Int, b: Int, sum: (Int, Int) -> Int): Int {
	return a + b + sum(a, b)
}

fun main() {
	val out = calculator(11, 10 , {a, b -> a + b})
	//        calculator(11, 10) {a, b -> a + b}의 문법과 동일
	
	println(out)
  //  11 + 10 + (11 + 10) = 42
}
```

### Multiplatform

![Untitled](https://user-images.githubusercontent.com/30489264/145199133-0bc2bba8-6828-4334-832f-deba2e1adddc.png)

Kotlin Multiplatform = (Kotlin/JVM + Kotlin/Native + Kotlin/JS)

사용 가능한 플랫폼

- Kotlin/JVM - 자바 가상 머신 상에서 동작하는 앱을 만들 수 있다
- Kotlin/JS - 자바스크립트에 의해 브라우저에서 동작하는 앱을 만들 수 있다
- Kotlin/Native - LLVM기반의 네이티브 컴파일을 지원해 여러 타입의 앱을 만들 수 있다
    - Kotlin/Native에서의 타깃 → iOS, MacOS, Android, Windows, Linux, WebAssembly
    

## Kotlin in Spring

Spring Framework는 Kotlin에 대해 전폭적으로 지원하며, 참조 문서의 대부분 코드 샘플을 Java 이외에도 Kotlin으로 제공한다.

### Requirements

- Spring 1.3버전 이상
- `kotlin-stdlib` and `kotlin-reflect`
- Jackson Kotlin module (serializing/deserializing JSON data for Kotlin Class)

### Class & Interface

기본 생성자를 통한 Kotlin 클래스 인스턴스화, Immutable 클래스 데이터 바인딩, default 값이 있는 함수의 선택적 매개변수와 같은 다양한 Kotlin 구성을 지원한다

### Annotation

Null-safety를 사용해서 `required` attribute를 명시하지 않고도 HTTP 매개변수가 필요한지 여부를 결정할 수 있다

- `@RequestParam name: String?` : not required
- `@RequestParam name: String` : required

마찬가지로 `@Autowired`, `@Bean`, `@Inject` 를 이용한 빈 주입 시에도 해당 빈의 필수 여부를 결정할 수 있다

- `@Autowired lateinit var thing: Thing` : `Thing` 유형의 Bean이 ApplicationContext에 등록되어 있어야 함
- `@Autowired lateinit var thing: Thing?` : 위와 같은 Bean이 존재하지 않아도 오류가 발생하지 않음
- `@Bean fun play(toy: Toy, car: Car?) = Baz(toy, Car)` : `Toy` 의 Bean은 ApplicationContext에 반드시 등록되어 있어야 하고, Car는 있을수도 있고 없을수도 있다.

### Bean Definition DSL

👉 [참고 링크](https://docs.spring.io/spring-framework/docs/current/reference/html/languages.html#kotlin-bean-definition-dsl)

XML 또는 Java Configuration(@Configuration, @Bean)의 대안으로 람다식을 사용해서 함수적 프로그래밍 방식으로 Bean을 등록할 수 있도록 한다. 간단히 말해서 FactoryBean 역할을 하는 람다식에 빈을 등록할 수 있다는 것을 의미한다(?)

### Coroutines

👉 [참고 링크](https://kotlinlang.org/docs/reference/coroutines/coroutines-guide.html)

코루틴은 비동기적으로 실행되는 코드를 간소화 하기 위한 경량 쓰레드 라이브러리다.

- 경량
- 메모리 누수 감소
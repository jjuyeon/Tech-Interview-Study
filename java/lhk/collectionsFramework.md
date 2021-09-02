## Java의 데이터 타입
### primitive 데이터 타입
- 정수타입 : byte, short, int, long
- 소수타입 : float, double
- bool 타입 : boolean
- 문자타입 : char 

각 primitive 별 참조타입이 존재한다.

### Wrapper 타입
- 정수타입 : Byte, Short, Integer, Long
- 소수타입 : Float, Double
- bool 타입 : Boolean
- 문자타입 : Character

## 오토박싱과 언박싱 개념 등장
Collection 자료구조가 추가되면서 preimitive 타입을 Collection에 담을 수 없어 내부로 감싼 Wrapper 타입을 활용하게 되었다.    
JDK1.5 이전에는 기본형과 참조형 간의 연산이 불가능해서 기본형을 래퍼 클래스 객체로 만들어서 연산했다.  
컴파일러가 자동으로 코드를 변환해주어 기본형과 참조형 간에 연산이 가능해졌다.    


```Java
// 컴파일 전
int i = 100;
Integer obj = new Integer(500);

int sum = i + obj;
```

```Java
// 컴파일 후
int i = 100;
Integer obj = new Integer(500);

int sum = i + obj.intValue();
```

## 오토박싱(autoboxing)
기본형 값을 래퍼 클래스 객체로 자동 변환해주는 것

## 언박싱(unboxing)
래퍼 클래스 객체를 기본형 값으로 자동 변환해주는 것

```Java
ArrayList<Integer> list = new ArrayList<Integer>();
list.add(100); // 100 -> new Integer(100) [autoboxing]

int value = list.get(0); // Integer(100) -> 100 [unboxing]
```

## 오토박싱과 언박싱의 단점
컴파일러가 자동으로 지원해주어서 편리하지만, 다른 타입간의 형변환은 성능에 영향을 끼치므로 지양하는 것이 좋다.


# Collections Framework
데이터 군을 다르고 표현하기 위한 단일화된 구조 (Java API 문서)

## List
### 특징
- 순서가 있다.
- 중복을 허용한다.

### 구현 클래스
- ArrayList 
  - 기존의 Vector를 개선한 것으로, Vector보다 사용을 권장한다.
  - 원하는 데이터에 접근하는데 빠르다.
  - 데이터 추가/삭제 시 이후 요소들도 한 칸씩 이동해줘야해서 데이터 개수가 많을수록 시간이 오래걸린다. 
  - 크기를 변경할 수 없다.
- LinkedList  
  - 데이터 추가/삭제가 빠르다. 
  - 데이터에 접근하는 데 느리다.
  - 데이터가 많을수록 접근성이 떨어진다. 
- Stack
  - Vector를 상속받아 구현 
  - 수식 계산, 괄호검사, 웹브라우저 뒤로가기/앞으로가기
- Vector


## Set
### 특징
- 순서를 유지하지 않는다.
- 중복을 허용하지 않는다.

### 구현 클래스
- HashSet
  - 가장 대표적인 클래스
  - 중복된 용소를 추가하려고하면 false를 반환하여 실패를 알린다.
- TreeSet
  - 이진 검색 트리 자료구조의 형태로 데이터를 저장한다.
  - 이진 검색 트리의 성능을 향상시킨 레드블랙트리로 구현되어 있다.


## Map
### 특징
- Key와 Value의 쌍으로 이루어진 집합
- 순서를 유지하지 않는다.
- Key는 중복을 허용하지 않고, Value는 중복을 허용한다.

### 구현 클래스
- HashMap
  - Hashtable보다 새로운 버전인 HashMap 사용 권장 (속도 향상)
  - 많은 양의 데이터를 검색하는데 뛰어난 성능을 가진다. 
- TreeMap
  - 정렬과 검색에 적합
- Hashtable
  - 동기화를 하기때문에 속도가 느리다. 
- Properties
  - Hashtable을 상속받아서 구현 


## 람다식
Java 8에서 새로 추가된 것 중 하나이다. 익명 함수라고도 하며, 메서드를 하나의 식으로 표현한 것을 말한다.       
모든 메서드는 클래스에 포함되어야 하므로 클래스를 만들어 객체를 생성해야 메서드를 호출할 수 있다. 
하지만 람다식은 그 자체로서 메서드 역할을 수행할 수 있어서 변수처럼 다루는 것이 가능하다.

## 람다식 작성하기

1. 메서드에서 이름과 반환타입을 제거하고 매개변수 선언부와 몸통 사이에 `->` 를 추가한다.
```Java
// 기존 메서드
int max(int a, int b) {
  return a > b ? a : b;
}

// 람다식 표현
(int a, int b) -> { return a > b ? a : b; }
```

2. 반환값이 있거나 반환값이 없어도 괄호 안의 문장이 하나이면 괄호를 생략하고 식으로 표현할 수 있다.

```Java
(int a, int b) -> a > b ? a : b
```

3. 매개변수 타입이 추론 가능하면 생략가능하다. 대부분 생략 가능하다.
```Java
(a, b) -> a > b ? a : b
```

```Java
// 기존 메서드
int square(int x) {
  return x * x;
}

// 1. 이름과 반환 타입을 제거하고 식으로 표현한다.
(int x) -> x * x

// 2. 타입을 생략한다.
(x) -> x * x

// 3. 매개변수가 하나이면 괄호를 생략한다.
x -> x * x
```

## 함수형 인터페이스
람다식으로 정의된 익명 객체를 호출하기 위해서는 참조 변수가 필요하다. 이때, 참조 변수의 타입으로 람다식과 동등한 메서드가 정의된 인터페이스를 사용할 수 있다.
즉, 람다식을 다루기 위한 인터페이스를 말한다.      
람다식과 인터페이스의 메서드가 1:1로 연결되기 위해, 오직 하나의 추상 메서드만 정의되어 있어야 한다는 제약이 있다.    
```Java
MyFunction f = (int a, int b) -> a > b ? a : b;
int bif = f.max(5, 3) // 호출

@FunctionalInterface
interface MyFunction {
  public abstract int max(int a, int b);
}
```

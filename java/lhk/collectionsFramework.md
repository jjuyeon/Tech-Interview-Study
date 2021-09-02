## 오토박싱과 언박싱 개념 등장
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


# Collections Framework
데이터 군을 다르고 표현하기 위한 단일화된 구조 (Java API 문서)

## List
### 특징
- 순서가 있다.
- 중복을 허용한다.

### 구현 클래스
- ArrayList
- LinkedList
- Stack
- Vector


## Set
### 특징
- 순서를 유지하지 않는다.
- 중복을 허용하지 않는다.

### 구현 클래스
- HashSet
- TreeSet


## Map
### 특징
- Key와 Value의 쌍으로 이루어진 집합
- 순서를 유지하지 않는다.
- Key는 중복을 허용하지 않고, Value는 중복을 허용한다.

### 구현 클래스
- HashMap
- TreeMap
- Hashtable
- Properties


## 람다식
익명 함수라고도 하며, 메서드를 하나의 식으로 표현한 것  
모든 메서드는 클래스에 포함되어야 하므로 클래스를 만들어 객체를 생성해야 메서드를 호출할 수 있다. 
하지만 람다식은 그 자체로서 메서드 역할을 수행할 수 있어서 변수처럼 다루는 것이 가능하다.

## 람다식 작성하기

1. 메서드에서 이름과 반환타입을 제거하고 매개변수 선언부와 몸통 사이에 `->` 를 추가한다.
```
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


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

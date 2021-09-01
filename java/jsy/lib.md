# :question: JAVA 의 라이브러리

#### reference
https://snd-snd.tistory.com/18<br>
https://milkoon1.tistory.com/44
<hr>

## Question
1. [자바 컬렉션에 대해서 설명해주세요.](#1-collection-framework)
- 자바에서 컬렉션이란 객체나 데이터를 효율적으로 관리할 수 있는 방법을 모아둔 라이브러리입니다. 컬렉션 프레임워크는 크게 두개로 나눌 수 있는데, 순서나 집합적인 저장 공간의 명세를 나타내는 Collection 인터페이스와 key와 value로 데이터를 핸들링하는 명세를 정의하는 Map 인터페이스로 나눌 수 있습니다.
<br><br>

2. [컬렉션 프레임워크에 속한 List, Set, Map에 대해 설명해주세요.](#1-3-list-vs-set-vs-map)
- 3개의 인터페이스 모두 하나 이상의 element를 저장한다는 공통점이 있습니다.
- List는 순서가 존재하고 중복이 허용됩니다. Set은 집합적인 개념을 가지며, 순서가 없고 Element의 중복은 허용되지 않습니다. Map은 (key,value) 쌍으로 이루어져 있으며 순서가 없으며 key값은 중복될 수 없습니다.
<br><br>

3. [정규 표현식에 대해 설명해주세요.]()

<hr>

## :nerd_face:	What I study
### 1. Collection Framework
![collection](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Fc3GrCd%2FbtqxXaEtic3%2FKgMYKbNC4JUmwkLUyswAN0%2Fimg.png)
- 객체나 데이터를 효율적으로 관리(추가, 삭제, 검색)할 수 있는 표준화된 방법을 모아둔 클래스의 집합/라이브러리
- 컬렉션을 쉽고 편리하게 다룰 수 있는 다양한 인터페이스와 클래스를 제공한다.
- 배열의 단점을 보완해준다.
- ```java.util``` 패키지에 포함되어 있다.
- Map과 Collection 인터페이스가 존재한다.
  - 사용목적에 따라 Map, List, Set 각각의 하위 구현체를 선택한다.

![collection2](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FSkdDj%2FbtqC3reXSMS%2FK1i0B8yqXCGxRP9Frhps20%2Fimg.jpg)
### 1-1) Collection
#### List
- 순서가 있는 Collection
- 데이터를 중복해서 포함할 수 있다.
- 순차적으로 대량의 데이터를 접근하거나 입력할 때 사용된다.

<br>
- LinkedList와 ArrayList의 차이점

||LinkedList|ArrayList|
|:---:|:---:|:---:|
|데이터 관리|특정 데이터 타입의 배열|노드에 데이터를 저장하고 앞 뒤 노드의 주소값을 연결|
|삽입/삭제|O(1)|O(n)|
|검색|O(n)|O(1)|

<br>
- Vector와 ArrayList의 차이점

||Vector|ArrayList|
|:---:|:---:|:---:|
|동기화 처리|O|X|
|스레드 접근|한 번에 하나의 스레드|한 번에 여러 개 스레드|
|속도|느림|빠름 (멀티스레드 상황 제외)|

#### Set
- 집합적인 개념의 Collection
- 순서가 없다.
- 데이터를 중복할 수 없다.
- 순차적인 접근을 위해 Iterator를 사용한다.

<br>

### 1-2) Map
- 검색할 수 있는 인터페이스
- 순서가 없다.
- 많은 양의 데이터에서 원하는 특정 데이터에 접근할 때 사용한다.
- 데이터를 삽입할 때, (key-value) 쌍으로 삽입된다.
- 검색 시에는, key를 이용해서 value를 얻을 수 있다.
- 동일한 데이터를 key로 사용할 수 없다.

<br>
- HashMap과 HashTable의 차이점

- key, value로 데이터를 관리하고 key를 이용하여 데이터를 검색한다.

||HashMap|HashTable|
|:---:|:---:|:---:|
|동기화 여부|X|O|
|멀티 스레드 세이프|X|O|
|속도|빠름|느림 (동기화 처리 비용 때문)|
<br>

TreeMap과 TreeSet의 차이점
  - 큰 차이점은 Map과 Set의 차이이다.
  - 둘다 Red Black Tree를 기반으로 이루어져 있다.
  - cf) Red Black Tree: balanced binary search tree, BST에서 발생하는 불균형 문제를 색깔을 통해 자체적으로 해결한다.

<br>

### 1-3) List vs Set vs Map

||List|Set|Map|
|:---:|:---:|:---:|:---:|
|특징|순서 O, 중복 O|순서X, 중복X|순서X, key에 대한 중복X|
|장점|가변적인 자료구조 사이즈|빠른 속도|빠른 속도|
|단점|원하는 데이터가 뒤쪽에 위치하면 속도가 느림|정렬하려면 별도의 처리가 필요|key의 검색 속도가 검색 속도를 좌우함|

<br><br>
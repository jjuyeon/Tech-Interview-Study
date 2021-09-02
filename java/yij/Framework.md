> [💡#](#collection) 컬렉션 프레임워크에 속한 List, Set, Map에 대해 설명해주세요.
> - List, Set, Map 모두 지정된 타입의 여러 Element를 저장할 수 있는 Collection Framework입니다. 
> - List는 ArrayList, LinkedList로 구현될 수 있으며 각각 배열, 링크드리스트의 특성을 갖습니다. 또한 순서가 존재하고 중복이 허용됩니다.
> - Set은 구현체에 따라 순서 여부는 달라질 수 있지만 Element의 중복은 허용되지 않습니다.
> - Map은 <Key, Value>로 이루어져 있으며 구현체에 따라 순서 여부는 달라질 수 있지만 Key값은 중복될 수 없습니다.

# Framework

## Collection

![image](https://user-images.githubusercontent.com/30489264/131596123-e23e98de-793c-4a8a-9402-5a90d84cb977.png)

자바의 대표적인 Collection Framework로는 **List, Set, Map**이 있다.
세 자료구조 모두 하나 이상의 Element를 저장한다는 공통점이 있지만, 세부적인 특징이 다르다.

<br>

### List

저장된 요소들 사이에 순서가 존재하고, Element의 중복이 허용된다.
List Interface의 구현체로는 <code>ArrayList</code>, <code>LinkedList</code>, <code>Vector</code>가 있다.

#### ArrayList

- Array(배열)을 이용한 리스트
- Default Size는 10으로 선언되어 있고, List의 Elements가 추가될 때 마다 새로운 배열을 선언한 뒤 옮겨 담는다.
- 구현체가 배열이라는 특성 때문에 조회는 O(1)에 가능하다
- 하지만 Element의 개수가 증가하여 Capacity에 가까워지면 Resizing이 발생하고, Array의 특성을 따르기 때문에 삽입 및 삭제 시간이 느리다

#### LinkedList

- LinkedList로 구현한 리스트
- 각각의 Element는 Node< E > 클래스로 이루어져 있으며, next, prev를 가진 양방향 연결 리스트 구조이다.
- ArrayList와는 반대로, 조회보다는 삽입 및 삭제가 자주 발생할 때 사용하는 것이 좋다.

<br>

### Set

: (일반적으로)저장된 요소들 사이에 순서가 없고, 중복을 허용하지 않는다.
Set Interface의 구현체는 <code>HashSet</code>, <code>LinkedHashSet</code>, <code>TreeSet</code>이 있다. 세 가지 구현체 모두 synchronized하지 않기 때문에 thread-safe하지 않다.

#### HashSet

- 해시 테이블(실제로는 HashMap 인스턴스)에 의해 지원되는 Set 구현 클래스
- **중복 불가, 순서 없음**
- 대표적으로 가장 많이 사용하는 Set 구현체

#### LinkedHashSet

- HashSet을 extend한 Set 구현 클래스
- **중복 불가, 순서 있음**
- 들어온 순서에 따라 양방향 연결 리스트 형태를 가진다
- 삽입한 순서대로 순서를 가짐

#### TreeSet

- NavigableSet을 implements한 Set 구현 클래스
- **중복 불가, 순서 있음**
- **비교 가능한 순서에 의해 정렬된다 !!** 

<br>

### Map

Key, Value 쌍으로 저장되는 자료구조이다. Set과 같이 일반적으로 순서를 가지고 있지 않다. Key 값은 중복될 수 없지만(똑같은 Key를 가진 두 개 이상의 Element가 존재할 수 없다) Value 값은 중복될 수 없다.  
 주로 <code>HashMap</code>, <code>HashTable</code> 두 개의 구현체를 사용한다.

#### HashMap

- 보조 해시 함수를 지원하여 Collision을 감소시킨다
- 지속적으로 성능 개선을 하고 있다
- Thread-safe하지 않다

#### HashTable

- Thread-safe하다
- not fail-fast를 제공한다(?)
- JRE 1.0, 1.1 환경 대상으로의 하위 호환성을 제공한다(기능 발전은 X)

#### LinkedHashMap

- HashMap과 동일하지만 insert 된 순서를 유지한다

#### TreeMap

- HashMap과 동일하지만 비교 가능한 순서에 의해 정렬된다

<br>

### List 🆚 Set 🆚 Map

(가장 많이 사용하는 구현체 기준)

||List|Set|Map|
|-|-|-|-|
|대표 구현 Class|**ArrayList**<br>LinkedList|HashSet|HashMap|
|순서 여부|O|X|X|
|중복 허용|O|X|X(Key 중복 불가)|

<br>

## 참고 자료

> https://techvidvan.com/tutorials/java-collection-framework/
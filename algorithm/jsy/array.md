# :question: Array, ArrayList, LinkedList

#### reference
https://girawhale.tistory.com/8
<hr>

## Question
1. Array와 LinkedList의 차이가 무엇인가요?
- 탐색방식과 저장방식에 차이가 있습니다.
- Array는 데이터를 인덱스를 통해 특정 요소에 직접 접근하는 방식을 사용합니다. 반면 LinkedList는 어떤 데이터를 접근할 때 순차적으로 검색하는 방식을 사용합니다.
- 저장방식도 Array에서 요소들은 인접한 메모리 위치에 연속적으로 저장됩니다. 반면 LinkedList에서 요소들은 비연속적으로 저장되며, 새로운 노드에 할당된 메모리 위치 주소는 이전 노드에 저장됩니다.
<hr>

## :nerd_face:	What I study
### 1. Array
![array](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FblRy0C%2FbtqP4msw6DQ%2FDvajT3DX0P2N9n0s9mUAok%2Fimg.png)
- 메모리 공간을 연속적으로 사용하는 자료구조
- 초기화시 설정한 배열의 크기를 이후에 변경할 수 없다.
- 메모리에 연속적으로 값을 저장한다.
- 검색 시, 인덱스를 통한 검색을 하므로 성능이 좋다. (**O(1)**)
- 삽입/삭제 시, 다른 인덱스들의 위치를 shift 해야하므로 성능이 좋지 않다. (**O(N)**)
- 선언 시, 컴파일 타임에 할당 된다. (정적 메모리 할당)
- Stack 영역에 메모리 할당이 이루어진다.

<br><br>

### 2. LinkedList
![linkedList](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FbEtrjD%2FbtqPYSsnKD1%2FKT2vr8zKtpgX5LqQDDa1T0%2Fimg.png)
- 일렬로 연결된 데이터를 저장할 때 사용하는 자료구조
- 이중 연결 리스트의 형태를 가지고 있다.
- 초기화시 설정된 리스트의 크기를 이후에 변경할 수 있다.
- 포인터를 통해 다음 데이터의 위치를 가리키고 있다.
- 불연속적인 메모리 공간에 데이터를 저장하기 때문에, 검색 성능이 좋지 않다.
- 검색 시, 시작이나 끝에서부터 원하는 인덱스에 도달할 때까지 **순차적으로** 저장된 다음 주소를 탐색하므로 성능이 좋지 않다. (**O(N)**)
- 삽입/삭제 시,  
  - 시작이나 끝 요소에 접근할 때, **O(1)**
    - LinkedList는 head(시작)와 tail(끝)을 갖는 이중 연결 리스트의 구조이다.
  - 그 외 요소에 접근할 때,  **O(N)**
    - 원하는 인덱스에 도달할 때까지 순차적인 접근을 한다. - O(N)
    - `index-1`의 노드의 next와 `index+1`의 prev를 삽입/삭제할 노드 정보로 변경하는 시간은 상수시간이다. - O(1)
- 새로운 요소가 추가될 때, 런타임에 메모리를 할당한다. (동적 메모리 할당)
- Heap 영역에 메모리 할당이 이루어진다.

<br><br>

### 3. ArrayList
- Array를 사용해 만든 List형 자료구조
- 내부적으로 데이터를 배열의 형태로 관리한다.
- 초기화시 설정된 배열의 크기를 이후에 변경할 수 있다.
- 검색 시, 인덱스 기반 탐색을 하므로 성능이 좋다. (**O(1)**)
- 삽입/삭제 시, 임시 배열을 생성해 데이터를 복사하는 방법을 사용한다. 이는 새로운 메모리를 할당하는 방법으로, 성능이 좋지 않다. (**O(N)**)
  - 삽입: 기존에 있던 배열에서, 추가하고 싶은 index부터 마지막 index까지 한 칸씩 뒤로 미루는 연산을 한다.
  - 삭제: 삭제된 index + 1부터 마지막 index까지 한 칸씩 앞으로 당기는 연산을 한다.

<br><br>

### 4. 정리
||Array|ArrayList|LinkedList
|:---:|:---:|:---:|:---:|
|get/set|O(1)|O(1)|O(N)|
|add(시작)|O(N)|O(N)|O(1)|
|add(끝)|O(N)|O(1)|O(1)|
|add(일반)|O(N)|O(N)|O(N)|
|remove(시작)|O(N)|O(N)|O(1)|
|remove(끝)|O(N)|O(1)|O(1)|
|remove(일반)|O(N)|O(N)|O(N)|

<br>

- 일반적으로 검색, 수정을 자주 사용한다면, `Array , ArrayList`
  - 데이터의 크기가 고정적일 때, `Array`
  - 데이터의 크기가 가변적일 때, `ArrayList`
- 잦은 삽입, 삭제가 발생한다면, `LinkedList`
- 공간 복잡도의 경우, 종종 ArrayList가 LinkedList보다 빠른 경우가 발생한다.
  - ArrayList는 연속된 메모리 공간 안에 데이터가 저장되기 때문이다.
  - LinkedList는 요소마다 두 개의 참조 노드가 필요하고, 메모리 공간에 비연속적으로 노드가 흩어져 있을 수 있다.
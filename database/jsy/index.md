# :question: 인덱스 (Index)

#### reference
https://helloinyong.tistory.com/296<br>
https://zorba91.tistory.com/293
<hr>

## Question
1. [DB 인덱스에 대해 설명해주세요.](#1-index)
   - DB의 데이터를 논리적으로 정렬하여 검색과 정렬 작업의 속도를 높이기 위해 사용되는 방법입니다.
   - 책의 목차, 색인과 같다고 할 수 있습니다.
<br><br/>

2. DB 인덱스를 사용하는 이유는 무엇인가요?
   - ***테이블을 조회하는 속도와 그에 따른 성능을 향상***시킬 수 있으며, 그에 따른 ***전반적인 시스템 부화를 줄일 수*** 있습니다.
<br><br/>

3. [DB 인덱스에 해쉬 보다 B Tree(B- Tree)를 쓰는 이유는 무엇인가요?](#2-hash-table)
   - 해시 테이블은 단 하나의 데이터를 탐색하는, 등호(=) 연산에만 특화되어 있습니다.
   - DB는 기준 값보다 크거나 작은 요소들을 탐색하는, 부등호(<,>) 연산이 자주 발생될 수 있으므로 DB 인덱스 용도로 해시 테이블은 어울리지 않습니다.

<hr/>

## :nerd_face:	What I study
### 1. Index
- DB Index에 대한 설명
  - 기본키는 항상 DBMS가 내부적으로 정렬된 목록을 관리한다. -> 기본키에 대해서 특정 행을 가져올 때 빠르게 처리됨.
  - 기본키 외의 다른 열의 내용을 검색하거나 정렬할 때는 하나하나 대조해보기 때문에 시간이 오래걸린다. -> 이를 인덱스로 정의해두면 검색 속도가 향상됨!
  - 즉, 데이터와 데이터의 위치를 포함한 자료구조인 인덱스를 생성하여 빠르게 조회한다!
- 장점
  - 테이블 조회 속도, 성능 향상
  - 전반적인 시스템 부화 감소
- 단점
  - 데이터 변경 작업이 자주 일어나는 경우, Index를 재작성해야 한다. (성능에 영향을 미침)
  - 인덱스를 관리하기 위해 DB에 추가 저장공간이 필요하다.
- 사용하면 좋은 경우
  - where, join문에 자주 사용되는 컬럼
  - DML(insert, update, delete)가 자주 발생하지 않는 컬럼
  - 데이터의 중복도가 낮은 컬럼
  - 규모가 작지 않은 테이블
<br><br/>

### 2. Hash Table
![hash](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FcSdWcV%2FbtqLDOnT21u%2FcYdqhhPUIm1KgBIt3hWVt0%2Fimg.png)
- 해시 테이블은 해시 함수를 통해 나온 해시 값을 이용하여 저장된 메모리 공간에 한 번에 접근한다.
- O(1) (왜? 단 하나의 데이터를 탐색하는 시간)
- 모든 값이 정렬되어 있지 않으므로, 해시 테이블에서는 특정 기준보다 크거나 작은 값을 찾을 수 없다. (부등호 연산에 매우 비효율적)
- 즉, 해시 테이블은 단 하나의 데이터를 탐색하는, 등호 연산에 매우 효율적이다.
<br><br/>

### 3. B Tree
#### 1) B- Tree
![b-](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FwUaAz%2FbtqLFTnLNpN%2F8gJlCP70LaTNjksl84xolk%2Fimg.jpg)
- DB 인덱스에서 사용되는 자료구조
- ***데이터가 정렬된 상태로 유지되는 것***이 핵심이다.
- B- tree는 루트로부터 리프까지의 거리가 일정한 **균형트리**이다. 
![complexity](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FpQB0I%2FbtqBQ05iyzt%2Ff8oPM7x0blzKhEzZMwXDck%2Fimg.png)
- 데이터 갱신이 자주 발생하면 서서히 균형이 깨지고, 성능에도 나쁜 영향을 미치게 된다.
- 데이터 갱신이 자주 발생하는 테이블의 인덱스는 인덱스 재구성을 통해 트리의 균형을 되찾는 작업이 필요하다.
#### 2) B+ Tree
![b+](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FbRiL19%2FbtqBTMSBCWF%2FJ3nKw2qympUVxGThnVdLK0%2Fimg.png)
- B- tree의 확장개념
- ex) InnoDB (MYSQL DB engine)
- 리프 노드에만 key, data를 저장한다. -> 사용되는 메모리 양을 줄일 수 있다.
- 하나의 노드에 더 많은 key들을 담을 수 있으므로, 트리의 높이가 낮아진다.
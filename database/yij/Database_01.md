# 1) Database 
- contributor : [김경원](https://github.com/shining8543) , [윤이진](https://github.com/483759) , [이현경](https://github.com/honggoii) , [진수연](https://github.com/jjuyeon)
<hr/>

### :notebook_with_decorative_cover: 인덱스 (Index)
1. DB 인덱스에 대해 설명해주세요.
   - 데이터베이스에서 풀 스캔이 아닌 레인지 스캔을 하기 위해 지정하는 자료구조.
2. DB 인덱스를 사용하는 이유는 무엇인가요?
   -  테이블을 조회할 시 SQL문의 수정 없이 빠른 속도로 조회하기 접근하기 위해
3. DB 인덱스에 해쉬 보다 B Tree를 쓰는 이유는 무엇인가요?
    - B+트리를 사용하게 되면 banlanced 성질이 유지되어 데이터의 속성에 관계 없이 일관된 시간복잡도 내에 조회 가능. 해쉬는 해쉬 버킷의 일부 영역에만 데이터가 집중되는 경향이 있어 cache miss가 발생할 확률이 높음.

### :notebook_with_decorative_cover: 관계형 DB vs 비관계형 DB
1. 관계형 DB 와 비관계형 DB 의 차이점에 대해 설명해주세요.
   - 관계형 DB는 1️⃣정해진 데이터 스키마에 따라 테이블에 저장되며 2️⃣관계를 통해 여러 테이블에 분산된다는 특징이 있다. 따라서 스키마를 준수하지 않은 레코드는 테이블에 추가할 수 없다. 비관계형 DB는 그에 반해 스키마도 없고, 관계도 없다. 레코드가 아닌 문서(documents)라고 부르며, json과 비슷한 형태의 관련된 데이터들을 동일한 컬렉션에 저장한다.
2. RDBMS과 비교하였을 때 NoSQL의 장점을 설명해보세요.
    - 스키마가 없어서 데이터베이스 구조가 유연하다. 언제든지 저장된 데이터를 조정하고 새로운 필드를 추가할 수 있다. 
    - 어플리케이션이 필요로 하는 형식으로 데이터를 저장하기 때문에 읽어오는 속도가 빨라진다.
    - 수직 및 수평적 확장이 가능해서 어플리케이션이 발생시키는 모든 읽기/쓰기 요청을 처리할 수 있다.
4. 어떤상황에서 NoSQL을 쓰는 것이 더 적합한가?
    - 정확한 데이터 구조를 알 수 없거나, 변경 및 확장 가능성이 있는 경우
    - 읽기를 자주 하지만 데이터 변경은 자주 없는 경우
    - 데이터베이스를 수평으로 확장해야 하는 경우

### :notebook_with_decorative_cover: 트랜잭션 (Transaction)
#### 트랜잭션이란?
------
데이터베이스 관리 시스템에서 하나의 논리적 작업 단위를 구성하는 일련의 연산들의 집합이다.
> ex) 계좌 간의 자금 이체
> 한 계좌에서 다른 계좌로 10만원을 이체하는 작업은 한 계좌에서 10만원 인출 + 다른 계좌로 10만원 입금의 두 작업으로 구성된다

하지만 전체 작업이 정상적으로 처리될 수 없는 경우 아무 것도 실행되지 않은 처음 상태로 복구되어야 한다



#### 트랜잭션의 성질 ACID
* Actomicity (원자성): DBMS는 완료되지 않은 트랜잭션의 중간 상태를 데이터베이스에 반영해서는 안 된다
* Consistency (일관성): 고립된 트랜잭션의 수행이 데이터베이스의 일관성을 보존해야 한다. 즉, 성공적으로 수행된 트랜잭션은 정당한 데이터들만을 데이터베이스에 반영해야 한다.
* Isolation (독립성): 여러 트랜잭션이 동시에 수행되더라도 각각의 트랜잭션은 다른 트랜잭션의 수행에 영향을 받지 않고 독립적으로 수행되어야 한다. = 한 트랜잭션의 중간 결과는 다른 트랜잭션에게 숨겨져야 한다
* Durability (지속성): 트랜잭션이 성공적으로 완료되어 커밋되고 나면, 해당 트랜잭션에 의한 모든 변경은 향후에 어떤 소프트웨어나 하드웨어 장애가 발생되더라도 보존되어야 한다.
<br/>

#### 트랜잭션을 병행으로 처리할 때 발생할 수 있는 문제점과 이를 방지하기 위한 방법
----
##### 발생 가능한 문제점
- 갱신 분실(Lost Update) : 두 개 이상의 트랜잭션이 같은 자료를 공유하여 갱신할 때 갱신 결과의 일부가 없어지는 현상

- 비완료 의존성(Uncommitted Dependency) : 하나의 트랜잭션 수행이 실패한 후 회복되기 전에 다른 트랜잭션이 실패한 갱신 결과를 참조하는 현상. 임시 갱신이라고도 한다.

- 모순성(Inconsistency) : 두 개의 트랜잭션이 병행수행될 때 원치 않는 자료를 이용함으로써 발생하는 문제. 불일치 분석이라고도 한다.

- 연쇄 복귀(Cascading Rollback) : 병행수행되던 트랜잭션들 중 어느 하나에 문제가 생겨 Rollback하는 경우 다른 트랜잭션도 함께 Rollback되는 현상

##### 이를 해결하기 위한 방법
: Locking(한 트랜잭션이 DB 상태를 점유하는 동안 다른 트랜잭션이 접근할 수 없도록 막는 것)을 이용해 데이터 액세스를 상호 배타적으로 처리한다.

##### Locking 제어 기법을 사용할 때 Locking 단위를 크게/작게 했을 때의 차이점
Locking을 통해 무조건적으로 다른 커넥션을 막는다면, 트랜잭션의 수행이 serialize되고 데이터베이스의 성능이 저하될 수 있다.

반대로, 성능을 높이기 위해 Locking의 범위를 줄이면, isolation 특성이 보장받지 못해 잘못된 값을 처리하게 될 확률이 생긴다.

##### Locking 제어가 일으킬 수 있는 문제점
Isolation level이 너무 높아지면 트랜잭션이 직렬화될 수 있다.
<br>

#### 트랜잭션에 의해 발생할 수 있는 데드락에 대해 설명
-----
두 개 이상의 트랜잭션이 특정 자원(테이블 또는 레코드)의 잠금(Lock)을 획득한 채, 다른 트랜잭션이 소유하고 있는 잠금을 요구하는 상태. 아무리 기다려도 소유권을 받을 수 없으며 복수의 트랜잭션을 사용하다 보면 데드락이 발생할 수 있다. 

Transaction1이 Table B의 첫 번째 레코드의 lock을 얻고, Transaction2도 Table A의 첫 번째 레코드의 lock을 얻었다고 하자

    Transaction 1> create table B (i1 int not null primary key) engine = innodb;
    Transaction 2> create table A (i1 int not null primary key) engine = innodb;

    Transaction 1> start transaction; insert into B values(1);
    Transaction 2> start transaction; insert into A values(1);
이 때, 트랜잭션을 commit 하지 않은 채 서로의 첫 번째 레코드에 대한 lock을 요청하면 데드락이 발생한다.

    Transaction 1> insert into A values(1);
    Transaction 2> insert into B values(1);
    ERROR 1213 (40001): Deadlock found when trying to get lock; try restarting transaction



##### 데드락을 방지할 수 있는 방법은?
1. 트랜잭션을 자주 커밋한다
2. 정해진 순서로 테이블에 접근한다
   * (위의 예시에서는)Transaction1이 테이블 B ➡ A의 순으로 접근하고, Transaction2는 테이블 A ➡ B의 순으로 접근했기 때문에 데드락이 발생했다. 트랜잭션들이 동일한 테이블 순서에 의해 접근하도록 한다
3. 읽기 Lock 획득(SELECT ~ FOR UPDATE)의 사용을 피한다
4. 한 테이블의 복수 레코드를 복수 커넥션에서 순서 없이 갱신하면 교착상태가 발생하기 쉽다
   * 테이블 단위의 Lock을 획득해 갱신을 직렬화하면 동시성은 떨어지지만 데드락을 회피할 수 있다

#### 트랜잭션 격리 수준의 각 레벨에 대해 간략하게 설명
- Read Uncommitted (Level 0): SELECT 문장이 수행되는 동안 해당 Shared Lock이 걸리지 않는다. 트랜잭션이 현재 처리중이거나, 아직 Commit되지 않은 데이터를 다른 트랜잭션이 읽는 것을 허용한다
- Read Committed (Level 1): SELECT 문장이 수행되는 동안 해당 데이터에 Shared Lock이 걸린다. 트랜잭션이 수행되는 동안 다른 트랜잭션이 접근할 수 없어 대기하게 된다(Commit이 이루어진 트랜잭션만 조회 가능하다)
- Repeatable Read (Level 2): 트랜잭션이 완료될 때 까지 SELECT 문장이 사용하는 모든 데이터에 Shared Lock이 걸린다. 트랜잭션이 범위 내에서 조회한 데이터 내용이 항상 동일함을 보장한다
- Serializable (Level 3): 트랜잭션이 완료될 때 까지 SELECT 문장이 사용하는 모든 데이터에 Shared Lock이 걸린다. 완벽한 Read 일관성 모드를 제공한다

<br>
<hr>

### :notebook_with_decorative_cover: 데이터 모델링
1. 다양한 데이터 모델에 대해서 설명해주세요.
2. 데이터 모델링의 디자인 스키마에 대해서 설명해주세요.
3. 위에서 답변한 스키마 중에서 어떤 것이 더 낫습니까?

### :notebook_with_decorative_cover: 정규화 (Normalization)
1. 정규화란 무엇인지, 필요한 이유와 함께 답변해주세요.
2. 각 정규화 단계에 대해 **만족되어야 할 조건**을 중심으로 설명해주세요.
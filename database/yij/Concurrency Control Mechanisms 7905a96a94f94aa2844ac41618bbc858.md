# Concurrency Control Mechanisms

# Database Lock ❓

![Untitled](https://user-images.githubusercontent.com/30489264/142808253-8ed178bf-fabd-460d-9bbe-c70e56bdee78.png)

RDBMS에서 트랜잭션 시스템에 적용되는 동시성 제어 방법.

특정 트랜잭션이 레코드에 접근 중 일때 다른 트랜잭션이 해당 데이터에 접근하지 못 하게 막는 것을 의미한다. 일반적으로 Lock의 단위가 너무 작으면 독립성이 보장받지 못 하고, 너무 크면 트랜잭션이 직렬화 되어 수행 시간이 느려진다.

# Optimistic Concurrency Control

![Untitled 1](https://user-images.githubusercontent.com/30489264/142808274-788b5832-61a0-4c69-ae2c-3a913c15be82.png)

여러 트랜잭션이 **서로 간섭하지 않고 자주 완료될 수 있다**고 가정하는 동시성 제어 방식.

일반적으로 Block Contention(동일한 인덱스나 데이터 블록에 동시에 액세스하기 위해 경쟁하는 프로세스 또는 인스턴스)이 낮은 환경에서 사용한다.

원하는 규칙이 커밋 시 **위반되면 트랜잭션을 즉시 중단하고 다시 실행**하기 때문에 오버헤드가 발생한다. 너무 많은 트랜잭션이 중단되지 않는 상황일 경우 **일반적으로 좋은 전략**이다.

## 실행 과정

### Begin

트랜잭션의 시작을 표시하는 타임스탬프 기록

### Modify

데이터베이스 값을 읽고 변경 사항을 임시로 기록

### Validate

- 이 트랜잭션이 사용(Read/Write)한 데이터를 다른 트랜잭션이 수정했는지 확인
- 시작 시간 이후에 완료된 트랜잭션과 유효성 검사 시간에 여전히 활성 상태인 트랜잭션 포함

### Commit/Rollback

- 충돌이 없으면 모든 변경 사항 적용
- 충돌이 있는 경우 일반적으로 트랜잭션을 중단하여 해결

## 특징

### 장점

충돌이 드물 경우 락을 관리하는 비용 없이 트랜잭션을 완료할 수 있고, 다른 트랜잭션의 락이 해제될 때 까지 기다리지 않고도 높은 처리량을 기대할 수 있다.

### 단점

데이터 리소스에 대한 경합이 빈번한 경우, 트랜잭션을 반복적으로 다시 실행해야 하기 때문에 성능이 크게 저하된다.

- Redis가 WATCH 명령어를 통해 OCC를 제공한다
- MySQL이 Group Replication 구성을 위해 OCC를 구현한다
- Elasticsearch의 검색 엔진은 버전 속성을 통해 OCC를 지원한다

## OCC in JPA

```java
@Entity
public class Student {
    @Id
    private Long id;
    private String name;
    private String lastName;
    @Version
    private Integer version;
    // getters and setters
}
```

JPA에서는 `@Version` 어노테이션을 이용해서 낙관적 락을 사용할 수 있다. 이를 통해 외부에서 값을 검색할 수는 있지만 업데이트하거나 증가하도록 허용해서는 안 된다(Persistence Provider에게만 권한을 주어서 데이터의 일관성 유지).

이 때 지켜야 할 몇 가지 규칙이 있는데, 

1. 각 Entity Class에는 `@Version` 특성이 하나만 존재해야 한다
2. 여러 Table에 매핑된 경우 주 테이블에 배치돼야 한다
3. 유형은 int, long, short 혹은 Integer, Long, Short, java.sql 중 하나여야 한다.

# Pessimistic Concurrency Control

![Untitled 2](https://user-images.githubusercontent.com/30489264/142808297-5b05b622-c728-43c9-8501-0114246af8f8.png)

여러 트랜잭션이 자주 간섭된다고 가정하는 동시성 제어 방식.

규칙 위반을 유발하는 경우 위반 가능성이 사라질 때 까지 트랜잭션 작업을 차단한다. 일반적으로 **Shared Lock, Exclusive Lock**을 통해 이를 구현한다.

OCC에 비해 높은 무결성을 가지고 있지만, 교착 상태가 발생할 가능성이 존재한다.

### Shared Lock

트랜잭션이 읽기를 할 때 사용되는 락의 종류이다. 트랜잭션이 데이터를 조회할 때 공유락을 걸게 되면, 다른 트랜잭션은 그 데이터를 읽을 수 있고 변경이나 삭제는 불가능하다. 

### Exclusive Lock

배타락은 수정/삭제를 할 때 거는 락의 종류이다. 배타락을 걸게 되면 다른 트랜잭션은 그 데이터에 읽기/수정/삭제 연산을 할 수 없고, 어떠한 락도 걸 수 없다.

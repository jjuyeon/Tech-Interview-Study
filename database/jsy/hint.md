# :question: HINT (Oracle)

#### reference
https://gent.tistory.com/306<br>
https://gurume.tistory.com/entry/%EC%98%A4%EB%9D%BC%ED%81%B4-%EC%9E%90%EC%A3%BC%EC%82%AC%EC%9A%A9%ED%95%98%EB%8A%94-%ED%9E%8C%ED%8A%B8%EB%AA%A9%EB%A1%9D-%EC%A0%95%EB%A6%AC%EC%B9%9C%EC%A0%88%ED%95%9C-sql-%ED%8A%9C%EB%8B%9D
<hr>

## Question
1. HINT(힌트)는 무엇인가요?
- 오라클에서 SQL을 튜닝하기 위한 지시구문으로 옵티마이저가 최적의 계획으로 SQL문을 처리하지 못하는 경우에 개발자가 직접 최적의 실행 계획을 제공하는 것입니다. 
- 개발자가 SQL문 실행을 위한 데이터 스캐닝 경로, 조인 방법 등을 작성하여 옵티마이저에게 알려줍니다.

<hr>

## :nerd_face:	What I study
### 1. HINT

#### 1-1) 목적
- 오라클 서버가 업그레이드되면서 옵티마이저의 성능도 함께 향상되어 쿼리 실행시 최적의 방법으로 실행해준다.
- 하지만 옵티마이저가 항상 최적의 실행 경로를 만들어내기는 불가능하기 때문에 개발자(DBA)가 **직접 최적의 실행경로를 작성**해줘야 하는 경우가 생긴다. (튜닝)
- -> 이때 사용하는 것이 **HINT** 절이다!
  - ex) 인덱스 힌트: DBA는 한 쿼리에서 어떤 인덱스의 선택도가 높은지 파악한 후, 옵티마이저에 의존한 실행계획보다 효율적인 실행 계획을 만들어 내어야 한다.

#### 1-2) 사용법
- 액세스 경로, 조인 순서, 병렬 및 직렬 처리, 옵티마이저의 목표 변경, 데이터 값 정렬에 사용된다.
- HINT는 쿼리 서두에 명시한다.
- 첫 줄에 힌트 주석(`/*+(힌트명)*/`)을 작성하면 된다.
- 여러 개의 힌트를 명시할 수 있고, 공백으로 구분된다.
  - 공백이 아니면 힌트가 적용되지 않는다.
  - 단, 힌트 안의 괄호 사이에는 쉼표를 사용할 수 있다.
- 힌트 안의 괄호가 명시되는 테이블은 `ALIAS`으로 정의되며, 스키마까지 포함해서 작성하지 않는다.
- 주석에 꼭 `+`를 붙여야 힌트절이 실행되며, 없으면 일반 주석으로 간주된다.

  ```
  -- 인덱스 힌트(index_asc) : 인덱스 영역에서 순방향으로 스캔해라
  SELECT /*+ index_asc(e idx_myemp1_ename) */ EMPNO, ENAME, SAL FROM MYEMP1 e WHERE ENAME >= '가'
  ```

- HINT 쿼리에 오타가 있는 경우, 무시되어 HINT가 없는 것처럼 동작한다.
- 무분별한 HINT의 사용은 성능의 저하를 초래하기 때문에, 최적의 실행 경로를 알고 있을 경우 적절하게 사용되어야 한다.

#### 1-3) 종류
##### 출처: https://gurume.tistory.com/entry/오라클-자주사용하는-힌트목록-정리친절한-sql-튜닝
<br>

1. 최적화 목표
- `/*+ALL_LOWS */` : 전체 처리속도 최적화
- `/*+FIRST_ROWS(N) */` : 최초 N건 응답속도 최적화

2. 액세스 방식
- `/*+FULL */` : 인덱스 타지말고 바로 테이블 풀스캔으로 접근해라
- `/*+INDEX */` : 인덱스를 타라
- `/*+INDEX_DESC */` : 인덱스를 ORDER BY DESC 역순으로 타라 (시간, 결과값등 최근인 것 혹은 MAX값 구할때 좋음)
- `/*+INDEX_FFS */` : INDEX FAST FULL SCAN으로 타라
- `/*+INDEX_SS */` : INDEX SKIP SCAN

3. 조인순서
- `/*+ORDERED */` : FROM절에 나열된 테이블 순서대로 조인해라
- `/*+LEADING */`: 힌트절에 열거한 테이블 순서대로 조인해라 
  - ex) `/*+ LEADING (A B C)*/` (A,B,C 순서대로 조인)

4. 조인방식
- `/*+USE_NL */` : NL(NESTED LOOP - 중첩루프)방식 조인 유도
- `/*+USE_MERGE */` : 소트머지 조인으로 유도
- `/*+USE_HASH */` : 해시조인으로 유도
- `/*+NL_SJ */` : NL SEMI조인으로 유도
- `/*+MERGE_SJ */` : 소트머지 세미조인으로 유도
- `/*+HASH_SJ */` : 해시 세미조인으로 유도

5. 서브쿼리팩토링
- `/*+MATERIALIZE */` : WITH문으로 정의한 집합을 물리적으로 생성하도록 유도 
  - ex) `WITH /*+ MATERIALIZE*/ T AS (SELECT ...)`
- `/*+INLINE */` : WITH문으로 정의한 집합을 물리적으로 생성하지않고 INLINE 처리하도록 유도
  - ex) `WITH /*+ INLINE*/ T AS (SELECT ...)`

6. 쿼리변환
- `/*+ MEERGE */` : 뷰 MERGING 유도
- `/*+NO_MERGE */` : 뷰 MERGING 방지
- `/*+UNNEST */` : 서브쿼리 UNNESTING 유도
- `/*+NO_UNNEST */` : 서브쿼리 UNNESTING 방지
- `/*+PUSH_PRED */` : 조인조건 PUSHDOWN 유도
- `/*+NO_PUSH_PRED */` : 조인조건 PUSHDOWN 방지
- `/*+USE_CONCAT */` : OR 또는 IN-LIST조건을 OR-EXPANSION으로 유도
- `/*+NO_EXPAND */` : OR 또는 IN-LIST 조건에 대한 OR-EXPANSION방지

7. 병렬처리

- `/*+PARALLEL */` : 테이블 스캔, DML 병렬방식으로 처리하도록 할때 사용, 단일 대형 테이블의 접근시 정말 많이 쓴다. 
  - ex) `/*+ PARALLEL(T1 4)*/`
- `/*+PARALLEL_INDEX */` : 인덱스 스캔을 병렬방식으로 처리하도록 유도
- `/*+PQ_DISTRIBUTE */` : 병렬수행시 데이터 분배방식 결정
  - ex) PQ_DISTRIBUTE(T1 HASH(--BUILD INPUT) HASH(--PROBE TABLE))

8. 기타

- `/*+APPEND*/` : DIRECT PATH INSERT유도로 INSERT문에 주로 많이 쓴다.
- `/*+DRIVING_SITE */` : DB LINK REMOTE쿼리에 대한 최적화 및 실행 주체 지정 (LOCAL 또는 REMOTE)
- `/*+PUSH_SUBQ */` : 서브쿼리를 가급적 빨리 필터링하도록 유도
- `/*+NO_PUSH_SUBQ */` : 서브쿼리를 가급적 늦게 필터링하도록 유도
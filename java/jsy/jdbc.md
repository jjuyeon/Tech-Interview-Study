# :question: JDBC

#### reference
https://devlog-wjdrbs96.tistory.com/139<br>
https://shs2810.tistory.com/18
<hr>

## Question
1. JDBC란 무엇인가요?
- JDBC는 데이터베이스와 연결되어 데이터를 주고 받을 수 있도록 하는 API 입니다. 
- JDBC의 특징으로는 데이터베이스의 종류에 종속되지 않는다는 점과 응용프로그램과 DBMS 간의 통신을 중간에서 변역해주는 역할을 한다는 것입니다.
<hr>

## :nerd_face:	What I study

### JDBC
- Java DataBase Connectivity
- 자바 언어로 데이터베이스와 연결되어 데이터를 주고 받을 수 있도록 사용되는 라이브러리(API)
- DBMS에 종속되지 않는 관련 API를 제공한다.
  - 데이터베이스 종류에 상관없다.
  - 응용프로그램과 DBMS간의 통신을 중간에서 번역해주는 역할을 한다.

<br>

![jdbc](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Foed9z%2FbtqDjGqc8Hh%2FfTNTE08lq5mhxWQecAi5k0%2Fimg.png)

- JDBC Driver: 각 DBMS에서 제공하는 라이브러리 압축파일
- JDBC API : 다양한 클래스와 인터페이스로 구성된 패키지 `java.sql`로 구성되어 있다.
  - 데이터베이스를 연결하여 테이블 형태의 자료를 참조
  - SQL문 질의
  - SQL문 결과 처리
  - 구체적인 메소드 설명  [https://shs2810.tistory.com/18]

<br><br>

### JDBC를 이용한 데이터베이스 연동 과정
![connection](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FOvHi2%2FbtqDibLdLep%2FRLGbbmViRkyd4jkfDKkGw1%2Fimg.png)

1. JDBC 드라이버 Load
    - 데이터베이스와의 연결을 위해 드라이버를 로딩한다.
2. Connection 객체 생성
    - 데이터베이스와의 연결을 위해 URL과 계정 정보가 필요하다.
3. Statement 객체 생성
    - SQL 구문을 정의하고, 쿼리 전송 전에 변수를 세팅한다.
4. Query 수행
    - executeUpdate
      - SQL Query문이 INSERT, DELETE, UPDATE인 경우
      - 반환 타입은 int
    - executeQuery
      - SQL Query문이 SELECT인 경우
      - 반환 타입은 ResultSet
5. Resultset 객체로부터 데이터 추출 (SELECT인 경우)
    - 데이터베이스 조회 결과집합에 대한 표준
    - `next()`를 통해 DB의 테이블 안에 row를 조회한다.
    - `getString()`, `getInt()` 등을 통해 한 행의 특정 column을 조회한다.
6. Close (Resultset, Statement, Connection)
    - 열어줬던 객체들을 하나씩 닫아준다.
# Hibernate

## SQL Mapper vs ORM

- SQL Mapper
  - SQL <-매핑-> Object
  - SQL Mapper는 SQL 쿼리로 직접 DB 데이터를 다룸
  - ex) MyBatis
- ORM(Object-Relational Mapping) 객체-관계 매핑
  - DB 데이터 <-매핑-> Object
  - 객체를 통해 간접적으로 데이터베이스 데이터를 다룸
  - 객체간의 관계를 바탕으로 SQL을 자동으로 생성해 준다

### Hibernate

- 자바 언어를 위한 객체 관계 매핑 프레임워크
- `JPA`는 Java Persistence API의 약자로, 자바 어플리케이션에서 관계형 데이터베이스를 사용하는 방식을 정의한 `인터페이스`이다.
- JPA는 단순한 명세이기 때문에 구현이 없다.
- Hibernate는 JPA의 구현제이다.
- 그렇기 때문에 JPA를 사용하기 위해서 Hibernate 대신에 다른 구현체를 사용하여도 상관이 없다.

## MyBatis vs JPA

- MyBatis
  - SQL Mapper
  - 장점
    - SQL 쿼리를 직접 작성하므로 최적화된 쿼리를 구현 가능
    - 엔티니에 종속받지 않고 다양한 테이블 조합이 가능
    - 복잡한 쿼리도 SQL 쿼리만 작성할 수 있다면 처리 가능
  - 단점
    - 스키마 변경시 SQL 쿼리를 직접 수정해줘야함
    - 반복된 쿼리가 발생하여 반복된 작업이 있음
    - DB변경시 로직도 함께 수정해주어야함
- JPA
  - 장점
    - 개발이 편리함 (SQL을 직접 작성하지 않아도 됨)
    - 테이블 변경 시 JPA의 엔티티만 수정해주면 됨

### Spring Data JPA

- Spring Data JPA는 Spring에서 제공하는 모듈로, JPA를 한 단계 추상화시킨 `Repository` 라는 인터페이스를 제공해준다.
- `Repository` 인터페이스에 정해진 규칙대로 메소르를 입력하면 Spring이 알아서 해당 메소드 이름에 적합한 쿼리를 날리는 구현체를 만들어서 Bean을 등록해 준다.

### 정리

- JPA는 자바 JDBC를 사용하는 방식을 정의한 인터페이스이다.
- Hibernate는 JPA의 구현한 라이브러리이다.
- Spring Data JPA는 JPA를 쓰기 편하게 하기 위해 JPA를 한 단계 추상화 시킨 모듈이다.

<img width="996" alt="jpa" src="https://user-images.githubusercontent.com/43779730/137508464-7351ffb4-8ee8-454d-8f51-d9cd7092078e.png">

### 참고 사이트 및 문헌

- https://spring.io/projects/spring-data-jpa#overview
- https://suhwan.dev/2019/02/24/jpa-vs-hibernate-vs-spring-data-jpa/

# SQL Injection

## SQL Injection 이란?

SQL Injection이란 SQL 쿼리문을 조작할 수 있는 입력을 사용해서 데이터를 악의적으로 이용하거나 탈취할 수 있는 공격을 의미한다.
주로 로그인, 사용자 입력 창에서 입력 값에 대한 검증 로직이 빈약할 경우 발생할 수 있으며 이로 인한 피해는 인증 우회/원격 명령 실행/DB 정보 수정,삭제,탈취와 같은 피해가 발생할 수 있다.

![image](https://user-images.githubusercontent.com/30489264/139569339-9702cd5b-7758-44d1-9bc0-41508df627e7.png)

2017년 우리나라 숙박 애플리케이션 여기어때도 SQL Injection으로 인해 고객 정보를 탈취당한 사례가 있다.

<br>

```java
public List<AccountDTO>
  unsafeFindAccountsByCustomerId(String customerId)
  throws SQLException {
    // UNSAFE !!! DON'T DO THIS !!!
    String sql = "select "
      + "customer_id,acc_number,branch_id,balance "
      + "from Accounts where customer_id = '"
      + customerId 
      + "'";
    Connection c = dataSource.getConnection();
    ResultSet rs = c.createStatement().executeQuery(sql);
    // ...
}
```

이 코드에서는 customerId를 인자로 받아 SQL문을 실행한다.
물론 코드의 목표는 `customer_id`와 일치하는 사용자 계정이 존재할 때 에만 정보를 조회하는 것이다.
하지만 아래와 같은 API 요청은 전혀 의도하지 않은 결과를 도출할 수 있다. 

```shell
curl -X GET \
  'http://localhost:8080/accounts?customerId=abc%27%20or%20%271%27=%271' \
```

이는 아래와 같은 customerId 인자로 변환된다.

```
abc' or '1' = '1
```

위의 SQL 쿼리문과 결합해서 생성된 동적인 쿼리문은 다음과 같다.

```mysql
select customer_id, acc_number,branch_id, balance
  from Accounts where customerId = 'abc' or '1' = '1'
```

이는 where 절에 `'1'='1'`과 같이 언제나 true를 반환하는 조건을 삽입해서, 모든 사용자 계정을 얻어내는 목적으로 사용할 수 있다.

또, 다음과 같은 로그인 쿼리가 있다고 했을 때

```mysql
SELECT * FROM user WHERE user_id = ? AND user_pwd = ?
```

의도한 동작은 유효한 ID가 존재하고, (아마도 암호화된) 패스워드와 동일하게 입력해야만 동작하는 것이 기본 의도일 것 이다.

하지만 user_id에 `test_id' #`을 입력하면 다음과 같은 쿼리가 실행된다.

```mysql
SELECT * FROM user WHERE user_id = 'test_id' #' AND user_pwd = 'qwerty1234'
```

`#`은 MySQL에서 주석을 의미하기 때문에, 원하는 id의 개인 정보를 탈취할 수 있는 쿼리문이 실행될 것 이다.

<br>

## SQL Injection 종류

### Error Based SQL Injection

![image](https://user-images.githubusercontent.com/30489264/139570349-478e9947-c1ec-46a3-94d9-2717e4cf1a4a.png)

싱글 쿼드('), 세미 콜론(;)과 같이 SQL 문법 오류를 유발하는 특수 문자를 사용하고, 오류 메시지를 통해 데이터베이스의 정보를 유출하거나 의도치 않은 결과를 실행시킬 수 있는 공격

### Union Based SQL Injection

![image](https://user-images.githubusercontent.com/30489264/139570332-e555a707-e959-45a6-a2a8-1e0c2acb5ccc.png)

UNION 키워드를 이용해서 다른 테이블의 정보를 함께 조회하여 데이터를 유출할 수 있는 공격

### Blind based SQL Injection

![image](https://user-images.githubusercontent.com/30489264/139570342-f8989b4e-384b-4c3a-8ffe-cbd601b3024a.png)

쿼리문의 실행 결과가 참/거짓 만을 반환할 때 사용하는 공격. 함수를 이용해서 쿼리 결과를 얻고, 아스키코드로 변환해서 비교하는 과정을 통해 데이터베이스의 테이블 정보와 같은 정보들을 얻을 수 있다.

이외에도 많은 SQL Injection 방식들이 있지만 위의 세 가지가 대표적인 SQL Injection의 예시이다.

<br>

## SQL Injection in Persistence Framework

웹 개발 중 주로 사용하는 퍼시스턴스 프레임워크는 myBatis, JPA가 있다.
물론 둘다 잘못 사용하면 SQL Injection의 위험에 노출된다.
자바에서의 `PreparedStatement`를 사용하면 사용자 입력을 쿼리문에 그대로 삽입하는 것이 아니라 문자열로 변환한 뒤 넣기 때문에 SQL Injection을 기본적으로 예방할 수 있다.

```java
public List<AccountDTO> unsafeJpaFindAccountsByCustomerId(String customerId) {    
    String jql = "from Account where customerId = '" + customerId + "'";        
    TypedQuery<Account> q = em.createQuery(jql, Account.class);        
    return q.getResultList()
      .stream()
      .map(this::toAccountDTO)
      .collect(Collectors.toList());        
}
```

따라서 SQL Injection은 String concatenation 기반의 쿼리문을 작성할 때 주로 발생하기에 간단한 예방 방법을 살펴보자

<br>

### Parameterized Query

```java
public Item findOneByName(String name) {
    return em.createQuery("select i from Item i where i.name = :name", Item.class)
            .setParameter("name", name)
            .getSingleResult();
}
```

Parameterized Query를 사용하는 것은 사용자 입력을 쿼리로 전송하는 것이 아니라 문자열 인수로 전달한다.
SQL Injection 공격이 발생할 수 있는 "사용자가 DB Query 수행 권한을 가진다"라는 전제 조건을 차단하는 것 이다.

### Query API

따라서 JPQL을 사용할 때 에도 parameter를 사용하는 것이 안전하지만 실제로는 JPA Criteria, QueryDSL같은 API를 사용하는 것을 권장한다고 한다.

```java
 Member findMember = queryFactory
                      .selectFrom(member)
                      .join(member.team, team).fetchJoin()
                      .where(member.username.eq("member1"))
                      .fetchOne();
```

빌더 패턴 기반의 동적 쿼리 생성 API들은 데이터 영속성에 관련된 연산들의 가시성을 높여주고, SQL Injection과 같은 취약점을 예방할 수 있다는 장점이 있다.

<br>

## Insecure Application 실습

https://github.com/WebGoat/WebGoat

WebGoat는 Java 기반의 웹 사이트에서 겪을 수 있는 다양한 웹 취약점을 실험해볼 수 있는 프로젝트이다.

프로젝트를 하면서 웹 취약점에 대해 깊게 신경쓰지 못 했었는데, 이번 프로젝트가 끝나면 이 부분도 실습해보며 공부하면 좋을 것 같다 !!

<br>

## 참고 자료

> https://www.baeldung.com/sql-injection
> https://stackoverflow.com/questions/14102334/how-to-prevent-sql-injection-with-jpa-and-hibernate/54122517
> https://www.ahnlab.com/kr/site/securityinfo/secunews/secuNewsView.do?curPage=91&seq=7964
> http://www.newstown.co.kr/news/articleView.html?idxno=281308
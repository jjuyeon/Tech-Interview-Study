## Date의 문제점

- `java.util.Date` , `java.sql.date`
- 불변 객체가 아니다 (Not immutable)
- 상수 필드의 남용
  - ex) Calendar.SECOND ,Calendar.OCTOBER
- 헷갈리는 값 지정
  - 일부 값이 0 부터 시작한다.
  - ex) Calender.OCTOBER = 9
  - calendar.set(2021,10,21) = 2021년 9월 21일
  - calendar.set(2020,12,31) = 2021년 1월 31일 -> 12를 넣어도 에러가 아닌 1로 변환된다
- 일관성 없는 상수

  - Calender.DAY_OF_WEEK는 int형으로, 일요일을 1로 표현한다
  - Date.getDay()로 요일을 구하면 일요일을 0으로 표현한다.

- Date와 Calendar의 불편한 역할 분담

  - 특정 시간대의 날짜를 생성하거나, 날짜 단위의 계산을 Date 클래스만으로 수행이 불가능
  - 날짜 연산을 위해선 Calendar 객체를 생성하고 생성된 객체에서 Date를 생성한다. -> 불필요한 과정을 거침
  - 헷갈리는 함수명 : Calendar.getTime() : 반환값 Date 타입

- 오류에 둔감한 ID 지정

  - Asia/Seoul 대신 Seoul/Asia로 오타가 나도 오류가 발생하지 않음

    ```
    TimeZone zone = TimeZone.getTimeZone("Seoul/Asia");
    ```

## LocalDate, LocalDateTime

- `java.time.LocalDate` , `java.time.LocalDateTime`
- Joda-Time이나 다른 오픈소스 라이브러리에 영향을 많이 받은 표준 명세
- DateTime 대신 ZoneDateTime을 사용
- 단위별 날짜 연산 메소드를 지원함(plusDays, plusMinutes)
- 월의 int값과 명칭이 일치함 `1월` = `(int)1`
- 13월처럼 잘못된 값이 들어가면 객체 생성 시점에서 `DateTimeException`을 던짐
- 요일 클래스는 Enum 상수로 제공됨
- 잘못 된 시간대 ID 지정에 대해서 `ZoneRulesException` 던짐

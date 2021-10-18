# :question: DAO, DTO, VO

#### reference
http://melonicedlatte.com/2021/07/24/231500.html
<hr>

## Question
1. DAO, DTO, VO에 대해 설명해보세요.
- DAO는 데이터베이스의 데이터에 접근하기 위한 객체입니다. DTO는 로직을 가지지 않고 순수하게 계층 간 데이터 교환을 하기 위해 사용하는 객체입니다. VO는 값을 위해 사용되는 객체로 오직 읽는 것만 가능합니다.

<hr>

## :nerd_face:	What I study

### 1. DAO (Data Access Object)
- 데이터베이스의 data에 접근하기 위한 객체
- 데이터베이스에 접근하기 위한 비즈니스 로직을 분리하기 위해 사용한다.
- 데이터베이스에 접근하여 데이터를 CRUD 할 수 있는 기능을 수행한다.
- MVC 패턴의 Model 부분을 의미한다.

<br>

### 2. DTO (Data Transfer Object)
- 계층 간(Controller, View, Business Layer) 데이터 교환을 하기 위해 사용하는 객체
- 로직을 가지지 않는 순수한 데이터 객체
- getter, setter만을 가진 클래스

<br>

### 3. VO (Value Object)
- 값을 위해 사용되는 객체
- 사용하는 도중에 변경이 불가능하며, 오직 읽는 것만 가능하다. (read-only)

|DTO|VO|
|:---:|:---:|
|가변|불변|
|getter/setter|getter|
|인스턴스의 개념|리터럴의 개념|
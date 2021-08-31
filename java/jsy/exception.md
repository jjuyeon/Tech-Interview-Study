# :question: JAVA 의 예외처리

#### reference
https://cheese10yun.github.io/checked-exception/<br>
https://devlog-wjdrbs96.tistory.com/351
<hr>

## Question
1. [Checked Exception과 Unchecked Exception의 차이를 설명해주세요.](#1-checked-exception)
- Runtime Exception을 상속하지 않는 클래스를 Checked exception이라고 하며, 상속하는 클래스는 Unchecked exception이라고 합니다. 
- Checked exception은 반드시 예외 처리를 해야 하지만, Unchecked exception은 명시적으로 하지 않아도 된다는 차이점이 있습니다.
<hr>

## :nerd_face:	What I study
### 1. Checked Exception
- ***반드시 명시적으로 예외 처리를 해야한다.***
- **try-catch문**을 사용하여 예외 처리를 하거나, **throw**를 통해서 호출한 메소드로 예외를 던져야 한다.
- 트랜잭션 rollback이 되지 않고 commit까지 완료된다.
- Exception을 상속 받는 하위 클래스 중, Runtime Exception을 제외한 모든 예외 클래스
<br>

### 2. Unchecked Exception
- ***명시적으로 예외 처리를 강제하지 않는다.***
- 실행 중에 발생할 수 있는 예외이다.
- 트랜잭션 rollback이 진행된다.
- Runtime Exception을 상속받는 모든 예외 클래스
<br>

||Checked Exception|Unchecked Exception|
|:---:|:---:|:---:|
|처리 여부|필수|Optional|
|확인시점|컴파일 단계|실행 단계|
|Rollback 여부|X|O|
|ex|IO Exception, SQL Exception|NullPointer Exception, IllegalArgument Exception|

<br><br>

### 3. Error
- 시스템 레벨에서 발생하는 심각한 수준의 오류를 의미한다.
- 개발자가 예측하기 쉽지 않으며, 직접 처리할 수 있는 방법도 없다.
- 그러므로 개발 시 예외 처리에 신경쓰지 않아도 된다.
- ex. OutOfMemory Error, StackOverFlow Error
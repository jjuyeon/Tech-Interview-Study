# :question: 정규화 (Normalization)

#### reference
https://m.blog.naver.com/PostView.naver?isHttpsRedirect=true&blogId=dydghdkdl&logNo=221165683361 <br>
https://m.blog.naver.com/cjw950607/221460234203 <br>
https://nirsa.tistory.com/107
<hr>

## Question
1. [정규화란 무엇인지, 필요한 이유와 함께 답변해주세요.](#1-normalization)
- 하나의 릴레이션에 하나의 의미만 존재할 수 있도록 릴레이션을 분해하는 과정을 뜻합니다.
- 데이터의 중복을 최소화하고 일관성을 보장하기 위해 정규화 과정을 거칩니다.
<br><br/>

2. [각 정규화 단계에 대해 **만족되어야 할 조건**을 중심으로 설명해주세요.](#2-정규화-단계)
- 총 5(+1)단계로 구성되어 있습니다.
- 제 1정규화는 ***각 컬럼들의 값이 원자값을 가지도록 바꾸는 정규화***를 말합니다.
- 제 2정규화는 ***테이블의 모든 컬럼에서 부분적 함수 종속을 제거하는 정규화***를 말합니다.
- 제 3정규화는 ***기본키를 제외한 속성들 간의 이행적 함수 종속을 제거하는 정규화***를 말합니다.
- BNCF는 ***결정자이면서 후보키가 아닌 것을 제거하는 정규화***를 말합니다.
- 제 4정규화는 ***결합 종속성을 제거하는 정규화***를 말합니다.
- 제 5정규화는 ***합성키에 순환 종속성을 제거하는 정규화***를 말합니다.
<hr/>

## :nerd_face:	What I study
### 1. Normalization
- 관계형 데이터베이스의 설계에서 중복을 최소화하게 데이터를 구조화하는 프로세스
- 목적: 데이터 구조의 안정성 최대화, 중복 배제를 통한 DML 이상 발생 방지
- 정규화를 거치지 않으면 이상(Anormaly) 문제가 발생한다.
- 모든 테이블을 정규화하는 것은 좋지 않다. (JOIN 연산이 많아져 성능의 저하가 발생할 수 있기 때문)

#### 1) 장점
- 이상(Anormaly) 현상을 방지하여 유연한 데이터 구축이 가능해진다.
- 불필요한 쿼리(ex. 서브쿼리)를 제거할 수 있다.
- 새로운 데이터 형의 추가로 인한 확장 시, 구조를 최소한으로 변경할 수 있다.
#### 2) 단점
- 릴레이션의 분해로 인해 릴레이션 간 JOIN연산이 많아진다. -> 질의 응답시간이 느려질 수 있다. (성능 저하)
- 위의 해결방법으로는 '반정규화'를 적용해볼 수 있다.
<br></br>

### 2. 정규화 단계
- 정규화는 1-5 정규화까지 있지만, 실무에서는 대체로 1-3 정규화까지의 과정을 거친다.
#### 1) 1NF
Before|After
:-------------------------:|:-------------------------:
![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FbIXuX0%2FbtqBGpEhISu%2FkNCOwnGlgcOJLMP43tiV6k%2Fimg.png) | ![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FcDtHsC%2FbtqBLiXLMaA%2Fe1iuGoGl5Eppp0CP5PAzEk%2Fimg.png) 

- 릴레이션에 속한 모든 속성의 도메인(속성값)이 원자 값으로만 구성되어야 한다.
- 테이블의 각 컬럼의 유형은 유일해야 한다.
#### 2) 2NF
Before|After
:-------------------------:|:-------------------------:
![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FcmTwvD%2FbtqBHTxUlyE%2FWad2A4jpY6pEXfarlh1v91%2Fimg.png) | ![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FeNjmkB%2FbtqBGfor6SV%2F7OcKkFE73Xiw0T9Ylc1nu1%2Fimg.png)
- 제 1정규화를 만족해야 한다.
- 모든 속성들은 오직 개체의 고유 식별자(유일키)에만 의존해야 한다.
- 기본키를 제외한 속성이 기본키에 완전 함수 종속되어야 한다.
#### 3) 3NF
- **면접질문 빈도수가 가장 높은 정규화**

Before|After
:-------------------------:|:-------------------------:
![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FeNjmkB%2FbtqBGfor6SV%2F7OcKkFE73Xiw0T9Ylc1nu1%2Fimg.png) | ![](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FSlASz%2FbtqBHTdDoo3%2F6yv3htZo6k1twifKSUd9MK%2Fimg.png)
- 제 2정규화를 만족해야 한다.
- 테이블의 키 외에 다른 속성에 의존하는 요소가 존재하면 안된다.
- 위와 같은 요소가 존재할 경우, 다른 새로운 테이블로 옮긴다.
- 3NF까지 완성되면 데이터베이스는 정규화된 것으로 간주한다.
#### 4) BNCF
- 제 3정규화를 만족해야 한다.
- 모든 테이블들은 오직 하나의 기본키를 가져야 한다.
- 결정자이면서 후보키가 아닌 것을 제거한다.
#### 5) 4NF
- BNCF를 만족해야 한다.
- 테이블은 하나의 키(pk)에 여러 값을 가질 수 없다. (결합 종속성 제거)
- 다치 종속을 제거한다.
#### 6) 5NF 
- 제 4정규화를 만족해야 한다.
- 후보키를 통하지 않는 조인 종속을 제거한다.
<br></br>

### 3. 함수적 종속
- 테이블 속성들간의 관계에 대한 제약조건
- 속성 A가 속성 B를 결정할 때, B는 A에 함수적으로 종속된다고 한다.
#### **1) 완전함수종속**
- 함수적 종속에서 X의 값이 여러 요소일 경우, 즉, {X1, X2} -> Y일 경우, X1과 X2가 Y의 값을 결정할 때를 의미한다.
#### **2) 부분함수종속**
- X1, X2 중 하나만 Y의 값을 정할 때를 의미한다.
#### **3) 이행적함수종속**
- 세 가지 종속간의 종속이 (X->Y) & (Y->Z) 일 경우 X->Z가 성립되는 종속을 의미한다.
<br></br>

### 4. Anomaly 현상
- 정규화를 거치지 않아 데이터가 불필요하게 중복되어 생기는 예기치 않게 발생하는 문제
- 좋은 관계형 데이터베이스를 설계하기 위해서는 정보의 이상 현상이 생기지 않도록 고려해야 한다.
#### 1) 갱신 이상
- 릴레이션에서 튜플의 속성값을 변경할 때, 일부 튜플만 변경되는 현상
#### 2) 삽입 이상
- 릴레이션에 데이터를 삽입할 때, 원하지 않는값이 삽입되는 현상
#### 3) 삭제 이상
- 릴레이션에서 튜플을 삭제할 때, 원하지 않는 다른 값들도 연쇄적으로 삭제되는 현상

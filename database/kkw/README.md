
# 데이터 모델링
## 데이터 모델링
### 데이터 모델링이란?
- 정보 시스템을 구축하기 위한 데이터관점의 업무 분석 기법
- 현실 세계의 데이터에 대한 약속된 표기법으로 표현하는 과정
- 데이터 베이스를 구착하기 위한 분석/설계 과정

### 데이터 모델의 기능
- 시스템을 현재 또는 원하는 모습으로 가시화하도록 도와준다
- 시스템의 구조와 행동을 명세화 할 수 있게한다
- 시스템을 구착하는 구조화된 틀을 제공한다
- 시스템을 구축하는 과정에서 결정한 것을 문서화한다
- 다양한 영역에 집중하기 위해 다른 영역의 세부 사항은 숨기는 다양한 관점을 제공
- 특정 목표에 따라 구체화된 상세 수준의 표현 방법을 제공

## 데이터 모델링의 중요성과 유의점
### 데이터 모델링의 중요성
- 파급효과 : 데이터 모델링이 초기에 제대로 이루어지지 않으면, 구현하고 테스트하는 과정에서 문제점이 나타날 수 있음. 모델을 변경해야될 경우, 다양한 분석이 추가로 필요함
- 복잡한 정보 요구사항의 간결한 표현 : 구축할 시스템의 요구사항과 한계를 가장 명확하고 간결하게 표현할 수 있게 해준다.
- 데이터 품질 : DB에 담긴 데이터는 기업/단체의 중요한 자산이다. 이러한 데이터의 보관 기간이 늘어날 수록 활용가치가 더 높아진다. 그러기 위해선 정확성과 품질이 높아야 한다.

### 데이터 모델링의 유의점
- 중복 : 한 DB가 여러 장소에 같은 정보를 저장하지 않도록 유의해야 한다.
- 비유연성 : 데이터의 정의를 데이터의 사용 프로세스와 분리하여  유지 보수시 위험성을 낮춘다
- 비일관성 : 데이터의 모순, 데이터와 데이터 간 상호 연관 관계에 대해 명호가하게 정의해서 방지한다. ( 중복이 없더라도 비일관성이 발생할 수 있다 )

## 데이터 모델링의 3단계
- 데이터 모델링을 하는데 있어서 시간에 따라 진행되는 3가지 과정이 있으며, 추상화 수준에 따라 달라지며 개념적 데이터 모델링, 논리적 데이터 모델링, 물리적 데이터 모델링으로 정리된다.

|데이터 모델링|내용|수준|
|-----------|----------------------------|:--------:|
|개념적 데이터 모델링|업무 중심적이고 포괄적인 수준의 모델링 진행|추상적|
|논리적 데이터 모델링|시슽메으로 구축하고자 하는 업무에 대해 Key 속성, 관계 등을 정확하게 표현|↓|
|물리적 데이터 모델링|실제 DB에 이식할 수 있도록 성능,저장 등 물리적인 성격을 고려하여 설계|구체적|

### 개념적 데이터 모델링
- 추상화 수준이 높고, 업무 중심적이고 포괄적인 수준의 모델링
- 어떤 자료가 중요하고 유지되어야하는지를 결정
- 핵심 엔티티와 그들간의 관계를 발견하고, 그것을 표현하기 위해서 엔티티-관계 다이어그램(ER Diagram)을 생성하는 것
- 사용자와 시스템 개발자가 데이터 요구 사항을 발견할 수 있도록 지원한다
- 현 시스템이 어떻게 변형되어야 하는가를 이해하는데 유용함

### 논리적 데이터 모델링
- 비지니스 정보의 논리적인 구조와 규칙을 명확하게 표현하는 기법 또는 과정
- 논리 데이터 모델링의 결과로 얻어지는 논리 데이터 모델은 데이터 모델링이 최종적으로 완료된 상태라고 정의할 수 있음
- 정규화가 이루어짐, 시스템으로 구축하고자 하는 업무에 대해 Key, 속성, 관계 등을 정확하게 표현하는 단계의 모델링이다
- 재사용성이 높음

### 물리적 데이터 모델링
- 실제 DB에 이식할 수 있도록 성능, 저장 등 물리적인 성격을 고려하여 설계하는 단계
- 데이터가 물리적으로 컴퓨터에 어떻게 저장될 것인가에 대해 정의하는 단계
- 테이블, 칼럼 등으로 표현되는 물리적인 저장구조와 사용될 저장 장치, 자료를 추출하기 위해 사용될 접근 방법 등을 결정함

  
  
 
## 데이터 독립성
- 단계별 스키마에 따라 데이터 정의어와 데이터 조작어가 다름을 제공
- 각 View의 독립성을 유지하고 계층별 VIew에 영향을 주지 않고 변경 가능

### 데이터 독립성의 필요성
- 유지 보수 비용 증가
- 데이터 중복성 증가
- 데이터 복잡도 증가
- 요구사항 대응 저하


### 데이터베이스의 3단계 구조
- 데이터 독립성을 가지는 모델은 다음과 같이 외부 단계, 개념적 단계, 내부적 단계로 서로 간섭되지 않는 모델을 제시한다.

### 외부 단계 (외부 스키마, External Schema)
- view 단계
- 여러 개의 사용자 관점으로 구성
- 개개인의 사용자 단계로서 개개인의 사용자가 보는 개인적인 DB 스키마
- DB의 개개 사용자나 응용 프로그래머가 접근하는 DB 정의

### 개념적 단계(개념 스키마, Conceptual Schema)
- 통합적인 관점
- 개념 단계 하나의 개념적 스키마로 구성되는 모든 사용자 관점을 통합한 조직 전체의 DB를 기술하는 것
- 모든 응용 시스템들이나 사용자들이 필요로 하는 데이터를 통합한 조직 전체의 DB를 기술한 것으로 DB에 저장되는 데이터와 그들간의 관계를 표현한 것

### 내부적 단계(내부 스키마, Internal Schema)
- DB가 물리적으로 저장한 형식을 나타냄
- 물리적 장치에서 데이터가 실제적으로 저장되는 방법을 표현

### 데이터 독립성
|독립성|내용|특징
|------------|--------------------------|:----:|
|논리적 독립성|개념 스키마가 변경되어도 외부 스키마에는 영향을 미치지 않도록 지원하는 것, 논리적 구조가 변경되어도 응용 프로그램에 영향이 없음|사용자 특성에 맞는 변경 가능, 통합 구조 변경 가능|
|물리적 독립성|내부 스키마가 변경 되어도 외부/개념 스키마는 영향을 받지 않도록 지원하는 것, 저장 장치의 구조 변경은 응용 프로그램과 개념 스키마에 영향이 없음|물리적 구조 영향없이 개념 구조 변경 가능, 개념 구조 영향없이 물리구조 변경 가능|

## 좋은 데이터 모델의 요소
1. 완정성 : 업무에 필요로 하는 모든 데이터가 데이터 모델의 정의되어 있어야 함
2. 중복 배제 : 하나의 DB 내에는 동일한 사실은 반드시 한 번만 기록 되어야 함
3. 업무 규칙 : 데이터 모델링 과정에서 도출되고 규명되는 수많은 업무 규칙을 데이터 모델에 표현하고 이를 해당 데이터 모델을 활용하는 모든 사용자가 공유할 수 있도록 제공해야함
4. 데이터 재사용 : 데이터의 통합성과 독립성에 대해서 충분히 고려하여 데이터의 재사용성을 향상시킬 수 있어야함
5. 의사 소통 : 업무규칙들은 해당 정보세스템을 운용, 관리하는 많은 관리자들이 설계자가 정의한 업무규칙들을 동일한 의미로 받아들이고 정보시스템을 활용할 수 있게 하는 역할과 데이터 모델이 진정한 의사소통의 도구로서의 역할을 하게 
6. 통합성 : 동일한 성격의 데이터를 한 번만 정의함으로써 공유 데이터에 대한 구조를 여러 업무 영역에서 공동으로 사용하기 용이하도록 해야함
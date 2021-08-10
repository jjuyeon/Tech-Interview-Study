# :question: Paging & Segmentation

#### reference
https://velog.io/@codemcd/%EC%9A%B4%EC%98%81%EC%B2%B4%EC%A0%9COS-13.-%ED%8E%98%EC%9D%B4%EC%A7%95<br>
https://hibee.tistory.com/303
<hr>

## Question
1. [페이징과 세그먼테이션에 대해서 설명하세요.](#2-paging)
- 메모리 단편화 해결 기법으로 페이징과 세그먼테이션 기법이 있습니다.
- 페이징은 프로세스를 일정한 사이즈의 페이지로 나누어 물리적 메모리에  불연속적으로 저장하는 방식을 말합니다. 이를 통해 외부 단편화 문제를 해결할 수 있습니다.
- 세그먼테이션은 프로세스를 서로 크기가 다른 논리적인 블록 단위인 세그먼트로 분할하고, 메모리에 저장하는 방식을 말합니다. 이를 통해 내부 단편화 문제를 해결할 수 있습니다.
<br><br>

1. [내부 단편화와 외부 단편화가 무엇인지 설명하세요.](#1-단편화)
- 내부 단편화는 프로세스가 자신의 크기보다 더 큰 메모리 공간에 적재된 후, 그 공간 안에 남게 되는 메모리 공간을 의미합니다. 프로세스가 할당되었지만, 사용하지 못하는 공간이 있기 때문에 메모리가 낭비됩니다.
- 외부 단편화는 메모리 공간이 쪼개져, 프로세스가 적재될만큼 충분한 공간이 존재하지 않는 경우 발생합니다. 어떤 프로세스도 차지하지 못한 빈 공간이지만, 프로세스를 넣을 수 없습니다.
<hr>

## :nerd_face:	What I study
### 1. 단편화
- 메모리에 프로세스들이 적재, 제거되는 일이 반복된다. 
- 이때, 프로세스들이 차지하는 메모리 틈 사이에 프로세스가 충분히 사용하지 못하는 작은 공간들이 늘어난다. <- 이 작은 공간을 '단편화'라고 한다.
- 실제로는 메모리 공간이 충분하지만, 사용이 불가능하여 시스템 성능이 저하될 수 있다.
- 페이지 교체가 자주 일어난다.
#### 1) 외부 단편화
- **프로세스의 크기 > 단편화 크기**인 경우, 해당 공간이 비었음에도 불구하고 프로세스가 공간을 차지(적재)하지 못하여 놀게 되는 메모리 공간을 의미한다.
- 어떤 프로세스도 차지하지 못한 빈 공간이지만, 사용할 수 없다.
- ex) 동적 메모리 할당 해제를 자주하는 경우
#### 2) 내부 단편화
- **프로세스의 크기 < 단편화 크기**인 경우, 해당 공간에 프로세스를 적재한 후 남게 되는 메모리 공간을 의미한다.
- 프로세스가 차지한 공간 내부에서 발생하는 사용하지 않는 작은 공간이다.
- ex) 배열을 크게 잡은 경우, Paging 기법에서 page 크기를 크게 잡을 경우
<br><br>

### 2. Paging
![paging](https://user-images.githubusercontent.com/34755287/54821888-d9191700-4ce6-11e9-8b11-7af6fdbcbe06.png)
- 프로세스의 주소 공간을 **고정된 사이즈의 페이지**로 나눠, 물리적 메모리에  불연속적으로 저장하는 방식
- 물리 메모리는 **Frame**이라는 고정 크기로 분할된다.
- 프로세스가 점유하는 논리 메모리(가상 메모리)는 **Page**라는 고정 크기로 분할된다.
- 사용자 프로그램과 CPU는 논리적 주소만을 다룰 뿐, 실제 물리적 주소는 알지 못한다.
- 메모리상에서 여러 곳에 흩어진 프로세스를 수행하기 위해, **MMU**를 통해 논리 주소와 물리 주소를 나눠 사용한다.
- 즉, 실제 메모리 공간은 연속적이지 않는데, CPU는 연속적으로 사용하고 있다고 믿으며(CPU를 속이며) 정상적으로 프로세스를 수행한다.
#### 1) MMU (Memory Management unit)
- CPU core 안에 위치하여 가상 주소를 실제 메모리 주소로 변환해주는 하드웨어 장치
- 프로세스를 실행하기 위해서 논리 주소(가상 주소)를 물리주소로 바꿔줘야 하는데, 이 mapping 작업에 사용되는 장치가 MMU이다.
- Page Table을 사용하여, frame과 page를 연결하는 page mapping 작업을 진행한다.
- 논리 주소
  - 프로세스가 메모리에 올라올 때까지, 메모리의 어떤 주소에 위치하게 될지 알 수 없다.
  - 따라서, CPU가 메모리 접근을 위해 임의의 연속된 메모리를 할당해주는 주소를 의미한다.
- 물리 주소
  - CPU가 실제 메모리에 접근하려면, 실제 메모리 주소를 알아야한다.
  - 따라서, 논리 주소와 매핑된 실제 메모리의 주소가 필요하다. 이것이 물리 주소이다.
#### 2) Page Table
- 프로세스 1개 당 1개씩 존재한다.
- 배열 구조로, **배열의 index는 가상 주소 값**을 의미하고 **그 배열 안의 값은 실제 주소의 페이지 번호**를 의미한다.
#### 3) 장점
- 논리 메모리는 물리 메모리에 저장될 때, 연속되어 저장될 필요가 없다.
- 논리 주소 공간 전체를 실제 메모리에 모두 올릴 필요가 없다.
- 물리 메모리의 남는 프레임에 적절히 배치됨으로써 외부 단편화를 해결할 수 있다.<br>(왜? 논리적 주소 공간과 메모리 주소 공간이 같은 크기의 페이지 단위로 나누어지기 때문)
#### 4) 단점
- 페이지 단위에 항상 fit하게 나눠지지 않으므로, 프로세스의 주소 공간 중 제일 마지막에 위치한 페이지에서는 내부 단편화 문제가 발생할 수 있다.
- mapping 과정이 추가되기 때문에 오버헤드가 발생한다.
<br><br>

### 3. Segmentation
- 프로세스를 서로 크기가 다른 논리적인 블록 단위인 **세그먼트**(**Segment**)로 분할하고, 메모리에 저장하는 방식
- 세그먼트는 주소 공간을 기능 단위 또는 의미 단위로 나눈 것을 의미한다.
  - 일반적으로 code, data, stack 등의 기능 단위로 세그먼트를 정의한다.
- Segment Table을 사용하여, page mapping 작업을 진행한다.
#### 1) Segment Table
- 논리적 주소가 (segment number, offset)으로 저장된다.
  - segment number: 논리적 주소가 몇 번째 세그먼트에 속하는지를 의미
  - offset: 그 세그먼트 내에서 논리적 주소가 얼마만큼 떨어져있는지를 의미
- 세그먼트 길이가 다르기 때문에, 세그먼트의 기준점(base) 정보 뿐만 아니라 세그먼트의 길이(limit)도 저장하고 있어야한다.
- 즉, 테이블의 항목으로 base와 limit을 가지고 있다.
  - base: 실제 메모리에서의 세그먼트 시작 위치를 의미
  - limit: 그 세그먼트의 길이를 의미
- limit의 크기를 넘는 주소 값이 들어오면, CPU에서 인터럽트가 발생시켜 해당 프로세스를 강제로 종료시킨다.
#### 2) 장점
- 메모리에 적재될 때 빈공간을 찾아 할당하므로, 내부 단편화를 해결할 수 있다.
- 세그먼트는 의미 단위이기 때문에 공유와 보안에 있어 효과적이다.
#### 3) 단점
- 서로 다른 크기의 세그먼트들이 적재, 제거되는 일이 반복되면, 프로세스의 크기가 더 큰 메모리 공간이 생길 수 있다.<br>즉, 공간이 비었음에도 불구하고 프로세스가 적재하지 못하여 놀게 되는 메모리 공간이 생기는 외부 단편화 문제가 발생할 수 있다.
- 프로세스를 의미 단위인 세그먼트로 관리하기 때문에, 크기가 다른 세그먼트들을 메모리에 적재할 때 오버헤드가 발생한다.
<br><br>

### 4. Paged Segmentation
- Paging 기법과 Segmentation 기법의 장점만을 취하는 주소 변환 기법
  - segmentation: 보호와 공유에 효율적
  - paging: 외부 단편화 문제 해결
- 프로세스를 논리적인 블록 단위인 세그먼트로 나눈다. 하지만, 세그먼트는 반드시 동일한 크기의 페이지들의 집합으로 구성된다.
- 물리적 메모리에 적재하는 단위는 page 단위이다.
- 주소 변환을 위해, 두 단계로 외부의 segment table과 내부의 page table을 사용한다.
  - 하나의 세그먼트가 여러 개의 페이지로 구성되므로, 각 세그먼트마다 페이지 테이블을 가진다.
#### 1) 장점
- 세그먼트 크기를 페이지 크기의 배수가 되도록 하여, 외부 단편화 문제를 해결한다.
- 세그먼트 단위로 공유, 보안이 이루어지도록 한다.
#### 2) 단점
- 세그먼트와 페이지가 동시에 존재하기 때문에 주소 변환도 2번 해야한다.
  - CPU -> segment table -> page table -> Main memory
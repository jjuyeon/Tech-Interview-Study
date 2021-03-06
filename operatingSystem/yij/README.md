### :notebook_with_decorative_cover: Process & Thread
1. 프로세스와 스레드의 차이를 설명하세요.

> 프로세스는 프로그램의 실행 단위를 의미합니다. 
> 프로세스는 다음의 요소들로 이루어져 있습니다.
> - 프로세스 주소 공간: 코드(프로그램 코드 저장), 힙(동적 할당 데이터를 저장), 스택(지역 변수, 실행중인 함수 저장), 데이터(전역 변수 등을 저장) 영역
> - 하드웨어 문맥: Register 상태, PC(Program Counter) 등 저장
> - PCB: 프로세스 메타 데이터 저장소(CPU 스케줄링 시 이용되는 프로세스 정보 저장. ex. ID, state, priority, cpu register 등)
> 
> 쓰레드는 프로세스 내에 존재하는 독립된 실행 단위를 의미합니다. 기본적으로 한 프로세스 내에 한 개 이상의 쓰레드가 존재합니다. 같은 프로세스 내에 있는 쓰레드들은 코드, 힙, 데이터 영역을 공유하며 별도의 스택 영역을 가지고 있습니다.
> 
> 쓰레드는 자원을 공유하기 때문에 프로세스보다 반응성이 좋고, 자원을 절약할 수 있습니다. 하지만 공유 자원의 접근을 제한하지 않으면 일관성을 해칠 수 있습니다.

1. 멀티 프로세스와 멀티 스레드의 차이를 설명하세요.

> 멀티 프로세스는 여러 개의 CPU를 설치해서 한 개 이상의 프로세스를 병렬 실행 시키는 것을 의미합니다.
> - 장점: 각각이 독립된 데이터 공간을 가져 서로의 영역을 침범하지 않습니다.
> - 단점: 작업량이 많을수록 오버헤드가 발생하며 프로세스 교체로 인한 Context Switching이 발생할 수 있습니다.
> 
> 멀티 쓰레드는 한 프로세스 내에서 여러 쓰레드를 구성해서 각 쓰레드가 병렬 작업을 수행하는 것을 의미합니다. 
> - 장점: 멀티 프로세스에 비해 메모리를 절약할 수 있으며 쓰레드 교체로 인한 시간 및 자원을 아낄 수 있다.
> - 단점: 공유 자원의 배타적 사용을 보장하지 않으면 안정성 문제가 발생할 수 있다.
> 

<br>

### :notebook_with_decorative_cover: System call
1. 시스템 콜에 대해 설명해주세요.

> 운영체제의 특성 상 커널 / 응용 프로그램이 CPU에 대해 갖게 되는 권한 및 접근 능력이 다릅니다.
> 시스템콜은 응용 프로그램이 운영체제의 커널이 제공하는 서비스에 접근하기 위해 사용하는 인터페이스입니다.
> 직접 시스템을 호출할 수 없기 때문에 고급 API를 통해 시스템 호출에 접근하게 하는 방법입니다.
> 
> 시스템콜의 기능
> 1. 사용자 모드에 있는 응용 프로그램이 커널의 기능을 사용할 수 있다.
> 2. 시스템콜을 호출하면 사용자 모드에서 커널 모드로 바뀐다.
> 3. 커널에서 시스템콜을 처리한 뒤에는 커널 모드에서 다시 사용자 모드로 전환되어 작업을 지속할 수 있다. 

2. 프로세스가 종료되는 두 가지 조건에 대해 설명해주세요.

> 프로세스는 exit() 시스템콜을 이용해 프로세스가 마지막 명령을 수행한 후 종료되는 자발적 종료, abort() 시스템콜을 이용해 부모 프로세스가 자식 프로세스의 수행을 종료시키는 비자발적 종류로 구분됩니다.
> 
> exit()
> - 자식 프로세스에서 마지막 statement를 수행한 후 exit() 시스템콜을 통해 종료될 수 있다.
> - 명시적으로 적지 않아도 main 함수가 return 되는 위치에 컴파일러가 삽입한다.
>
> abort()
> - child가 한계치 이상의 자원을 요구했거나 child에게 할당된 작업이 더 이상 필요하지 않을 경우 수행한다.
> - 키보드로 kill, break 등을 킨 경우 수행한다.
> - 부모 프로세스를 종료하는 경우에는 child 먼저 종료시킨 후 종료한다

### :notebook_with_decorative_cover: CPU Scheduling
1. 자신이 알고 있는 CPU 스케줄링을 선점형, 비선점형으로 나누어 특징을 설명해주세요.

> 비선점형 CPU 스케줄링(프로세스가 자발적으로 CPU를 반납해야만 종료)
> - FCFS(First-Come-First-Serve): job이 도착한 순서대로 수행하는 방식
> - SJF(Shortest-Job-First): CPU Burst Time이 짧은 순서대로 job을 수행하는 방식
>
> 선점형 CPU 스케줄링(우선순위가 더 높은 프로세스가 도착한다면 강제적으로 현재 수행중이던 프로세스를 종료한 뒤 해당 프로세스에게 CPU 권한 위임)
> - SRJF(Shortest-Remaining-Job-First): SJF의 선점형 스케줄링 방식
> - RR(Round-Robin): time quantum을 두어 해당 quantum 내에 task를 전부 수행하지 않으면 그 프로세스의 수행을 종료시키고 ready queue로 돌려보내는 방식

2. 1에서 설명한 각 CPU 스케줄링의 단점을 설명해주세요.

> FCFS: 한 프로세스가 CPU를 오랫동안 독점하는 경우 평균 응답 시간이 길어짐
> SJF: CPU Burst Time을 예측하기 힘듦
> SRJF: CPU Burst Time이 긴 프로세스는 CPU 권한을 평생 얻지 못할 수 있음(Starvation)
> RR: time quantum이 너무 클 경우 FCFS가 되고, 너무 짧으면 context switching이 자주 발생해 오버헤드가 크다 

<br>

### :notebook_with_decorative_cover: 동기화 & 비동기화
1. 동기와 비동기의 차이(블로킹, 넌블로킹) 를 장단점과 함께 설명해주세요.

> 개념적 이해
> - 동기: 작업을 수행하는 두 개 이상의 주체가 (1)서로 동시에 시작하거나, (2)동시에 끝나거나, (3)끝나는 동시에 시작할 때
> - 비동기: 두 주체가 서로의 시작, 종료 시간과는 상관 없이 별도의 수행 시작&종료 시간을 가지고 있을 때
> 
> 함수적 이해
> 
> - 동기: 함수를 호출하고 호출된 함수의 작업이 완료된 후의 반환을 기다리는 방식
> - 비동기: 함수를 호출할 때 callback 함수를 같이 전달해 작업이 완료되면 callback을 실행하는 방식

2. 교착상태(데드락)란 무엇이며, 교착상태가 발생하는 조건을 설명해주세요.

> 둘 이상의 프로세스가 공유 자원을 선점한 상태에서 상대방의 자원을 무한 대기하는 현상.
> 
> 교착상태 발생 4가지 조건
> 1. Mutual Exclusion: 매 순간 하나의 프로세스만이 자원을 선점할 수 있다
> 2. No Preemption: 프로세스는 스스로 자원을 내놓기 전까지는 강제로 빼앗기지 않는다
> 3. Hold and Wait: 자원을 가진 프로세스가 추가 자원을 기다릴 때 보유한 자원을 포기하지 않는다
> 4. Circular Wait: 자원을 기다리는 프로세스들 간에 사이클이 형성된다

3. 세마포어와 뮤텍스의 차이에 대해 설명해주세요.

> 세마포어: 공유된 자원의 데이터에 여러 **프로세스**가 동시 접근하는 것을 제한하는 병렬 프로그래밍 방식
> 뮤텍스: 공유된 자원의 데이터에 여러 **쓰레드**가 동시 접근하는 것을 제한하는 병렬 프로그래밍 방식


### :notebook_with_decorative_cover: Paging & Segmentation
1. 페이징과 세그먼테이션에 대해서 설명하세요.

> 페이징: 프로세스를 일정한 크기(페이지)로 분할하여 메모리에 noncontiguous하게 저장하는 기법. 메모리도 일정한 크기인 프레임으로 분할하고, 페이지와 프레임의 크기는 같다. 페이지가 존재하는 실제 물리 메모리 주소를 저장하기 위해 매핑 테이블인 **페이지 테이블**을 이용한다.
> 세그먼테이션: 프로세스를 의미적 단위(주로 code, stack, heap 영역)로 분할하여 메모리에 noncontiguous하게 저장하는 기법. 세그먼트마다 크기가 다르기 때문에 물리 메모리에 저장되는 주소인 **base**와 세그먼트의 길이인 **limit**으로 이루어져 있음.

2. 내부 단편화와 외부 단편화가 무엇인지 설명하세요.

> 내부 단편화: 메모리 영역을 일정한 크기로 나누어 프로세스를 저장할 때, 할당된 메모리 영역보다 프로세스의 크기가 작아 잉여 공간이 남게 되는 현상.
> 외부 단편화: 적재하려는 프로세스 크기보다 메모리 영역의 크기가 작아서 프로세스를 적재할 수 없는 현상. 메모리 영역을 할당한 후 해제할 때 주로 발생.

### :notebook_with_decorative_cover: Page replacement
1. 페이지 부재(page fault)가 무엇인지 설명해주세요.
2. 페이지 교체란 무엇인지 설명해주세요.
3. 페이지 교체 알고리즘의 종류와 각각의 특징에 대해 설명해주세요.

### :notebook_with_decorative_cover: Memory Management

1. 운영체제의 메모리 관리 전략인 Contiguous Allocation, Noncontiguous Allocation의 차이점을 설명해주세요.

> ##### 연속 할당
> : 프로세스를 메모리에 올릴 때, 각각의 프로세스를 연속된 메모리 공간에 적재하는 것.
> ex) Fixed partition allocation, Valiable partition allocation
> ##### 동적 할당
> : 하나의 프로세스를 메모리의 여러 영역에 분산시켜 적재하는 것.
> ex) Paging, Segmentation, Paged Segmentation

2. Swapping이란?

> 현재 사용하지 않는 프로세스를 일시적으로 ```메모리 -> backing store```로 쫓아내는 기법.
> 
> priority-based CPU 스케줄링 알고리즘을 사용할 시, priority가 낮은 프로세스를 swap out / 높은 프로세스를 swap in 한다
> 
> compile time 혹은 load time binding을 사용하면 원래 위치로 swap in 해야하며, execution time binding에서는 추후 빈 메모리 영역 아무 곳에나 올릴 수 있다 ❗

   - Swapping 시 발생할 수 있는 문제점?

> swap-out 하는 과정(메모리에서 backing store로 프로세스를 쫓아낼 때) 메모리 영역 해제로 인한 **External Fragmentation**이 발생할 수 있다

   - 외부 단편화를 해소할 수 있는 방법 두 가지 제시

> 1. compaction(압축) 기법을 사용하여 적재된 각강의 메모리 영역, Hole들을 연속된 메모리로 이동시킨다 (하지만 비용이 너무 크다느 문제점이 있다)
> 2. Noncontiguous Allocation 기법인 **페이징**을 사용한다. (하지만 내부 단편화는 발생할 수 있음)

<br>

### :notebook_with_decorative_cover: Vertual Memory
1. 가상 메모리가 필요한 이유를 하는 일과 관련지어 설명해주세요.
2. 요구 페이징이란 무엇이고, 요구 페이징에서 Page Fault가 발생했을 때, 처리되는 Page 교체 순서에 대해 설명해주세요.
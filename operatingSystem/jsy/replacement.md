# :question: Page replacement

#### reference
https://baked-corn.tistory.com/20<br>
https://preamtree.tistory.com/21<br>
https://goodmilktea.tistory.com/37<br>
https://velog.io/@injoon2019/%EC%9A%B4%EC%98%81%EC%B2%B4%EC%A0%9C-%EC%9A%B4%EC%98%81%EC%B2%B4%EC%A0%9C-%EB%B0%98%ED%9A%A8%EA%B2%BD-%EA%B5%90%EC%88%98%EB%8B%98-2017%EB%85%84-11.-%EA%B0%80%EC%83%81-%EB%A9%94%EB%AA%A8%EB%A6%AC
<hr>

## Question
1. [페이지 부재(page fault)가 무엇인지 설명해주세요.](#1-page-fault)
- 페이지 부재란 가상 메모리를 사용할 때 발생하는 문제 상황으로, 지금 실행시켜야 할 페이지가 실제 물리 메모리에 올라와있지 않은 것을 의미합니다.
<br><br>

2. [페이지 교체란 무엇인지 설명해주세요.](#2-page-replacement)
- 프로세스가 특정 페이지를 요구할 때, 실제 메모리에 해당 페이지가 없고 빈 프레임이 존재하지 않는 상황에 발생하는 작업입니다.
- 즉, 메모리 상에 존재하는 프레임 중 하나를 선택하여, 이를 가상 메모리에 저장하고 필요한 페이지를 물리 메모리에 로드하는 작업을 '페이지 교체'라고 합니다.
<br><br>

3. [페이지 교체 알고리즘의 종류와 각각의 특징에 대해 설명해주세요.](#3-page-replacement-algorithm)
- FIFO 알고리즘은 가장 간단한 페이지 교체 알고리즘으로, 물리 메모리에 적재된지 가장 오래된 페이지를 교체 대상 페이지로 선택하는 알고리즘입니다.
- OPT 알고리즘은 앞으로 가장 오랫동안 사용하지 않을 페이지를 교체 페이지로 선택하는 알고리즘입니다.
- LRU 알고리즘은 최근에 사용하지 않은 페이지를 선택하는 알고리즘으로, 페이지 교체 알고리즘 중 가장 많이 쓰입니다.
- LFU 알고리즘은 페이지의 참조 횟수가 가장 적은 페이지를 교체 대상 페이지로 선택하는 알고리즘입니다.
<hr>

## :nerd_face:	What I study
### 1. Page fault
- 가상 메모리를 사용할 때 발생하는 문제
  - 가상 메모리를 사용하면, 당장 실행에 필요한 부분만 실제 메모리에 올라가게 된다. <br>ex) 크기를 크게 잡아둔 배열의 경우, 사용하는 크기만큼만 실제 메모리에 올라가게 된다.
  - 올라왔던 페이지가 사용 후에도 자리만 차지하고 있을 수 있다. -> 메모리가 가득찰 수 있다.
  - 추가로 페이지를 가져오기 위해서, 안쓰는 페이지는 메모리에서 빼고, 필요한 페이지를 넣을 필요가 있다.
- 여기서, Page fault는 지금 실행시켜야 할 page가 실제 물리 메모리에 올라와있지 않은 것을 의미한다.
- page fault로 인한 I/O 작업은 CPU의 효율성을 낮춘다.
#### page fault 발생시, 요구 페이징(Demanding Paging) 과정이 실행된다.
![fault](https://t1.daumcdn.net/cfile/tistory/99CE723359CA2F1E28)
``` 
1. CPU는 trap을 발생시켜 운영체제에게 알린다. 
2. 운영체제는 CPU의 동작을 잠시 멈춘다.
3. 운영체제는 page table을 확인하여, 가상 메모리(Disk)에 페이지가 존재하는지 확인하고 없으면 프로세스를 종료한다.
4. 현재 물리 메모리에 비어있는 프레임(free frame)이 있는지 찾는다.
5. 비어있는 프레임이 없다면, 페이지 교체 알고리즘을 사용하여 공간을 만든다.
6. 비어있는 프레임에 해당 페이지를 로드하고, page table을 업데이트한다.
7. 중단되었던 CPU를 다시 시작한다.
```
<br><br>

### 2. Page replacement
- page fault가 발생했을 때, 물리 메모리에 비어있는 프레임이 없을 수 있다.
- 이 경우, 희생할 프레임을 선택하여 이를 가상 메모리(디스크)에 저장하고(out) 필요한 페이지를 물리 메모리에 로드하는(in) 작업이 필요하다.
  - 위의 작업을 **page replacement**라고 한다.
  - 어떤 페이지를 물리 메모리에서 out시킬지 선택하기 위해 사용하는 알고리즘이 **page replacement 알고리즘**이다.
- **page fault를 적게 발생시키는 것이 목표이다.**
  - 가까운 미래에 참조될 가능성이 가장 적은 페이지를 선택하여 메모리에서 내보낸다.
- 수정이 되지 않는 페이지를 선택하는 것이 좋다.<br>(왜? 페이지가 수정되면 메인 메모리에서 내보낼 때, 하드디스크에서 또 수정을 해야하므로 시간이 오래걸리기 때문)
#### 1) 교체 범위
- 교체 대상이 될 프레임의 범위를 정하는 방법
- 전역교체 방법이 더 효율적이다. <br>(왜? 자기 프로세스 페이지에서만 교체를 하면, 교체를 해야할 때 각각 모두 교체를 진행해야 하므로 비효율적이다.)

1. 전역교체 (Global replacement)
- 메모리 상의 모든 페이지 프레임이 교체 대상이 될 수 있다.
- 프레임 수가 변하지 않는다.
- 전체 메모리를 각 프로세스가 공유해서 사용하고, 교체 알고리즘을 통해 할당되는 메모리 양이 변한다.
2. 지역교체 (Local replacement)
- 현재 수행 중인 프로세스에게 할당된 프레임(자기 프로세스 페이지) 안에서만 교체 대상이 될 수 있다.
- 프레임 수가 변할 수 있다.
- 프로세스마다 페이지 프레임을 미리 할당하는 것을 전제로 한다.
<br><br>

### 3. Page replacement algorithm
#### 1) FIFO(First in First Out) algorithm
![fifo](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FbLsNsm%2FbtqALPbW6jc%2FtCEU5GIGEGeEQHQc6tpAGK%2Fimg.jpg)
- 물리 메모리에 적재된지 가장 오래된 페이지를 선택하는 알고리즘
- 가장 간단한 방법으로, 초기화 코드에서 사용하기 적절하다.
- 참조 가능성을 고려하지 않고, 순서에 따라서만 내보낼 페이지를 선택하기 때문에 비효율적이다.
- page frame 수가 증가해도 page fault가 많이 발생할 수 있다.
  - 일반적으로는 page frame이 많아지면 메모리가 증가하므로, page fault가 줄어든다.
- ex) 초기화 코드는 처음 프로세스가 실행될 때, 최초 초기화를 시키는 역할만 진행한다. 그러므로 메인 메모리에서 빠져도 괜찮다.
- -> 처음 프로세스 실행시에는 반드시 필요하므로, 초기화를 시켜준 후 가장 먼저 내보낸다.
#### 2) OPT(Optinal Page Replacement) algorithm
![opt](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FcwOEfq%2FbtqALPC0rpi%2FXkkRr3cxkU5n4FBEW5KO5k%2Fimg.jpg)
- 앞으로 가장 오랫동안 사용하지 않을 페이지를 선택하는 알고리즘
- 선택한 페이지가 앞으로도 잘 사용되지 않을 것이라는 보장이 없으므로, 현실적으로 구현이 어렵다.
  - 모든 프로세스의 메모리 참조 계획을 미리 파악할 수 없다.
- page fault 발생률이 가장 낮은 효율적인 알고리즘이다.
#### 3) LRU(Least Recently Used) algorithm
![lru](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Ftvbud%2FbtqAK4gnKVD%2FhWAva1ykKaHL7wolK7ybDK%2Fimg.jpg)
- 최근에 사용하지 않은(가장 오랫동안 사용하지 않은) 페이지를 선택하는 알고리즘
- 최근에 사용하지 않으면, 앞으로도 사용하지 않을 가능성이 높다.<br>(최근에 사용했으면 조만간 다시 사용될 것이다.)
- OPT의 초점이 '미래'라면 LRU의 초점은 '과거'이므로, 현실적으로 구현이 가능하다.
- FIFO보다 page fault가 발생하는 횟수가 적다. <- 실질적으로 구현할 수 있는 알고리즘에서 최적의 방법 중 하나이다.
- 페이지 최근 참조 시간을 소프트웨어적으로 계속 저장하고 비교해야 하므로, 오버헤드가 발생한다.
#### 4) LFU(Least Frequently Used) algorithm
- 페이지의 참조 횟수로 교체할 페이지를 선택하는 알고리즘이다.
- 많이 사용되는 페이지는 참조 횟수가 많아질 것이다.
- 물리 메모리에 존재하는 페이지 중에서 과거에 참조 횟수가 가장 적었던 페이지를 선택하여 내보내고, 그 자리에 새로 참조될 페이지를 적재한다.
- 시간에 따른 페이지 참조 변화를 반영하지 못한다.
  - 어떤 페이지를 초기에 많이 사용하다가 이후에 사용하지 않게 되어도, 참조 횟수가 많았기 때문에 계속 메모리에 머물게 된다. (초기 가정에 어긋남)
- 페이지 참조 횟수를 소프트웨어적으로 계속 저장하고 비교해야 하므로, 오버헤드가 발생한다.
- LRU보다 구현이 복잡하다.
#### 5) Clock algorithm
= Second chance algorithm = NUR (Not Used Recently) 또는 NRU(Not Recently Used)
- 하드웨어의 지원을 받아 LRU, LFU 알고리즘에서 발생하는 오버헤드를 줄인 방식
- 최근에 사용하지 않은(오랫동안 사용하지 않은) **페이지 중 하나**를 선택하는 알고리즘
  - LRU의 근사 알고리즘 (LRU-approximation)
  - 교체되는 페이지의 참조 시점이 가장 오래되었다는 것을 보장하지 않는다.
  - cf) LRU는 **가장** 오랫동안 사용하지 않은 페이지를 선택하는 알고리즘.
- LRU, LFU에 비해 페이지의 관리가 훨씬 빠르고 효율적이다.
- 대부분의 시스템에서 페이지 교체 알고리즘으로 체택하고 있다.
![clock](https://media.vlpt.us/images/injoon2019/post/f5b61c32-90e0-4e4f-a89b-34fdd05c555a/image.png)
- page frame이 선형으로 배치되는 것과 달리 원형 배치가 되어 있다.
- 사각형 안 비트(0,1)는 reference bit(access bit)이다. 
  - 0이면 최근에 사용되지 않음을 의미, 1이면 최근에 사용되었음을 의미한다.
  - 각 프레임마다 하나씩 존재한다.
- reference bit를 순차적으로 조사하면서 교체 대상 페이지를 선택한다.
```
1. 프레임 내의 페이지가 참조될 때, reference bit가 1로 설정한다.
2. reference bit가 0인 것을 찾을 때까지 포인터를 하나씩 앞으로 이동한다.
3. 포인터를 이동하는 중에 reference bit가 1인 것은 모두 0으로 바꾼다.
4. 모든 페이지 프레임을 다 조사한 경우, 첫 번째 페이지 프레임부터 작업을 반복한다.
즉, 포인터가 참조된 이후에 한바퀴는 프레임이 살아남을 수 있다. (second chance)
5. reference bit가 0인 것을 찾으면 그 페이지를 교체한다. (swap-out)
- 0은 한 바퀴를 도는 동안 다시 참조되지 않았음을 의미한다. 그래서 이때 페이지 프레임을 교체한다. (자주 사용되는 페이지라면, reference bit가 1로 세팅되어 있어 교체되지 않는다.)
```
- read가 발생했을 때는 reference bit만 1로 만들고, write가 발생하면 (I/O를 동반하는 페이지) reference bit, modified bit 둘 다 1로 만든다. 
- modified bit이 1이면 페이지가 수정된 것이므로, 메모리에서 내보낼 때 disk에 쓰고 내보낸다.
- 적어도 한 바퀴를 도는 시간만큼은 페이지를 메모리에 유지시킬수 있기 때문에, page fault가 줄어들게 된다. (<- second chance)
- 비트를 1로 바꾸는 것은 hardware가 하고, 0으로 만드는 것은 운영체제가 한다. 
```
cf) 
reference bit
  - 페이지가 참조될 때, 1로 자동 세팅된다.
  - 페이지가 참조되지 않았을 때는 0이 되며, 페이지를 교체한다.
modified bit
  - 페이지 내용이 변경되었을 때, 1로 세팅된다.
  - 페이지 내용이 변경되지 않으면, 0으로 세팅된다.
```
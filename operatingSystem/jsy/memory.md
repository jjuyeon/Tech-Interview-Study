# :question: Memory Management

#### reference
https://github.com/WooVictory/Ready-For-Tech-Interview/blob/master/Operating%20System/%EB%8B%A8%ED%8E%B8%ED%99%94.md<br>
https://goodmilktea.tistory.com/31
<hr>

## Question
1. [운영체제의 메모리 관리 전략인 연속 할당, 동적 할당의 차이점을 설명해주세요.](#1-allocation-of-physical-memory)
- 연속 할당은 각각의 프로세스를 물리적 메모리의 **연속적인 공간에 할당**하는 방식입니다.
- 반면, 동적 할당은 하나의 프로세스를 물리적 메모리의 **여러 영역에 분산하여 할당**하는 방식입니다. 동적 할당의 종류로는 paging, segmentation 기법이 있습니다.
- ***추가답변)*** 연속 할당은 두 가지 방식으로 또 나눌 수 있는데, 1) 메모리 공간을 고정된 개수만큼 미리 분할해두는 방식인 고정 분할 방식과 2) 프로세스의 크기에 따라 분할 크기 및 개수가 변하는 방식인 가변 분할 방식이 있습니다.
<br><br>

2. [Swapping이란 무엇이며, Swapping시 발생할 수 있는 문제점을 설명해주세요.](#2-swapping)
- 메모리 관리를 위해 사용되는 기법 중 하나입니다. 메모리에 새로운 프로세스를 불러올 공간이 충분하지 않다면 메모리에 있는 프로세스를 디스크의 swap area에 일시적으로 내려보내고(swap out), 원하는 프로세스를 올리는(swap in) 방식입니다.
- swapping 시, 메모리와 디스크 사이의 context-switching이 많이 발생하기 때문에 오버헤드가 커집니다. 또한, 메모리에 올라와 있는 프로세스가 완전히 idle이 되었을 때 swapping을 해야하므로, 현재 프로세스의 상태를 확인해야하는 추가 작업이 필요합니다.
<br><br>

3. [외부 단편화를 해소할 수 있는 방법을 두 가지 이상 설명해주세요.](#3-외부-단편화-문제를-해결하는-방법)
- 메모리 압축 방법과 페이징, 세그멘테이션 기법이 있습니다.
- 메모리 압축 방법은 연속 메모리 할당을 진행하면서, 프로세스가 사용하는 공간들을 한쪽으로 몰아 하나의 큰 가용 공간을 확보하는 방법입니다. 이 방법은 작업시간이 길기 때문에 효율적이지 않습니다.
- 페이징, 세그멘테이션 기법은 압축 방법보다 효율적인 방법입니다. 한 번에 할당되는 크기를 여러 개로 분할하여 비연속적인 가용 공간에 프로세스들이 적재될 수 있도록 하는 방법입니다.
<hr>

## :nerd_face:	What I study
### 1. Allocation of physical memory
#### 1) 연속 할당
- 각각의 프로세스를 물리적 메모리의 연속적인 공간에 할당하는 방식
    1. 고정 분할 방식
       - 물리적 메모리를 주어진 개수만큼 영구적인 분할로 미리 나눈다.
       - 각 분할에 하나의 프로세스를 적재하여 실행시킨다.
       - 동시에 메모리에 올릴 수 있는 프로세스의 수가 고정되어 있다.
       - 메모리에서 수행할 수 있는 프로세스의 최대 크기가 제한된다.
       - **논리적 주소 > 분할된 메모리**이면, 외부 단편화 문제가 발생한다.
       - **논리적 주소 < 분할된 메모리**이면, 내부 단편화 문제가 발생한다.
    2. 가변 분할 방식
       - 메모리에 적재되는 프로그램의 크기에 따라 분할의 크기, 개수가 동적으로 변한다.
       - 일부러 '분할 크기 > 프로그램의 크기'로 할당하지 않기 때문에, 내부 단편화 문제는 발생하지 않는다.
       - 외부 단편화 문제가 발생할 수 있다.
       - 위의 외부 단편화 문제를 해결하기 위해, [Compaction](#3-압축-compaction)이라는 방법을 사용한다.
       ```
       <새로 들어갈 프로세스가 메모리 내 가용 공간 중 어디에 할당되어야 할지 결정하는 방법>
       - 일반적으로 First-fit과 Best-fit이 Worst-fit 보다 성능이 좋음

       1. 최초 적합(First-fit)
       - 요청한 크기(n)를 만족하는 첫 번째 가용 공간을 할당하는 방법
       - 검색의 시작점은 알고리즘에 따라 다르며, 가용 공간을 모두 탐색하지 않으므로 검색 속도가 빠르다.
       2. 최적 적합(Best-fit)
       - 요청한 크기(n)를 만족하는 가용 공간 중에서 가장 작은 공간을 할당하는 방법
       - 분할되어 남는 가용 공간들이 많아질 수는 있으나, 그 크기를 최소화하여 공간적으로 효율적이다.
       - 가용 공간들의 리스트가 정렬되어 있지 않은 경우, 모든 가용 공간 리스트를 탐색해야해서 시간적으로 오버헤드가 발생한다.
       3. 최악 적합(Worst-fit)
       - 가장 큰 가용 공간을 할당하는 방법
       - 모든 가용 공간 리스트를 탐색해야해서 검색 속도가 느리다.
       - 분할되어 남는 가용 공간이 커서 활용 가능성이 높다.
       - 상대적으로 더 큰 프로그램을 담을 수 있는 가용 공간이 빨리 사라지기 때문에, 메모리 이용 효율이 좋지 않다.
       ```
#### 2) 동적 할당
- 하나의 프로세스를 물리적 메모리의 여러 영역에 분산하여 할당하는 방식
    1. Paging
       - 프로세스의 주소 공간을 **고정된 사이즈의 페이지**로 나눠, 물리적 메모리에 불연속적으로 저장한다.
       - 내부 단편화 문제가 발생할 수 있다.
    2. Segmentation
       - 프로세스를 서로 크기가 다른 논리적인 블록 단위인 **세그먼트**(**Segment**)로 분할하고, 메모리에 저장한다.
       - 외부 단편화 문제가 발생할 수 있다.
    3. Paged Segmentation
       - 세그멘테이션을 기본으로 하되, 이를 다시 동일 크기의 페이지로 나누어 메모리에 저장한다.
       - paging 과 segmentation의 장점을 합친 방식이다.
#### 3) 압축 (Compaction)
- 외부 단편화를 해결하기 위해 프로세스가 사용하는 공간들을 한쪽으로 몰아, 하나의 큰 가용 공간을 확보한다.
- 프로세스의 메모리 위치를 상당 부분 이동시켜야 하므로, 작업 효율이 좋지 않다.
- 그러므로, 프로세스가 실행되는 중간에 동적으로 압축 방식을 진행하는 것은 효율적이지 않다.
<br><br>

### 2. Swapping
- 메모리를 관리하기 위해 사용되는 기법
- **메모리에 올라온 프로세스의 주소 공간 전체**를 디스크의 swap area(backing store)에 일시적으로 내려놓는 것
  - 프로세스가 실행되기 위해서는 메모리에 있어야 하지만, 프로세스는 실행 중에 임시로 예비저장장치(backing store)에 내보내어졌다가 실행을 계속하기 위해 다시 메모리로 돌아올 수 있다.
  - 프로세스가 종료되어 그 주소 공간을 디스크로 내보내는 것이 아니다.
  - **CPU 할당 시간이 끝난 프로세스(ready, waiting상태)를 디스크로 내보내고 다른 프로세스를 메모리에 올린다.**
![swapping](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FbjcOad%2FbtqAKiki52t%2FH1pMQi4JAo3QAdVaHqSVW1%2Fimg.png)
  ```
  1. 많은 프로세스가 동시에 메모리에 올라오면, 프로세스당 할당되는 메모리의 양이 줄어들기 때문에 시스템 전체 성능에 영향을 미친다.
  2. 이런 문제를 해결하기 위해, swapping을 통해 몇몇 프로세스의 주소 공간 전체를 디스크의 swap area에 보냄으로써(swap out) 적절한 메모리 공간을 유지한다.
  3. 이후에 메모리에 있는 프로세스가 충분히 실행되고 나면, 해당 프로세스를 내보낸다.
  4. 그리고 그 자리에 swap out 되었던 프로세스를 다시 메모리에 올린다. (swap in)
  ```
- 즉, Swapping은 ***메모리에 존재하는 프로세스의 수를 조절하는*** 중요한 역할을 한다.
#### 1) swap area(backing store)
- 디스크 내에 파일 시스템과는 별도로 존재하는 일정 영역
- 프로세스가 실행중인 동안만 디스크에 일시적으로 저장하는 공간
- 저장기간이 상대적으로 짧다.
- 다수의 프로세스를 담을 수 있을 만큼 충분히 큰 공간이어야 한다.
- 어느 정도의 접근 속도가 보장되어야 한다.
#### 2) 작업 종류
1. swap in : 디스크 -> 메모리
2. swap out : 메모리 -> 디스크

### 3. 외부 단편화 문제를 해결하는 방법
- 메모리 공간에 낭비를 초래하는 심각한 문제이기 때문에, 연속 메모리 할당은 간단한 방법이지만 실제 운영체제에 그대로 적용할 수 없다.
1. [메모리 압축](#3-압축-compaction)
- 연속 메모리 할당을 계속 진행하면서 발생한 짜투리 가용 공간들을 하나로 합쳐, 큰 가용 메모리 공간을 만든다.
2. [페이징 기법 & 세그멘테이션 기법](../jsy/paging.md)
- 메모리 압축 방식보다 더 효율적인 방법이다.
- 한 프로세스의 논리 주소 공간을 여러 개로 분할하여 비연속적인 물리 메모리 공간에 할당한다.
- 한 번에 할당되는 크기가 여러 개로 분할하기 때문에, 외부 단편화 문제를 어느 정도 해결할 수 있다.
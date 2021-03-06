# :question: GIT

#### reference
https://escapefromcoding.tistory.com/281<br>
https://codevang.tistory.com/217
<hr>

## Question
1. Git을 쓰는 이유는 무엇인지, Git과 GitHub의 차이점에 대해 설명해주세요
- Git을 사용하는 이유는 로컬저장소를 이용한 빠른 퍼포먼스와 브랜치를 통한 효율적인 협업을 하기 위함입니다.
- Git은 클라우드가 아닌 로컬 시스템에 설치되어 코드와 이력을 기록하고 관리하도록 돕는 버전관리 시스템이라면, GitHub는 클라우드 기반으로 Git 저장소를 관리해주는 호스팅 서비스입니다. 그래서 GitHub는 개인의 Git 저장소에 원격으로 접근이 가능하다는 차이점을 가지고 있습니다. 
<hr>

## :nerd_face:	What I study

![vs](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FcqwWj3%2FbtqCckCDdUD%2FLVMvU0EOrwQhLF6owuA88k%2Fimg.png)

### 1. Git
- 코드와 그 이력을 기록하고 관리하도록 돕는 **버전관리 시스템**
- 클라우드가 아닌 **로컬 시스템에 설치**되고 관리된다.
  - 속도가 빠르고 로컬에 부담없이 커밋할 수 있다.
  - 원격저장소가 터지더라도 로컬저장소를 통해 복구가 가능하다.
- 진행하는 프로그래밍 버전 기록을 스스로 관리할 수 있다.
- 사용자는 본인의 코드에서 또 다른 독립적인 로컬 브랜치를 만들 수 있다.
  - 브랜치를 만든 후 작업하고, 이전 브랜치로 다시 복구하고 쉽게 삭제, 병합이 가능하다.
- 다른 개발자가 실시간으로 작업하는 내용을 알 수 없다.
  - 팀원들이 프로젝트의 같은 부분을 수정해도 서로의 작업을 확인할 수 없으므로, conflict가 날 가능성이 있다.

<br>

### 2. Github
- Git 저장소를 관리하는 **클라우드 기반** 호스팅 서비스
- 개인의 로컬 서버 밖에서 Git 버전 프로젝트를 공유하고 기록하는 온라인 데이터베이스
- 자체 구축이 아닌 빌려쓰는 클라우드 개념을 사용한다.
- **클라우드 기반이기 때문에, 개인의 Git 저장소에 원격으로 접근이 가능하다.**
- 다른 개발자와 코드 공유가 가능하다.
- 실시간으로 하나의 프로젝트에 전체 팀원이 함께 협력할 수 있다.
- 만약 개개인이 브랜치를 push/pull 하지 않는다면 다른 사용자 저장소의 중심 디렉토리에 반영되지 않는다.
## Merge

- Merge는 branch들을 병합하는 것
- commit history로 같이 병합된다

  ![git-merge](https://user-images.githubusercontent.com/43779730/139853647-7378fc43-6b57-4fbb-afa3-8d0fb854de6a.png)

## Rebase

- Merge처럼 두 branch를 합칠 때 사용
- 현재 branch가 대상 branch위로 올라감
- Merge commit이 남지 않아 하나의 흐름으로 유지가 됨
- 위험하니깐 조심스럽게 사용하여야함
- commit history에 변경이 발생

  ![git-rebase](https://user-images.githubusercontent.com/43779730/139853853-6224c5e1-d0fd-40b5-ae68-226b991a3028.png)

- rebase는 엄밀하게 이동이 아니라 내부에서 노드 변경점의 역추적을 통한 `복제`가 이뤄집니다.

## Rebase의 활용

- Master 브랜치 이외의 브랜치에서 파생된 브랜치가 있는 경우

  ![git-rebase01](https://user-images.githubusercontent.com/43779730/139853995-be9602d8-3071-4313-ad9b-a6417c0f76f8.png)

- 변경된 부분만 가져오기 때문에 파생된 브랜치의 부분만 가져올 수 있다.
  ![git-rebase02](https://user-images.githubusercontent.com/43779730/139854016-c8945b43-fd4d-4bf2-91c3-97363a2e2d83.png)

- 로컬 작업물을 원격 저장소의 최신 변경 사항으로 업데이트 하려 할 때 사용
  - 원격 저장소의 커밋 히스토리를 어지럽히지 않을 수 있음

## Rebase 주의점

- 원격 저장소에 push된 branch는 rebase를 하지 않도록 한다

- 원격 저장소에서 팀원의 branch와 Merge된 Master Branch를 Pull 한다고 가정했을 때, 다음과 같은 상태가 될 것이다.

  ![git-rebase03](https://user-images.githubusercontent.com/43779730/139856981-2e1807b9-0ee2-4bed-a9d2-7a6ef0341a22.png)

- 만약의 팀원이 merge를 취소하고 Rebase 하게 된다면 어떻게 될까?

  ![git-rebase04](https://user-images.githubusercontent.com/43779730/139857168-ba281165-586b-469f-97f0-929ed59b6032.png)

- A4와 A3`는 실질적으로 똑같은 Commit을 나타내지만, 불필요하게 중복된 Commit 내용을 가져오게 된다.
- 이는 히스토리에 똑같은 Commit 내용이 발생하게 되고 혼란을 야기한다.

## Cherry-pick

- 특정한 Commit 하나만 골라서 현재 branch에 추가해준다.

  ![git-cherrypick](https://user-images.githubusercontent.com/43779730/139857739-ebdf8dea-3014-46f9-9c20-4b78db598b9b.png)

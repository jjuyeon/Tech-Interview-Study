# :question: Stack, Queue, Tree, Heap

#### reference
https://devuna.tistory.com/22<br>
https://techhan.github.io/study/interview-01/<br>
https://devowen.com/213<br>
https://velog.io/@vagabondms/DFS-vs-BFS
<hr>

## Question
1. 'Stack, Queue, Tree, Heap'의 특징을 설명해주세요.
- 스택: 쌓아 올리는 자료구조로, 먼저 넣게 되는 자료가 마지막으로 나오게 되는 후입선출(FILO) 구조입니다. 같은 구조와 크기의 자료를 정해진 방향으로 쌓을 수 있고, 한 방향으로만 접근할 수 있다는 특징이 있습니다.
- 큐: 원소의 줄을 세우는 자료구조로, 먼저 넣게 되는 자료가 가장 먼저 나오는 선입선출(FIFO) 구조입니다. 한 쪽 끝에서 삽입 작업을, 다른 쪽 끝에서 삭제 작업을 진행한다는 특징이 있습니다. 
- 트리: 정점과 간선을 이용해 사이클을 이루지 않도록 구성한 그래프의 특수한 형태로, 계층이 있는 데이터를 표현하기에 적합합니다. 순환구조를 가지지 않고, 부모노드에서 자식노드로 향하는 단방향만 가능하다는 특징이 있습니다.
- 힙: 최댓값 또는 최솟값을 찾아내는 연산을 쉽게 하기 위해 고안된 구조로, 각 노드의 키값이 자식의 키값보다 작지 않거나(최대힙) 그 자식의 키값보다 크지 않은(최소힙) 완전이진트리 자료구조입니다.
<hr>

## :nerd_face:	What I study
### 1. Stack
- 후입선출(LIFO)의 자료구조를 구현한 클래스
- 나중에 넣은 객체가 먼저 빠져나간다.
```
push() : 주어진 객체를 스택에 넣는다.
peek() : 스택의 맨 위 객체를 반환하고, 객체를 스택에서 제거하지 않는다.
pop() : 스택의 맨 위 객체를 반환하고, 객체를 스택에서 제거한다.
empty() : 스택이 비었다면 true를 반환하고, 그렇지 않다면 false를 반환한다.
search() : 스택에서 주어진 객체를 찾아서 그 위치의 인덱스를 반환한다. (배열과는 달리 1부터 시작한다.)
```

<br><br>

### 2. Queue
- 선입선출(FIFO)의 자료구조를 구현한 인터페이스
- 먼저 넣은 객체가 먼저 빠져나간다.
- 주로 데이터가 입력된 시간 순서대로 처리되어야 하는 경우 사용한다. 
```
offer() : 큐의 맨 뒤에 전달된 요소를 저장한다. 성공 시 true, 실패 시 false를 반환한다.
poll() : 큐의 맨 앞에 있는 객체를 반환하고, 객체를 큐에서 제거한다. (큐가 비어있을 경우 null 반환)
peek() : 큐의 맨 앞에 있는 객체를 반환하고, 객체를 큐에서 제거하지 않는다. (큐가 비어있을 경우 null 반환)

add() : 큐의 맨 뒤에 전달된 요소를 저장한다. 저장이 성공하면 true, 실패하면 Exception을 발생시킨다.
remove() : 큐의 맨 앞에 있는 요소를 제거한다. 큐가 비어있을 경우 Exception을 발생시킨다.
element() : 삭제 없이 큐의 맨 앞에 있는 요소를 반환한다. 큐가 비어있을 경우 Exception을 발생시킨다.
```

<br><br>

### 3. Tree
![tree](https://blog.kakaocdn.net/dn/c6gPs6/btqyLgjcwwB/2WVkOfT7VVqa6gnE4Sk25K/img.gif)
- **재귀로 정의된 자기 참조 자료구조**
  - 트리는 자식도 트리이고, 그 자식도 트리이다.
  - 여러 개의 트리가 쌓여 큰 트리가 된다.
- 일반적으로 level은 0에서 시작한다.
- 각 노드가 자식을 M개 이하로 가지고 있으면 M-ary 트리라고 부른다.
  - 이진트리는 **2개 이하로 가지고 있기 때문에** Binary Tree라고 부른다.
  - 포화 이진트리는 **리프노드를 제외한 모든 노드가 2개의 자식**을 가지고 있으며, **모든 리프노드가 동일한 레벨**을 가지고 있다.
  - 완전 이진트리는 마지막 레벨을 제외하고 모든 레벨이 완전히 채워져있으며, 마지막 레벨의 모든 노드는 **가장 왼쪽부터** 채워져 있다.

#### 3-1) 트리 vs 그래프
- 트리는 순환구조를 갖지 않는 그래프이다!
  - (그래프는 순환구조를 갖는다.)
- 트리는 부모노드에서 자식노드로 향하는 단방향뿐이다.
  - (그래프는 부모노드에서 자식노드, 자식노드에서 부모노드로 양방향이 가능하다.)

#### 3-2) 인접 행렬 vs 인접 리스트
- 완전 이진 트리의 경우 인접 행렬로 표현해도 괜찮지만, 그렇지 않은 경우에는 **메모리 낭비가 발생**할 수 있기 때문에 인접리스트로 구현하는 것이 좋다.

<br><br>

### 4. Heap
![heap](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FDsmcW%2FbtqyLJSZA24%2FFP4cAljKoVv7Kcti04VlY1%2Fimg.png)
- **완전 이진 트리에 있는 노드** 중에서 **키 값이 가장 크거나 작은 노드**를 찾기 위해 만들어진 자료구조
- 루트에 가장 크거나, 작은 값이 들어가 있다.
- 삽입의 경우, 새로운 키 값이 들어왔을 때 맨 뒤에 삽입을 하고 부모노드와의 비교 후 swap을 한다.
- 삭제의 경우, 마지막 인덱스의 값을 루트로 올린 다음 자식들과 비교 후 swap을 해준다.
  - 최대 힙이면 자식들 중 큰 값이 올라오고, 최소 힙이면 자식들 중 작은 값이 올라온다.
- 시간복잡도
  - 데이터 삽입/삭제 시: O(logN)
  - 최솟값, 최댓값 검색 시: O(1)
- ex) PriorityQueue

<br><br>

### 5. DFS vs BFS
![bruteForce](https://media.vlpt.us/images/vagabondms/post/037c243c-108a-49c6-ae6a-abb3673532ca/image.png)

||DFS|BFS|
|:---:|:---:|:---:|
|목적|모든 노드 방문|최단 경로, 임의의 경로 탐색|
|구현|재귀, 스택|큐|

<br>

#### 5-1) DFS (Depth First Search)
- 모든 노드를 방문하고자 할 때 사용한다.
- 재귀 또는 자료구조 stack을 이용하여 구현한다.
- 장점
  - BFS에 비해 저장공간의 필요성이 적다. 백트래킹을 해야하는 노드들만 저장해주면 된다.
  - 찾아야하는 노드가 깊은 단계에 있을수록, 그 노드가 좌측에 있을수록 BFS보다 유리하다.
- 단점
  - 답이 아닌 경로가 매우 깊다면, 그 경로에 깊이 빠질 우려가 있다.
  - 찾은 해가 최단 경로라는 보장이 없다.

```
1. 시작 정점을 방문 한 후, 자식을 모두 탐색한다.
2. 연결된 노드가 존재하지 않을 때까지 왔다면, 한 단계 이전으로 돌아가 다시 알고리즘을 수행한다.
```

```java
stack.push(root);

while(!stack.isEmpty()) {
    Node node = stack.pop();
    visit(node);
    stack.push(node's child);
}
```

```java
// 전위 순회: 부모 -> 좌 -> 우
preorder(node) {
    if node is None
        return;

    print(node.val);
    preorder(node.left);
    preorder(node.right);
}
```

```java
// 중위 순회: 좌 -> 부모 -> 우
inorder(node) {
    if node is None
        return;

    inorder(node.left);
    print(node.val);
    inorder(node.right);
}
```

```java
// 후위 순회: 좌 -> 우 -> 부모
postorder(node) {
    if node is None
        return;

    postorder(node.left);
    postorder(node.right);
    print(node.val);
}
```

#### 5-2) BFS (Breathed First Search)
- 두 노드 사이의 **최단 경로** 혹은 **임의의 경로**를 찾고 싶을 때 사용한다.
- 자료구조 **queue**를 이용하여 구현한다.
- 장점
  - 너비를 우선으로 탐색하기 때문에 최단 경로가 보장된다.
  - 최단 경로가 존재한다면, 어느 한 경로가 무한히 깊어진다고 해도 최단 경로를 반드시 찾을 수 있다.
  - 노드 수가 적고, 깊이가 얕은 해가 존재할 때 유리하다.
- 단점
  - 큐를 이용해 다음에 탐색할 노드를 저장하기 때문에, 노드의 수가 많을수록 필요없는 노드들까지 저장해야 한다. DFS보다 더 큰 저장공간이 필요하다.
  - 노드의 수가 늘어나면 탐색해야하는 노드가 많아지기 때문에 비효율적이다.

```
1. 시작 정점을 방문 한 후, 시작 정점에 인접한 모든 정점들을 우선 방문한다.
2. 더 이상 방문 할 정점이 없으면, 한 depth 내려가서 다시 인접한 모든 정점들을 우선 방문한다.
```

```java
queue.offer(root);

while(!queue.isEmpty()) {
    Node node = queue.poll();
    visit(node);
    queue.offer(node's child);
}
```
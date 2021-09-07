> 'Stack, Queue, Tree, Heap'의 특징을 설명해주세요.

# Stack

**LIFO(Last In First Out)의 구조**를 가진 선형 자료구조

ADT(Abstract Data Type) Stack은 5개의 연산을 지원한다.

1. create: 스택 생성
2. isEmpty: element가 있는지 검사
3. push: element를 삽입
4. pop: 제일 마지막에 삽입된 element를 제거
5. top: 제일 마지막에 삽입된 element 반환

위의 연산들은 Stack의 꼭 필요한 연산을 추상화한 것 이기 때문에 Programming Language에 따라 추가적인 연산을 더 지원할 수 있다. 하지만 Stack의 가장 중요한 성질인 LIFO를 위배하는 연산은 수행될 수 없다.

> Q) Stack이지만 중간 Item에도 접근하고 싶다! 이런 자료구조를 구현하면 안 되나?
> A) 자료구조를 구현하는 것은 본인의 마음이지만 특성 상 top에만 접근할 수 있는 것이 Stack의 정의이다. 그러니 Stack이라고 볼 수 없다.

<br>

### Stack C Code

<details>
<summary>코드 보기</summary>


``` c
typedef struct data_type {
	int num;
}data;

typedef struct {
	data* items;
	int capacity;
	int top;
}Stack;

void create(Stack* stack) {
	stack->capacity = 2;
	stack->top = -1;
	stack->items = (data*)malloc(sizeof(data) * stack->capacity);
}

bool isEmpty(Stack* stack) {
	if (stack->top == -1)return true;
	return false;
}

data top(Stack* stack) {
	return stack->items[stack->top];
}

void push(Stack* stack, data item) {
	stack->top++;
	if (stack->top == stack->capacity) {
		stack->capacity *= 2;
		stack->items = (data*)realloc(stack->items, sizeof(data) * (stack->capacity));
	}
	stack->items[stack->top] = item;
}

data* pop(Stack* stack) {
	if (isEmpty(stack)) {
		printf("Stack is already empty.\n");
		return NULL;
	}

	data* item = &stack->items[stack->top];
	stack->top--;
	return item;
}
```


</details>

<br>

### Stack의 예시

1. 함수의 Call Stack
![image](https://user-images.githubusercontent.com/30489264/132243628-cd1f1684-a41a-4420-9ea2-853ba9c1aa7a.png)

main함수에서는 func1함수를 호출 -> func1함수에서 func2함수를 호출

-> func2함수에서 값을 반환 -> func1함수에서 반환된 값에서 추가 연산을 한 뒤 값을 반환 -> main함수의 변수 대입

2. 웹 브라우저의 뒤로 가기 버튼

<br>

# Queue

**FIFO(First In First Out)의 구조**를 가진 선형 자료구조

ADT Queue는 Stack과 다르게 **front**(제일 처음으로 삽입된 원소), **rear**(제일 나중에 삽입된 원소)의 두 가지 포인터가 존재한다.

1. create: 큐 생성
2. isEmpty: element가 있는지 검사
3. enqueue: **rear**에 element 삽입
4. dequeue: **front**에서 element 삭제
5. peek: front에 있는 item 반환

Stack과 마찬가지로 ADT Queue의 성질에 위배되는 연산은 할 수 없다. 
Queue는 두 가지 방법으로 구현이 가능하다.

## Linear Array를 이용한 Queue 구현

: 일반적인 배열을 선언하고 front, rear 포인터를 두는 방식. 

<details>
<summary>코드 보기</summary>

``` c
typedef struct queue{
  int front;
  int rear;
  int item[MAX_ITEM];
}Queue;

void create(Queue* q){
  q->rear = -1;
  q->front = -1;
}

int isEmpty(Queue* q){
  int empty = 0;

  if(q->front == q->rear){
    empty = 1;
  }
  
  return empty;
}

void enqueue(Queue* q, int new_item){
  if(q->rear == MAX_ITEM - 1){
    printf("Queue Overflow");
    return;
  }

  q->item[++(q->rear)]=new_item;
}

int dequeue(Queue* q){
  if(isEmpty(q)){
    printf("Queue is Empty");
    return;
  }

  return q->item[++(q->front)];
}
```

</details>

<br>

> 💡 단점? 
> 연산을 반복할 수록 front, rear는 계속 오른쪽으로 이동한다
> Array의 Size는 한계가 있고, 빈 공간이 있음에도 낭비하게 된다
> 💡 삭제할 때 마다 elements들을 왼쪽으로 sorting 시켜주면 안 되나?
> 원소의 개수를 N개라 할 때, 삽입 시 마다 O(N)의 시간을 소모하게 됨.

<br>

## Circular Array를 이용한 Queue 구현

: Linear Array의 한계를 극복하기 위한 구현 방법. 물리적으로 Array라는 것은 똑같으나 front, rear 포인터가 순환한다

<details>
<summary>코드 보기</summary>

``` c
typedef struct queue{
  int front;
  int rear;
  int count;
  int item[MAX_ITEM];
}Queue;

void create(Queue* q){
  q->rear = 0;
  q->front = 0;
  q->count = 0;
}

int isEmpty(Queue* q){
  int empty = 0;

  if(q->count){
    empty = 1;
  }
  
  return empty;
}

void enqueue(Queue* q, int new_item){
  if(q->count == MAX_ITEM){
    printf("Queue Overflow");
    return;
  }

  q->item[q->rear]=new_item;
  q->rear = (q->rear + 1) % MAX_ITEM;
  q->count++;
}

int dequeue(Queue* q){
  if(isEmpty(q)){
    printf("Queue is Empty");
    return;
  }

  int value = q->item[q->front];
  q->front = (q->front + 1) % MAX_ITEM;
  q->count--;

  return value;
}
```

</details>

Queue의 최대 size가 고정되어 있으므로 그 이상의 element를 관리할 수는 없지만, 재배치할 필요가 없다. 모든 연산은 O(1)에 가능하다.

<br>

# Tree

우리는 Computer Science에서 트리가 뭔가요?라고 한다면

![image](https://user-images.githubusercontent.com/30489264/132245788-3253ac27-35c2-4be5-9ae7-57958b912153.png)

이러한 그림으로 대답하는 경우가 있다.
이것은 정확하게는 Rooted Tree(루트가 존재하는 트리)라고 한다.

정확한 트리의 정의는 **Connected + Acyclic의 성질을 가진 그래프의 형태**를 트리라고 한다. 

<br>

![image](https://user-images.githubusercontent.com/30489264/132268868-d368235c-edda-42d2-a162-fb45e7ffced6.png)

1. Connected: Tree 상의 모든 Node는 연결되어 있어야 한다.
2. Acyclic: 특정 Node에서 자기 자신으로 가는 경로는 존재하지 않는다.

<br>

흔히 구현하는 트리는 Rooted Tree를 의미하며, 간단한 일반적 정의와 Recursive한 정의로 나눌 수 있다.

<br>

## Rooted Tree의 Non-Recursive 정의

: 데이터를 지닌 Node와, Node들을 연결해주는 Directed Edge의 집합으로 이루어진다. Edge의 방향은 Parent Node에서 Child Node로 향한다. Tree 상의 모든 Node K에 대해, Root에서 K로의 유일한 Path가 존재한다.

- Root Node: In-degree가 0인 Node
- Leaf Node: Child Node가 없는 Node
- Sibling: 같은 부모 노드를 가진 Node들의 관계
- Ancestor: Node u에서 Node v로의 Path가 존재할 경우, u는 v의 Ancestor이다.
- Decendant: Node u에서 Node v로의 Path가 존재할 경우, v는 u의 Decendant이다.

## Rooted Tree의 Recursive 정의

- Base Case: Single Node는 Tree이다.
- Recursive Step: 1️⃣ Root + 0개 이상의 Subtree와 2️⃣ Root에서 각각의 Subtree들의 Root로 향하는 Edge는 👉Tree이다.

## Tree 용어

![image](https://user-images.githubusercontent.com/30489264/132269884-15d6b05a-a992-49f4-8874-c604939a8665.png)
![image](https://user-images.githubusercontent.com/30489264/132269875-00ffde67-f14b-4fd6-aeae-f73447174635.png)

|용어|설명|
|-|-|
|Degree|해당 Node의 Child 수|
|Height|해당 Node에서 도달할 수 있는 아무 Leaf Node까지 가장 긴 Path의 길이|
|Depth|Root에서 해당 Node까지의 Path의 길이|
|Level|Depth + 1|

트리는 계층적인 구조를 나타내는 Non-Linear Data Structure로 활용할 수 있다. 특히, 그 모든 Node의 degree가 최대 N인 N-ary Tree 중에서도 Binary Tree를 많이 사용한다.
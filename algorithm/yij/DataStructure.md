> ❔) 'Stack, Queue, Tree, Heap'의 특징을 설명해주세요.
> 
> 💬
> Stack: LIFO 구조를 가진 선형 자료구조 입니다. top이라는 포인터를 가지며, 해당 포인터가 가리키는 위치에만 삽입/삭제/원소 반환이 가능합니다. 주로 함수의 콜 스택, 브라우저의 뒤로가기에서 사용합니다.
> Queue: FIFO 구조를 가진 선형 자료구조 입니다. front, rear라는 두 개의 포인터를 가지며 front에서 삭제/원소 반환, rear에서 삽입이 가능합니다. 주로 CPU 스케줄링에서 사용합니다.
> Tree: Connected + Acyclic(연결되어 있고, 사이클이 없는) 그래프의 한 종류입니다. 주로 루트를 가진 Rooted Tree로 많이 사용합니다. 이진 탐색 트리, 힙, 레드블랙 트리 등 다양한 자료구조들의 기본이 됩니다.
> Heap: Complete Binary Tree의 한 종류로, 각 parent가 자신의 child node의 value보다 작은지/큰지에 따라 Min-Heap인지 Max-Heap으로 분류할 수 있습니다. Priority Queue 혹은 Heap Sort에서 활용합니다.
> 
> ❔) Array와 LinkedList의 차이가 무엇인가요?
> 
> 💬 Array와 Linked List의 대표적인 차이는 **Size 고정 여부, 메모리 연속 할당 여부**입니다.
> Array는 선언 당시 Size가 고정되어 있고, 연속적으로 메모리가 할당되어 있습니다. 상대적으로 조회 속도는 빠르고, 삽입/삭제 속도는 느립니다.
> Linked List는 Size가 가변적이고, 불연속적으로 메모리에 할당되어 있습니다.
> 따라서 상대적으로 조회 속도는 느리지만 삽입/삭제 속도는 빠릅니다.


# Array, Linked List

Array와 Linked List는 각기 다른 구조, capacity 전략, 메모리 방식 등이 다르다.

## Array

- 선언 시 size가 고정되어 있다.
  - 기존 size보다 더 많이 저장하려면 새로운 배열을 할당해야 한다.
- 연속적인 메모리 공간에 할당된다.
  - N차원 배열이라도 연속적인 공간에 할당되기 때문에 시작 메모리 주소, index만 알면 빠르게 element의 위치를 알아낼 수 있다.
  - 👉 조회 시간 복잡도가 O(1)로 빠르다
- 위의 특성 때문에, Insert/Delete 연산에는 불리하다.
  - 배열의 중간 위치에 element를 삽입하거나 삭제하고 싶을 때에는, (메모리가 연속적이기 때문에) 뒤에 위치한 elements들을 전부 한 칸씩 한 칸씩 밀거나, 당겨야 한다.
  - 👉 삽입/삭제 시간 복잡도가 O(N)으로 느리다.

<br>

## Linked List

Linked List는 아이템들이 연결된 형태로 리스트를 관리하는 자료구조이다.
기본적으로 element를 저장할 때 item(value)를 저장하는 필드, 다음 Item의 위치를 가리키는 필드로 구성되는데 이를 **Node**라고 한다.

<br>

> 💡 면접 빈출 질문으로 Linked List의 연산에 대한 Pseudo Code에 대해 물어보는 경우가 있으니, 이는 꼭 알아두고 갑시다 ❗

<br>

### Linked List 종류

Doubly Circular Linked List도 있지만 Doubly와 Circular 특성을 합친 것 이기에 생략

||Singly-Linked-List|Doubly-Linked-List|Circular-Linked-List|
|-|-|-|-|
|Node 구조|value<br>next|value<br>next<br>prev|value<br>next<br>prev|
|Last Node의 next|NULL|NULL|HEAD|
|Linked List를 가리키는 포인터|HEAD|HEAD|LAST|

<br>

### Linked List 특징

- Linked List의 관리를 위해 HEAD(또는 LAST)의 포인터 하나의 정보만 가지고 있다.
  - 각각의 Node들이 독립적인 메모리 공간에 할당되고, 다음 Element의 주소에 대해서만 가지고 있다.
  - 👉 조회 시간복잡도가로 O(N)으로 느리다.
- 포인터라는 특성 덕분에, 삽입/삭제 연산은 빠르다.
  - 새로운 Node를 만들어서 Element를 가리키는 포인터들에 대해서만 변화를 준다.
  - 👉 삽입/삭제 시간복잡도가 O(1)로 빠르다.

<br>

## Array 🆚 Linked List

✅ Linked List는 Singly Linked List 가정

||Array|Linked List|
|-|-|-|
|Get|**O(1)**|O(N)|
|Insert At First|O(N)|**O(1)**|
|Insert At Middle|O(N)|O(N)|
|Insert At Last|O(N)|O(N)|
|Delete At First|O(N)|**O(1)**|
|Delete At Middle|O(N)|O(N)|
|Delete At Last|O(N)|O(N)|

👉 다룰 데이터의 size가 가변적이고 삽입/삭제가 빈번히 발생한다면 **Linked List**
👉 다룰 데이터의 size가 고정적이고 삽입/삭제의 빈도보다 조회/수정이 자주 발생한다면 **Array**
를 사용하는 것이 좋다.

<br>

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

## Stack을 이용한 Queue 구현

두 개의 Stack을 이용하면 Queue를 구현할 수 있습니다.

![image](https://user-images.githubusercontent.com/30489264/132338141-bf0f5d4e-aef0-432b-8684-6a13f66b92fa.png)

1. Enqueue 연산: 1번 큐에 Insert한다 - **O(1)**
2. Dequeue 연산
   - 2번 큐가 비었다면 1번 큐에 있는 모든 원소를 pop하면서 2번큐에 차례로 Insert한다 - **O(N)**
   - 2번 큐에 element가 있다면 2번 큐를 pop한다

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

<br>

# Heap

: **특정 property를 만족하는 Complete Binary Tree**. Heap의 Node들은 Value(이외의 값을 가질 수도 있지만 그러한 경우 comparable해야 한다)를 가지며, 대표적으로 **Min Heap과 Max Heap**이 존재한다.
주로 Priority Queue 혹은 Heap Sort를 구현하는 데 사용된다. Complete Binary Tree의 특성 상 중간에 낭비되는 공간이 없기 때문에, Linked List로 구현하기 보다는 Array로 구현하는 것이 훨씬 효율적이다.

<br>

<details>
<summary> Full/Perfect/Complete Binary Tree</summary>

> 💡 Complete Binary Tree?
> Binary Tree의 종류에는 Full, Perfect, Complete Binary Tree가 있다.
> 1. Full Binary Tree: 모든 Node는 0개 혹은 2개의 Child를 가진다.
> 2. Perfect Binary Tree: Full Binary Tree이면서 모든 Leaf Node의 Depth가 같다. (Level K에는 2^K개의 Node가 있다)
> 3. Complete Binary Tree: Node의 Child 개수에는 제한이 없지만, 노드들의 삽입 순서가 반드시 정렬되어 있어야 한다. 이 형태는 Tree의 Depth가 N이라고 할 때, N-1까지는 Perfect Binary 형태이면서 제일 Depth가 큰 Level에서는 왼쪽 -> 오른쪽의 순서대로 Node가 채워져 있어야 한다.
</details>

<br>

## Min/Max Heap

![image](https://user-images.githubusercontent.com/30489264/132321985-8c351803-deaf-4393-b562-6a429320b7d6.png)

Heap의 어떤 Node p에 대해, p의 Parent Node를 q라 하면 **q의 value는 p의 value보다 크다**

이러한 property를 만족하는 Heap을 Max-Heap이라 하며, 반대(q.value < p.value)인 Heap을 Min-Heap이라 한다.

<br>

## ADT Heap의 연산

Heap의 연산은 Min, Max Heap에 따라 달라지기 때문에 Max-Heap 기준으로 작성한다.

1️⃣ max value 반환
  - Heap의 root node의 value를 반환한다.
  - Heap에서 모든 child node의 value는 자신 parent node의 value보다 작다. 따라서 parent가 없는 root node의 value가 최대값임이 보장된다
  
2️⃣ Insert(value) - **O(log N)**

- Heap에 priority가 value인 node를 추가한다.


1. 우선 Heap의 맨 마지막 Node로 추가한다.
2. Heap의 Priority를 유지하기 위해 swap 과정을 진행해준다.
3. 마지막 node부터 시작해서 parent와 child의 priority를 비교해준 뒤, priority에 어긋난다면 두 node를 swap한다.
4. Heap 전체의 property가 성립할 때 까지 parent node로 이동하면서 재귀적으로 swap을 진행한다.

3️⃣ Delete() - **O(log N)**

- Heap의 Priority가 제일 큰 Node(Root Node)를 삭제한다.
- Heap의 성질을 유지하기 위해 swap 과정을 또 진행한다.

1. Root Node를 삭제하고 맨 마지막 Node를 Root Node의 자리로 이동시킨다.
2. 현재 Node p의 left, right child 중 priority가 큰 child를 선택한다
3. p보다 child의 priority가 크다면, p와 child를 swap한다.
4. 만약 교체했다면? 현재 node 포인터를 교체한 child로 이동한 뒤 2번으로 돌아가 재귀적으로 반복한다.
5. 교체하지 않았다면 종료한다.

<br>

## C에서의 Heap 구현

<details>
<summary>코드 보기</summary>

```c
#define MAX_SIZE (1<<5)
typedef struct heap {
	int array[MAX_SIZE];
	int cur;
}heap;

void create(heap* h) {
	h->cur = 0;
}

int isEmpty(heap* h) {
	if (h->cur > 0)return 0;
	return 1;
}

int findmax(heap* h) {
	return h->array[0];
}

void insert(heap* h, int item) {
	if (h->cur < MAX_SIZE) {
		h->array[h->cur] = item;

		int temp;
		int parent = (h->cur - 1) / 2;
		int index = h->cur;

		while (parent >= 0) {
			if (h->array[parent] < h->array[index]) {
				temp = h->array[parent];
				h->array[parent] = h->array[index];
				h->array[index] = temp;
				index = parent;
				parent = (index - 1) / 2;
			}
			else break;
		}
		h->cur++;
	}
}

int deletemax(heap* h) {
	if (isEmpty(h))return -1;
	int value = h->array[0];
	int new_root = h->array[h->cur - 1];
	h->array[h->cur - 1] = 0;
	h->array[0] = new_root;
	h->cur--;

	int index = 0, max;
	int left = 1,right=2,temp;
	while (left < h->cur || right < h->cur) {
		max = index;
		if (left < h->cur && (h->array[left] > h->array[max]))
			max = left;
		if (right < h->cur && (h->array[right] > h->array[max]))
			max = right;

		if (max != index) {
			temp = h->array[max];
			h->array[max] = h->array[index];
			h->array[index] = temp;

			index = max;
			left = index * 2 + 1; right = index * 2 + 2;
		}
		else break;
	}

	return value;
}
```

</details>

<br>

## Heap Sort

Priority가 가장 높은 value를 반환하는 Heap의 특성 상 Priority Queue를 구현하는 데에 많이 사용된다. (Priority Queue가 이름 때문에 선형 자료구조로 오해하는 경우가 있는데 헷갈리지 말자)
그 외에도, N개의 Node를 가진 Heap에서 Delete 연산을 N번 하면 Priority 순으로 value를 정렬할 수 있다 ❗

이것을 Heap Sort라 하고, 시간복잡도는 

    N개의 Node * Delete연산 = N * O(log N) = O(NlogN)

과 같다.

<details>
<summary>코드 보기</summary>

```c
int* heapSort(heap* h) {
	int* arr = (int*)malloc(sizeof(int) * (h->cur+1));
	int index = 0;
	while (h->cur != 0) {
		arr[index] = deletemax(h);
		index++;
	}
	return arr;
}
```

</details>

<br>

Heap Sort는 Worst-Case에서도 O(NlogN)의 시간복잡도를 가지기 때문에 시간복잡도 측면에서는 매우 우수하지만, Heap을 생성하기 위한 추가 공간을 필요로 한다(이를 **Out-Of-Place Algorithm**이라 한다). 

Heapify 연산을 이용하면 추가 array를 생성하지 않고서도 정렬을 할 수 있다.

<br>

## Heapify

(Max-Heap 기준)모든 Non-Leaf Node인 p에 대해, Level Order의 역순으로 아래의 과정을 거친다.
Node p와 p의 child node 중 큰 값을 가진 Node에 대해, Heap Property를 만족할 때 까지 Swap & 이동(child node로)을 반복한다.

### Heapify Algorithm

배열 이름을 A라 가정, Max-Heap 기준

1️⃣ A를 완전 이진 트리의 순서에 맞게 트리에 대응하여 생성한다
2️⃣ **Level Order의 역순** 3️⃣, 4️⃣번 과정을 반복하며 탐색을 진행한다(index: N/2 - 1 부터 0까지)
3️⃣ 현재 Node p의 두 child 중 큰 값을 가진 Node가 Heap property를 만족하지 않는다면, p와 해당 p의 child를 swap한다
4️⃣ 3번에서 swap했다면, 현재 Node를 가리키는 포인터를 Swap한 child 위치로 이동한 뒤 3번의 과정을 다시 수행한다.

1️⃣~4️⃣번을 수행하고 난 뒤 A는 Heap이 된다.

5️⃣ Heap에서 Delete 연산을 N번 진행한다
6️⃣ 현재 index를 i라 할 때, Delete하여 얻어낸 Max 값을 A[n-i]에 덮어씌운다 

5️⃣, 6️⃣번을 수행하고 나면 배열은 오름차순으로 정렬된다(Min-Heap의 경우 내림차순으로 정렬).

<br>

> 2️⃣번에서 Level Order의 역순으로 진행하는 이유는 뭘까?
> Heapify 연산 중 3️⃣, 4️⃣ 번을 반복하며 포인터가 가리키는 Level은 계속 증가한다.
> 만약 Level Order 순으로 진행한다면 Swap이 제대로 진행될 수 없다.
> 
> ![image](https://user-images.githubusercontent.com/30489264/132333914-7ee1f7d4-8d57-4961-bae5-37bbc8f5fa69.png)
> 이 예시에서, 맨 처음 7과 2를 비교하고 난 뒤 마지막에 2와 8을 swap하기 때문에 Heap Property가 성립하지 않는다.
> 
> Heapify 연산을 하면서 특정 Node에서 Depth가 증가하는 순으로 비교를 하기 때문에 **Root Node는 Swap될 기회가 1번 밖에 없다**.
> Level Order 순으로 진행한다면 나중에 진짜 Max 값이 위로 올라오더라도 Swap될 수 없다. 따라서 Level Order 순으로 진행하면서 높은 Level에 존재하는 Max Value들을 한 칸씩 위로 올리는 순서로 진행해야 한다.

<br>

# Binary Search Tree

: Tree 자료구조를 이용해서 Binary Search를 할 수 있도록 고안된 자료구조. Binary Search Tree의 각 node는 key를 가지고 있으며, key값 k를 가지는 non-leaf node p에 대해 **p의 left subtree 상의 모든 key값은 항상 p보다 작으며, p의 right subtree 상의 모든 key 값은 항상 p보다 크다**.

<br>

> Q) 왜 Array가 아닌 Tree로 만들었을까?
> A) 특정 element에 대해 자주 검색할 경우, 검색 빈도 수가 높아질 수록 탐색하는 우선순위를 높게 하면 시간을 아낄 수 있음 -> Optimal Binary Search Tree
> A) Array에 Insert, Delete를 하는 것은 O(N)시간이 소모됨

<br>

- get(key): tree traversal을 이용하여 현재 node의 key값이  찾으려는 key값보다 작다면 left child로, 크다면 right child로 이동한다. 일치하는 key값을 찾아내거나 NULL node로 이동할 때 까지 반복. **- O(h)**
- insert(key): get연산과 유사하게 node를 이동한 뒤, leaf node에 도달하면 해당 node의 child에 key값을 가진 새로운 node를 left/right child로 삽입 **- O(h)**
- delete(key): 해당 node가 leaf node/1개의 child/2개의 child를 가질 때에 따라 나뉜다. **- O(h)**
  1. leaf node일 때: 해당 node를 삭제한다
  2. 1개의 child를 가지고 있을 때: 해당 child를 현재 node의 자리로 대체하고 기존 node를 삭제한다.
  3. 2개의 child를 가지고 있을 때: left subtree의 right most descendant를 node의 자리로 대체하고 기존 node를 삭제한다.

<br>

![image](https://user-images.githubusercontent.com/30489264/133288747-4c0eabb1-b284-4005-9474-c21f932a3771.png)

> 💡 2개의 child node를 가진 node를 삭제하려고 할 때의 원리
> 👉 삭제하려는 node를 p, p의 left subtree에서 right most descendant를 u라고 하자.
> 이 때, u의 child는 최대 1개이다(right most node이기 때문에 해당 node의 right child는 존재할 수 없다). 
> 따라서 p를 u로 대체하고, (재귀적)1개의 node를 삭제할 때 처럼 u node를 삭제하면 된다.

<br>

**<u> Binary Search Tree의 연산 시간복잡도는 Tree의 Height로 정해진다 ❗❗ </u>**

![image](https://user-images.githubusercontent.com/30489264/133289796-daa9e81d-f04e-43b0-9c65-441ebc2c52e8.png)


하지만 Tree가 위의 그림과 같은 편향 이진 트리(Skewed Binary Tree)일 경우, 전체 element 개수에 비해 연산의 시간복잡도는 증가한다.
Tree가 **Complete Binary Tree일 때 시간복잡도가 O(log N)인 것**에 비해 최악의 경우 연결리스트의 형태를 한 **Skewed Binary Tree일 경우에는 시간 복잡도가 O(N)수렴**에 한다.

Binary Search Tree의 경우 Insert, Delete 연산이 반복되면 Balanced 성질이 깨질 수 있다. 이를 보완하기 위한 자료구조들로는 대표적으로 AVL Tree, Red-Black Tree가 있다.

<br>

# AVL Tree

![image](https://user-images.githubusercontent.com/30489264/133293230-036c9af6-986e-4058-b25e-d9d826209af8.png)
(출처: https://casterian.net/archives/217)

: (AVL은 사람 이름) Tree 상의 모든 node p에 대해 **p의 left subtree와 right subtree의 height 차이가 최대 1**을 만족하는 Binary Search Tree

위의 성질을 Balanced Property라고 함 ❗❗

Balanced Property에 기반한 AVL Tree의 최대 Height는(N개의 Node 기준) 최대 **2log n**이다.

Binary Search Tree와 비교했을 때, get연산은 완전히 동일하지만 Insert, Delete 연산을 수행한 뒤 Balanced Property를 유지하기 위한 추가 작업을 진행한다.

<br>

## Maintaining Balance in AVL Tree

![image](https://user-images.githubusercontent.com/30489264/133293811-37027a8d-6e88-47be-a6dc-690090b9ff91.png)

위의 그림처럼 12라는 Key를 가진 Node를 추가하면, **해당 Node를 자손으로 가지는 Node들의 Height가 변한다**.

이 때,
- w: 새로 추가한 node
- z: w의 ancestor 중 balanced property를 만족하지 않으면서 height가 최소인 node
- y: z의 두 child 중 height가 큰 child node(heavy child)
- x: z의 두 child 중 height가 큰 child node(heavy child)

라고 정의하자.

Tree의 모양은 다음과 같은 4가지가 나올 수 있다.

![image](https://user-images.githubusercontent.com/30489264/133294676-54494053-c047-4a39-a517-e074e600cb53.png)

### Case 1, 2의 경우

y를 중심으로 해서 Tree를 회전한다.

Case1의 경우

- y의 left subtree T2를 임시로 저장한다.
- y의 left child를 z로 연결한다.
- z의 right child를 T2로 연결한다.

Case2의 경우에는 Case1과 동일하지만 반대 방향으로 진행하면 된다.

### Case 3, 4의 경우

Case3의 경우

- y를 기준으로 right rotate한다.
- case1과 동일한 모양이 되어, case1의 rotate연산을 수행한다.

Case4의 경우도 마찬가지다.

(그림 추가 예정)

<br>

# Minimum Spanning Tree

: **(Weighted) Connected Graph G**가 주어졌을 때, 다음을 만족하는 *G의 부분 그래프*를 말한다.

G' = (V(G), T) (G의 Vertex Set과 일치하는 Vertex Set을 갖는다)

1️⃣ G'은 **Connected**하다
2️⃣ G'을 포함하는 Edge의 Cost 합이 (모든 경우의 수 중) 최소가 되어야 한다

<br>

> Q) G의 부분 그래프인데 왜 Tree인가?
> A) Tree의 정의는 Connected & Acyclic Graph이다. 이 때, 2번 조건에서 Edge Cost 합이 최소가 되는 Sub Graph는 Cycle을 가질 수 없다 ❗❗
> 만약 G'의 edge T를 선택하는 과정에서 cycle이 생긴하면, 해당 cycle에 속한 edge 중 아무런 edge 하나만 제거해도 Connected 성질을 만족하기 때문에 cost 합은 줄어든다. 따라서 2번 성질을 만족하면 자동으로 Acyclic Graph가 되고, 1번 성질에 의해 Tree가 되는 것 이다.

1번 조건만 만족하는 Subgraph는 **Spanning Tree**라 한다.

네트워크 회선의 설치 cost를 최소로 하는 시나리오에서 MST가 활용될 수 있다.

MST를 구하는 두 가지 알고리즘은 [Kruskal's Algorithm](Algorithm.md), [Prim Algorithm](Algorithm.md)이 있다.

<br>

# Stack, Queue, Tree, Heap 비교

✅ Tree는 일반 Binary Tree 기준
✅ 각 자료구조에 저장되어 있는 Element 개수를 N이라 가정

||Stack|Queue|Tree|Heap|
|-|-|-|-|-|
|Insert<br>시간복잡도|O(1)|O(1)|O(logN)|O(logN)|
|Delete<br>시간복잡도|O(1)|O(1)|O(logN)|O(logN)|
|접근 가능한 영역|top|front, rear|All|top|
|Linear 여부|O|O|X|X|

<br>
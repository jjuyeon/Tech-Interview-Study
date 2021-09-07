> 'Stack, Queue, Tree, Heap'의 특징을 설명해주세요.
> Stack: LIFO 구조를 가진 선형 자료구조 입니다. top이라는 포인터를 가지며, 해당 포인터가 가리키는 위치에만 삽입/삭제/원소 반환이 가능합니다. 주로 함수의 콜 스택, 브라우저의 뒤로가기에서 사용합니다.
> Queue: FIFO 구조를 가진 선형 자료구조 입니다. front, rear라는 두 개의 포인터를 가지며 front에서 삭제/원소 반환, rear에서 삽입이 가능합니다. 주로 CPU 스케줄링에서 사용합니다.
> Tree: Connected + Acyclic(연결되어 있고, 사이클이 없는) 그래프의 한 종류입니다. 주로 루트를 가진 Rooted Tree로 많이 사용합니다. 이진 탐색 트리, 힙, 레드블랙 트리 등 다양한 자료구조들의 기본이 됩니다.
> Heap: Complete Binary Tree의 한 종류로, 각 parent가 자신의 child node의 value보다 작은지/큰지에 따라 Min-Heap인지 Max-Heap으로 분류할 수 있습니다. Priority Queue 혹은 Heap Sort에서 활용합니다.

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

# Stack, Queue, Tree, Heap 비교

✅ Tree는 일반 Binary Tree 기준
✅ 각 자료구조에 저장되어 있는 Element 개수를 N이라 가정

||Stack|Queue|Tree|Heap|
|-|-|-|-|-|
|Insert<br>시간복잡도|O(1)|O(1)|O(logN)|O(logN)|
|Delete<br>시간복잡도|O(1)|O(1)|O(logN)|O(logN)|
|접근 가능한 영역|top|front, rear|All|top|
|Linear 여부|O|O|X|X|
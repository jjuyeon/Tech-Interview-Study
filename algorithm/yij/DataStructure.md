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
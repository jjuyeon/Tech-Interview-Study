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
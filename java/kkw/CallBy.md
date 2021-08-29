


## `Call By Value`
- 메소드 호출 시에 사용되는 인자의 메모리에 저장되어 있는 값(Value)를 복사하여 보내는 방법
- Primitive Type 들은 Call By Value로 넘어간다.

```
  
  int a = 10
  plus(a);
  System.out.println(a);

  void plus(int a){
    a += 10;
  }

  결과 : 10
  //값만 복사될 뿐 원본 인자에는 영향을 주지 못한다.

```


## `Call By Reference` 
- 메소드 호출 시에 사용되는 인자 값의 메모리에 저장된 주소(Address)값을 복사하여 보낸다.
- Reference Type 들은 Call By Reference로 넘어간다

```
int a[] = {10};
a[0] = 10;
plus(a);
System.out.println(a);

void plus(int[] a){
  a[0] += 10;
}

결과 : 20
//주소 값이 넘어갔기에 원본 인자 값이 변한다.
```

## 그렇다면 String 은 ?
- JAVA의 공식 API 문서에 따르면 primitive type은 `boolean, byte, short, int, long, char, floag, double`으로 명시하고 있다.
- 그렇다면 String을 매개변수로 넘길경우 Call-By-Reference 이므로 값이 변경되어야 한다.

```
String a = "hello";
test(a);
System.out.println(a);

void test(String a){
  a = "world";
}

결과 : hello
```

- 하지만 실제로 테스트 해보면 원본 값에 전혀 영향을 끼치지 못한다.

## 왜 그런걸까?
- 자바는 엄밀하게 말하면 항상 `Call By Value` 이다.
- 다음을 보자
```
class Node{
  int num;
  Node(int num){
    this.num = num;
  }

  void test(Node node){
    node = new Node(20);
  }
  
  main(){

    Node node = new Node(10);
    test(node);
    System.out.println(node.num);
    //결과는?
  }
}
```
- `Call By Reference` 라면 결과값이 `20`이 나올테지만, 실제로 돌려보면 결과값은 `10`이 출력된다
- 이는 Java가 Reference Type 들의 객체를 전달하는게 아니라 그 변수가 가지고 있는 주소 값을 복사하여 전달하기 때문이다.
- 우리는 Stack에게 새로운 주소를 주는 Object 방식으로는 원본 값에 어떠한 변화도 줄 수 없다는 뜻이다.

## 그래도 주소값이 전달되니깐 수정할 수 있지않아요?
- String의 특징 중 하나는 불변성(immutable)이다.
- String 인스턴스를 생성하게 되면, JVM은 Stack에 변수의 메모리를 할당하고 Heap 영역 내부에 String Pool 을 가르키고 있게 된다.

  ![메모리](https://user-images.githubusercontent.com/43779730/130810447-f377b8b1-0eaa-49a2-95ca-17d5d3d13783.png)

- JAVA에서는 String 값이 수정될 경우, String Pool에 새로 할당하여 그 주소값을 반환해준다. 
- 그렇다는 것은 매개변수로 넘어간 String은 결국 원본 String의 Stack 영역이 가르키고 있는 주소 값을 복사했을 뿐이므로, 메소드 내 지역변수 String의 Stack 영역의 참조값을 수정하여도 원본 String에는 아무 영향이 없는 것이다.

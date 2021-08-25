# :question: JAVA 의 호출방식

#### reference
https://devlog-wjdrbs96.tistory.com/44<br>
https://sleepyeyes.tistory.com/11
<hr>

## Question
1. [Call by Value와 Call by Reference의 차이에 대해 설명해주세요.](#2-call-by-reference)
- Call by Value는 실제 값에 의한 호출 방법이고, Call by Reference는 참조에 의한 호출 방법입니다.
- 좀 더 구체적으로, Call by Value는 함수의 매개변수 값을 복사하여 전달하는 방식으로, 전달받은 값이 변경되어도 원본 값은 변경되지 않고 유지됩니다.
- 반면, Call by Reference는 매개변수의 레퍼런스를 전달하는 방식으로, 전달받은 값이 변경되면 원본 값도 같이 변경됩니다.
<hr>

## :nerd_face:	What I study
### 1. Call by Value
- 값에 의한 호출
- JAVA는 항상 ```Call by Value```로 값을 넘긴다.
- 함수가 호출될 때, 메모리 공간 안에는 함수를 위한 임시 공간이 생성된다.
  - 함수가 종료되면 임시로 생성되었던 공간은 사라진다.
- 함수 호출시 **인자로 전달되는 변수의 값을 복사하여 함수의 인자로 전달한다.**
  - 원본의 데이터가 변경될 가능성이 없다.
  - 인자를 넘겨줄 때마다 메모리 공간을 할당해야해서 메모리 공간을 더 잡아먹는다.
- 복사된 인자는 함수 안에서 지역적으로 사용되는 local value의 특성을 가진다.
- 따라서 함수 안에서 인자의 값이 변경되어도, ***외부의 변수의 값은 변경되지 않는다.***
<br>

### 2. Call by Reference
- 참조에 의한 호출
- 함수가 호출될 때, 메모리 공간 안에는 함수를 위한 임시 공간이 생성된다.
  - 함수가 종료되면 임시로 생성되었던 공간은 사라진다.
- 함수 호출시 **인자로 전달되는 변수의 레퍼런스를 전달한다.**
  - 메모리 공간을 더 잡아먹지는 않는다.
  - 원본 값이 변경될 수 있다는 위험이 존재한다.
- 따라서 함수 안에서 인자의 값이 변경되면, ***인자로 전달된 변수의 값도 함께 변경된다.***

||Call by Value|Call by Reference|
|:---:|:---:|:---:|
|호출|값에 의한 호출|참조에 의한 호출|
|처리방식|전달받은 값을 복사|전달받은 값을 직접 참조|
|전달받은 값을 변경했을 때|원본은 변하지 않음|원본도 같이 변함|

<br>

### 3. JAVA 의 호출방식
- JAVA에서 함수의 인자로 전달되는 데이터 타입이 **원시 타입**인 경우 value로 ***해당하는 변수의 값***을 복사해서 전달한다. ***call by value***
- 따라서 함수 안에서 인자의 값이 변경되어도, **외부의 변수의 값은 변경되지 않는다.**
```java
public class Main {
    public static void main(String[] args) {
        int n = 10;
        System.out.print(n); // 10
        primitive(n);
        System.out.println(n); // 10
    }

    public static void primitive(int n) { // 원시 타입을 함수의 인자로 전달
        n -= 5;
        System.out.println(n); // 5
    }
}
```
<br>

- JAVA에서 함수의 인자로 전달되는 데이터 타입이 **참조 타입**인 경우 value로 ***해당 객체의 주소 값***을 복사하여 넘긴다. ***call by value***
  - JAVA는 C/C++과 같이 변수의 주소값 자체를 가져올 방법이 없으며, 이를 넘길 수 있는 방법도 없다.
- 따라서 **원본 객체의 property까지는 접근이 가능하나, 원본 객체 자체를 변경할 수는 없다.**
```java
public class Main {
    public static void main(String[] args) {
        int[] arr = {1, 2, 3};
        for (int num : arr) { // 1 2 3 
            System.out.print(num + " ");
        }
        
        reference(arr);

        for (int num : arr) { // 10 20 30 
            System.out.print(num + " ");
        }
    }

    // 참조형의 경우, 주소값이 전달되므로 값을 변경하면 원본도 영향을 받는다.
    public static void reference(int[] a) {
        for (int i = 0; i < a.length; i++) {
            a[i] *= 10;
        }
    }
}
```

```java
public class Main2 {
  /* parameter는 참조변수 Person 자체의 reference가 아닌 Object가 "저장하고 있는 주소값(value)" */
  public static void assignNewPerson(Person p) {
    p = new Person("yeon");
  }
  public static void changeName(Person p) {
    p.setName("yeon");
  }

  public static void main(String[] args) {
    Person p = new Person("su");

    assignNewPerson(p);
    System.out.println(p); // name is su

    changeName(p);
    System.out.println(p); // name is yeon
  }
}
```
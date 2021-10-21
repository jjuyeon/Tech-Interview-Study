# JVM

## 등장 배경

Write Once, Run Anywhere - Sun Micro Systems

일반적인 프로그래밍 언어는 컴파일 환경과 실행하는 환경이 달라지면 문제가 발생한다.

C의 경우 High Level 코드를 어셈블리어로 변환하고, 이를 기계어로 변환하는 데 플랫폼마다 실행시킬 수 있는 **기계어**의 문법은 전부 다르기 때문이다 ❗❗

Java는 WORA를 구현하기 위해 물리적 머신과 별개인 가상 머신을 기반으로 동작하도록 설계되었다. 따라서 JVM이 설치되어 있기만 하면 Window/Mac/Linux 등 다양한 플랫폼 환경에서도 자바 바이트 코드를 실행할 수 있다.

## JVM의 정의

: 자바 바이트 코드를 해석하고 실행하는 주체. 자바 애플리케이션을 클래스 로더를 통해 읽어 들여 자바 API와 함께 실행한다.

JVM은 일반적으로 인터프리터, JIT 컴파일 방식을 사용해서 다른 컴퓨터 위에서도 자바 바이트코드를 실행할 수 있도록 구현된다.

Java는 썬 마이크로시스템즈가 개발했지만 따르기만 하면 어떤 벤더든 JVM을 이용해서 개발할 수 있는 JVM 명세를 제공한다.

자바 바이트코드는 플랫폼에 독립적이며, 모든 자바 가상 머신은 JVM 규격에 정의된 대로 자바 바이트코드를 실행한다. 물론 JVM의 내부 구조는 타겟 플랫폼에 의존한다(운영체제 별 JVM).

표준 자바 API까지 동일한 동작을 하도록 구현한 상태에서는 이론적으로 모든 자바 프로그램이 CPU와 운영체제 종류에 무관하게 동일하게 동작할 것을 보장한다.

> 다양한 벤더가 만든 JVM이 명세를 만족하는지 어떻게 검증할까?

오라클은 TCK(Test Compatibility Kit)라는 테스트 툴을 제공해서 잘못 작성된 수많은 클래스 파일을 비롯한 테스트를 수행하여 통과할 수 있는지 검증한다.
> 

## JVM 특징

![Untitled](Java%201230eb30782543f99311ad465e5e16c6/Untitled.png)

[https://wikidocs.net/257](https://wikidocs.net/257)

- 스택 기반 가상 머신: 일반적 컴퓨터 아키텍처의 하드웨어가 레지스터 기반으로 동작하는 데 비해 JVM은 스택 기반으로 동작한다.
- 심볼릭 레퍼런스: 명시적 메모리 주소 기반의 레퍼런스가 아니라 심볼릭 레퍼런스를 지원해 C와 같이 주소 값을 임의로 조작하는 포인터 연산은 불가능하다.
- **가비지 컬렉션**: 클래스 인스턴스는 사용자 코드에 의해 명시적으로 생성되고 가비지 컬렉션에 의해 자동으로 없어진다.
- 기본 자료형의 정의를 명확히 하여 플랫폼 독립성 보장: C/C++은 플랫폼에 따라 int형의 크기가 다르지만, JVM은 기본 자료형을 명확하게 정의해서 호환성을 유지하고 플랫폼 독립성을 보장한다.

## JVM 사양

### Java 클래스로더

Java Class를 JVM으로 동적 로드하는 자바 런타임 환경(JRE)의 일부

일반적으로 클래스들은 요청 시 한 번만 로드된다. JRE는 클래스로더 때문에 파일과 파일 시스템에 대해 알 필요가 없다(delegation).

소프트웨어 라이브러리는 오프젝트 코드의 모임인데, 자바에서는 JAR로 묶여있다. 자바 클래스로더는 라이브러리를 위치시키고 내용물을 읽으며 라이브러리 안에 포함된 클래스를 읽는 역할을 한다.

프로그램에 의해 호출될 때 까지 로드하지 않다가 요청이 올 때 이루어진다. JVM이 시작되면 3개의 클래스 로더들이 사용된다.

- 부트스트랩 클래스 로더: `<JAVA_HOME>/jre/lib` 디렉터리에 위치한 핵심 자바 라이브러리들을 불러들인다. 이 클래스 로더는 JVM의 핵심이며, 네이티브 코드로 작성되어 있다.
- 확장 클래스 로더: 확장 디렉터리에 코드를 로드한다.
- 시스템 클래스 로더: java.class.path에서 볼 수 있으며 CLASSPATH 환경 변수에 매핑된다.

### 바이트코드 명령어

자바 가상 머신이 실행하는 명령어 형태

```java
for (int i = 2; i < 1000; i++) {
    for (int j = 2; j < i; j++) {
        if (i % j == 0)
            continue outer;
    }
    System.out.println (i);
}
```

자바 컴파일러는 위의 자바 코드를 아래와 같은 바이트 코드로 변환한다.

```bash
0:   iconst_2
1:   istore_1
2:   iload_1
3:   sipush  1000
6:   if_icmpge       44
9:   iconst_2
10:  istore_2
11:  iload_2
12:  iload_1
13:  if_icmpge       31
16:  iload_1
17:  iload_2
18:  irem
19:  ifne    25
22:  goto    38
25:  iinc    2, 1
28:  goto    11
31:  getstatic       #84; // Field java/lang/System.out:Ljava/io/PrintStream;
34:  iload_1
35:  invokevirtual   #85; // Method java/io/PrintStream.println:(I)V
38:  iinc    1, 1
41:  goto    2
44:  return
```

## Java Compile 과정

![https://user-images.githubusercontent.com/30489264/138115024-39c2542a-726d-46f7-90dd-ce58bdd3cc12.png](https://user-images.githubusercontent.com/30489264/138115024-39c2542a-726d-46f7-90dd-ce58bdd3cc12.png)

![https://user-images.githubusercontent.com/30489264/138115280-ff2e91e0-ae11-4eca-b0c9-8222b66225da.png](https://user-images.githubusercontent.com/30489264/138115280-ff2e91e0-ae11-4eca-b0c9-8222b66225da.png)

JVM Memory : JVM이 Java Bytecode를 실행하기 위해 사용하는 메모리 공간

Method Area & Heap → 모든 쓰레드가 공유

- Method Area : 클래스 로더가 클래스 파일을 읽어오면, 클래스 정보를 파싱해서 메서드 에리어에 저장
    - 정적 변수, 메서드 정보, 필드 변수 등
- Heap: 프로그램을 실행하면서 생성한 모든 객체를 Heap에 저장

Java Stack & PC Register & Native Method Stacks → 각 쓰레드마다 존재

- Java Stack: 쓰레드 별로 1개만 존재하고, 스택 프레임은 메서드가 호출될 때 마다 생성된다. 메서드 실행이 끝나면 스택 프레임은 pop되어 스택에서 제거된다.
    - Stack Frame: Local variables array, Operand stack, Frame Data를 갖는다
    - Frame Data는 Constant Pool, 이전 스택 프레임에 대한 정보, 현재 메서드가 속한 클래스/객체에 대한 참조 등의 정보를 갖는다.
- PC(Program Counter): 각 쓰레드는 메서드를 실행하고 있고, PC는 그 메서드 안에서 몇 번째 줄을 실행하고 있는지 나타내는 카운터
- Native Method Stacks: Java Bytecode가 아닌 다른 언어로 작성된 메서드

## 참고 자료

> [https://d2.naver.com/helloworld/1230](https://d2.naver.com/helloworld/1230)
[https://ko.wikipedia.org/wiki/자바_가상_머신](https://ko.wikipedia.org/wiki/%EC%9E%90%EB%B0%94_%EA%B0%80%EC%83%81_%EB%A8%B8%EC%8B%A0)
[https://ko.wikipedia.org/wiki/자바_클래스로더](https://ko.wikipedia.org/wiki/%EC%9E%90%EB%B0%94_%ED%81%B4%EB%9E%98%EC%8A%A4%EB%A1%9C%EB%8D%94)
> 

## 정리

> ❓ JVM에 대해 설명해주세요

JVM은 Java Virtual Machine으로, 자바의 바이트코드를 해석하고 실행시키는 주체를 의미합니다. 플랫폼에 관계 없이 해당 플랫폼에 맞는 JVM만 있다면 자바 코드를 실행할 수 있습니다.

❓ 언어의 실행 조건이 플랫폼에 따라 달라지는 이유가 무엇일까요?
CPU, OS같은 실행 환경에 따라 기계어 코드가 달라지는데 이것은 플랫폼마다 다릅니다. C/C++은 크로스 컴파일러를 사용해서 다른 플랫폼에서도 실행 가능한 코드로 변환하는 방식을 사용하지만, Java는 각 운영환경 마다 별도의 JVM을 통해 바이트 코드만 있다면 모든 종류의 하드웨어에서 실행할 수 있습니다.
>
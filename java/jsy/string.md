# :question: JAVA 의 String

#### reference
https://github.com/WeareSoft/tech-interview/blob/master/contents/java.md#java%EC%97%90%EC%84%9C-%EC%99%80-equals%EC%9D%98-%EC%B0%A8%EC%9D%B4
<hr>

## Question
1. ['=='과 'equals()'의 차이에 대해 설명하세요.](#1-문자열-비교)
- 동등연산자 ==는 두 객체가 같은 메모리 공간을 가리키는지 확인합니다. 즉, 비교하고자 하는 대상의 주소값을 비교합니다.
- 객체 비교 메소드 equals()는 두 객체의 값이 같은지 확인합니다. 즉, 비교하고자 하는 대상의 내용 자체를 비교합니다.
<br><br>

2. [String, StringBuffer, StringBuilder의 차이를 설명하세요.]()
<br><br>

3. [String형 객체를 ""로 만들었을 때와 new 키워드를 이용해서 만들었을 때의 차이점은?]()
<hr>

## :nerd_face:	What I study
### 1. 문자열 비교
#### 1-1) == 연산자
- 동등 연산자
- 두 객체가 같은 메모리 공간을 가리키는지 확인하는 **참조 비교**를 한다.
- 비교하고자 하는 대상의 **주소값**을 비교한다.
- boolean type으로 반환한다.
- 모든 원시 타입에 대해 적용할 수 있다.
#### 1-2) equals() 메소드
- 객체 비교 메소드
- 두 객체의 값이 같은지 확인하는 **내용 비교**를 한다.
  - 비교하고자 하는 대상의 **내용 자체**를 비교한다.
  - boolean type으로 반환한다.
- 원시 타입에는 적용할 수 없다.

```java
// Thread 객체
Thread t1 = new Thread();
Thread t2 = new Thread(); // 새로운 객체 생성. 즉, s1과 다른 객체.
Thread t3 = t1; // 같은 대상을 가리킨다.
// String 객체
String s1 = new String("JAVA");
String s2 = new String("JAVA");
String s3 = "JAVA";
String s4 = "JAVA";

System.out.println(t1 == t3); // true
System.out.println(t1 == t2); // false(서로 다른 객체이므로 별도의 주소를 갖는다.)
System.out.println(s1 == s2); // false(서로 다른 객체)
System.out.println(s2 == s3); // false(서로 다른 객체)
System.out.println(s3 == s4); // true

System.out.println(t1.equals(t2)); // false
System.out.println(s1.equals(s2)); // true(모두 "JAVA"라는 동일한 내용을 갖는다.)
System.out.println(s1.equals(s3)); // true
System.out.println(s3.equals(s4)); // true
```
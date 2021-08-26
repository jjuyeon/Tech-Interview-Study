# 제네릭
데이터 타입을 일반화(generalize) 하는 것으로 컴파일시 데이터 타입을 체크해준다.
런타임 에러에서 발생할 수 있는 형변환 에러를 컴파일 에러로 잡을 수 있다.

제네릭을 사용하지 않는 코드와의 호환성을 위해 컴파일시, 제네릭 타입은 변환된다.

제네릭 전에는 형변환 에러가 발생하여 컴파일 에러는 나지 않지만, 런타임 에러가 발생하여 프로그램이 종료되었다.
```Java
public class GenericEx {
  public static void main(String[] args) {
ArrayList list = new ArrayList();
list.add(10);
list.add("100"); // 컴파일 됨

System.out.println(list): // 형변환에러 발생 
  }
}
```

제네릭이 등장한 JDK 1.5 이후부터는 제네릭 클래스의 참조변수와 생성자에 다음과 같이 선언해주는 것이 좋다.  
참조변수와 생성자에 대입한 타입은 일치해야 한다. JDK 1.7 이후부터는 생성자 대입한 타입은 생략 가능하다. 
```Java
ArrayList list = new ArrayList(); // JDK 1.5 이전
ArrayList<Object> list = new ArrayList<Objcet>(); // JDK 1.5 이후
ArrayList<Object> list = new ArrayList<>(); // JDK 1.7 이후
```

## 제네릭 장점
- 객체 타입의 안정성
- 형변환 번거로움 감소


## 제네릭 용어
```Java
ArrayList<T> // 제네릭 클래스
ArrayList // 원시 타입
T // 타입 변수
```
  

## 제네릭 다형성
```Java 
class Vehicle {  }
class Car extends Vehicle {  }
class Truck extends Vehicle {  }

 
ArrayList<Vehicle> list = new ArrayList<Vehicle>(); // 참조변수와 생성자의 대입 변수는 일치
list.add(new Vehicle());
list.add(new Car()); // 자손
list.add(new Truck()); // 자손


Vehicle v = list.get(0);
Car c = (Car)list.get(1); // 형변환 필요
Truck t = (Truck)list.get(2); // 형변환 필요
```


## 제한된 제네릭 클래스
제네릭 대입 타입으로 Vehicle과 Vehicle의 자손 타입만 허용한다.
```Java
class Box<T extends Vehicle> {

}
```


## 제네릭 제약 사항
- static 멤버에 타입 변수 사용 불가
  - 이유: 제네릭 인스턴스 별로 다르게 타입 변수를 다르게 대입할 수 있으므로, 공통으로 사용하는 static 멤버에는 사용할 수 없다.
- 객체/배열 생성시 타입 변수 사용 불가 
  - 이유 : new로 객체나 배열을 생성할 때는 타입이 확정되어 있어야 하기 때문이다.


## 제네릭 와일드 카드  
서로 다른 타입이 대입된 여러 제네릭 객체를 다루기 위한 것
- `<?>` : 모든 타입 가능
- `<? extends T>` : T와 T의 자손들만 사용 가능. 상한을 T로 제한  
- `<? super T>` : T와 T의 조상들만 사용 가능. 하한을 T로 제한   

```Java
ArrayList<? extends Vehicle> list = new ArrayList<Car>();
```




참조
- 자바의 정석 - 기초편
- http://tcpschool.com/java/java_generic_concept



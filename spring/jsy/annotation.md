# :question: Spring Annotation

#### reference
https://youngjinmo.github.io/2021/06/bean-component/<br>
https://galid1.tistory.com/494<br>
https://withseungryu.tistory.com/64
<hr>

## Question
1. Bean/Component 어노테이션은 무엇이며, 둘의 차이점은 무엇인가요?
- 두 어노테이션 모두 IoC Container에 Bean을 등록하기 위해 사용되는 어노테이션입니다.
- 하지만, 두 어노테이션은 용도에 있어 차이점을 가지고 있습니다.
- @Component는 개발자가 작성한 class를 기반으로 실행시점에 Bean을 생성합니다. (싱글톤) 
- 반면, @Bean은 개발자가 작성한 method를 기반으로 method에서 반환하는 객체를 Bean으로 생성합니다. (싱글톤)
- 각자의 용도가 정해져있으므로 정해진 곳에서만 사용이 가능하며, 다른 곳에서 사용하려고 하면 컴파일 에러가 발생합니다.
<hr>

## :nerd_face:	What I study

### <참고>
- Spring은 개발의 제어권이 IoC Container에 있다.
- Spring이 개발자 대신 객체를 제어하기 위해서는 Bean으로 등록되어있어야 한다.
  - IoC Container는 Bean을 가져오기 위해 Bean Scanning을 한다.
  - cf) Bean 등록 방법: 1) XML로 지정, 2) 어노테이션

<br><br>

### 1. @Bean
- Bean으로 객체를 등록하기 위해 사용하는 어노테이션
- 개발자가 **직접 제어가 불가능한 외부 라이브러리를 Bean으로 등록하고 싶은 경우** 사용한다.
- 개발자가 작성한 ***method***를 통해 반환되는 객체를 Bean으로 만든다.
- 개발자가 수동으로 Bean을 등록해줘야 한다.
- @Configuration을 선언한 클래스 내부에서 사용한다.
- **name** 속성은 Bean의 id를 설정해준다. (모든 문자를 소문자로 바꿈)
  - 속성을 사용하지 않으면, 메소드 명을 camelcase로 변경한 것이 Bean의 id가 생성된다.

```java
@Configuration
public class ExampleConfig {
    // 'array' Bean 생성 
    @Bean // 외부 라이브러리를 사용하는 경우
    public ArrayList<Integer> array(){
    	return new ArrayList<Integer>();
    }
    
    // 'smallcharger' Bean 생성
    @Bean(name="smallcharger") // 해당 method를 통해 개발자가 만든 클래스가 반환되는 경우
  	public Product small(){
          Battery charger = new Battery("AAA", 2500); // 사이즈, 가격
          charger.setRechargeable(true);
          return charger;
   	}
}
```

<br>

### 2. @Component
- Bean으로 객체를 등록하기 위해 사용하는 어노테이션
- 개발자가 **직접 제어할 수 있는 내부 클래스를 Bean으로 등록하고 싶은 경우** 사용한다.
- 개발자가 직접 작성한 ***Class***를 Bean으로 등록한다.
- Spring이 Runtime할 때, component-scan을 통해 자동으로 Bean을 찾고 등록한다. (자동 의존성 주입)
- `@Repository`, `@Service`, `@Controller`, `@Configuration`가 `@Component`에 속해있다.
- **value** 속성은 Bean의 id를 설정해준다. (모든 문자를 소문자로 바꿈)
  - 속성을 사용하지 않으면, 메소드 명을 camelcase로 변경한 것이 Bean의 id가 생성된다.

```java
@Component(value="hello")
public class Example {
	puiblic Example(){
    	System.out.println("Hello world");
    }
}
```
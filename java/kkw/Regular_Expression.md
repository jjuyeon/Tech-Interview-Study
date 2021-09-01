## 정규 표현식(Regular Expression)
- 특정한 규칙을 가진 문자열의 집합을 표현하는데 사용하는 형식 언어



## 자바의 정규표현식 사용법
- `java.util.regex` 의 `Pattern` 클래스와 `Matcher` 클래스를 이용하여 정규 표현식을 사용 가능
- String의 matches 함수를 이용하여 사용 가능
- 아래와 같은 방법으로 정규표현식과 대상 문자열이 일치하는지 확인 가능

```java
Pattern p = Pattern.compile("a*b");
Matcher m = p.matcher("aaaab");
boolean b = m.matches();
```

```java
boolean b = Pattern.matches("a*b", "aaaab")
```

```java
String str = "aaaab";
String regex = "a*b";
		
boolean b = str.matches(regex);
```

### Pattern 클래스
- 우선 순위
  1. Literal escape : \x
  2. Grouping	[...]
  3. Range	a-z
  4. Union	[a-e][i-u]
  5. Intersection	[a-z&&[aeiou]]
- compile(String regex) : 주어진 정규표현식(regex)으로 패턴을 만듬
- matcher(CharSequense input) : 대상 문자열(input)이 패턴과 일치할 경우 true를 반환
- pattern() : 만들어진 패턴을 String으로 반환함


### Matcher 클래스
- 대상 문자열의 패턴을 해석하고, 주어진 패턴과 일치하는지 판별할 때 주로 사용
- matches() : 대상 문자열과 패턴이 일치할 경우 true를 반환
- find() : 대상 문자열과 패턴이 일치하는 경우 true를 반환하고, 그 위치로 이동
- start() : 매칭되는 문자열 시작위치를 반환
- end() : 매칭되는 문자열 끝 다음 위치를 반환
- group() : 매칭된 부분을 반환

### 정규 표현식 문법
  - ![1_agUeLvuA3-9NcX12_vI-hg](https://user-images.githubusercontent.com/43779730/131688858-5a6e1b05-ab52-4486-ac7d-4b2e7865762d.png)


  - 
    |횟수|0|1|2+|
    |:--:|:--:|:--:|:--:|
    |?|O|O|X|
    |*|O|O|O|
    |+|X|O|O|

- Greedy : 매칭을 위해서 입력된 문자열 전체를 읽어서 확인하고 뒤에서 한자씩 빼면서 끝까지 확인합니다.
- Reluctant : 입력된 문자열에서 한글자씩 확인해 나갑니다. 마지막에 확인하는 것은 전체 문자열 입니다.
- Possessive : 입력된 전체 문자열을 확인합니다. Greedy와 달리 뒤에서 빼면서 확인하지 않습니다.
  - |Greedy|Reluctant|Possessive|Meaning|
    |:--:|:--:|:--:|:--:|
    |x?|x??|x?+|x, 1번 혹은 0번|
    |x*|x*?|x*+|x, 0번 이상|
    |x+|x+?|x++|x, 1번 이상|
    |x{n}|x{n}?|x{n}+|x, 정확하게 n번 반복|
    |x{n,}|x{n,}?|x{n,}+|x, 적어도 n번 반복|
    |x{n,m}|x{n,m}?|x{n,m}+|x, 적어도 n번 이상 m번 이하로 반복|



### 참고 사이트
- https://docs.oracle.com/javase/8/docs/api/java/util/regex/Pattern.html#sum
- https://offbyone.tistory.com/400
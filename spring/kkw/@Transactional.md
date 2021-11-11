## Transactional을 조심해서 사용해야 하는 이유

### save를 해주지 않았는데 DB에 변경이 되는 경우

- JPA를 사용하다보면 save를 해주지 않았음에도 DB에 변경된 값이 들어가는 경우를 볼 수 있다.
- 아래와 같은 예시를 볼 수 있다.

```Java
@Service
@Transactional(readOnly = true)
public class TestService{

  @Autowired
  UserRepository userRepo;


  @Transactional
  public void setName(String name){
    User user = userRepo.findById("test");
    user.setName(name);
  }
}
```

- 위와 같이 코드를 작성한 경우 userRepo를 통해 save 함수를 호출 하지 않았음에도 DB에서 user의 name이 변경되는 것을 볼 수 있다.

```Java
@Service
public class TestService{

  @Autowired
  UserRepository userRepo;


  public void setName(String name){
    User user = userRepo.findById("test");
    user.setName(name);
  }
}
```

- 반면에 위와 같은 코드에서는 user의 값이 변하지 않는 것을 알 수 있다.

### 무슨 차이일까?

- EntityManager는 영속성 컨텍스트를 통해서 객체들을 관리한다.
- EntityManager는 트랜잭션 마다 다른 영속성 콘텍스트를 사용한다.
- 이 영속성 컨텍스트 안에 있는 객체가 변경될 경우, 변경 감지를 통해서 Update Query가 생성된다.
- @Transactional 으로 감싸운 메소드가 끝날 때, 트랜잭션이 커밋되고 이 때 EntityManager가 자동으로 flush를 호출하게 됩니다.
- 그러면 엔티티와 스냅샷을 비교해서 변경된 엔티티들을 찾고 수정 쿼리가 자동으로 생성되어 DB에 커밋되게 되는 것 입니다.

### 주의할 점

- Hibernate에서는 SQL쿼리가 항상 우리가 작성한대로 진행을 하지 않습니다.
- Hibernate의 공식 문서를 확인해보면 다음과 같은 순서로 쿼리문이 진행되는 것을 알 수 있습니다.
- 참고 : https://docs.jboss.org/hibernate/orm/4.2/javadocs/org/hibernate/event/internal/AbstractFlushingEventListener.html

```
1. Inserts, in the order they were performed
2. Updates
3. Deletion of collection elements
4. Insertion of collection elements
5. Deletes, in the order they were performed
```

- 그렇기 때문에 메소드와 Service 자체를 Transactional로 묶는 경우 Flush를 수동으로 해줘야 하는 경우가 생기지 않도록 구성을 잘해야 합니다.

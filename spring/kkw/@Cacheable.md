## @Cacheable

### 캐시를 사용하는 이유

- 서버의 부담을 줄이고, 성능을 높이기 위해 사용
- 반복적으로 동일한 결과를 반환화는 경우에 용이

### Spring 에서의 Cache

- 스프링에서 AOP 방식으로 Cache 기능을 제공하고 있음
- @EnableCaching 추가
  ```java
    @EnableCaching
    @SpringBootApplication
    public class MainApplication{
       public static void main(String[] args) {
        SpringApplication.run(MainApplication.class, args);
      }
    }
  ```
- 캐시 매니저 빈 추가

  - ConcurrentMapCacheManager: Java의ConcurrentHashMap을 사용해 구현한 캐시를 사용하는 캐시매니저
  - SimpleCacheManager: 기본적으로 제공하는 캐시가 없어 사용할 캐시를 직접 등록하여 사용하기 위한 캐시매니저
  - EhCacheCacheManager: 자바에서 유명한 캐시 프레임워크 중 하나인 EhCache를 지원하는 캐시 매니저
  - CompositeCacheManager: 1개 이상의 캐시 매니저를 사용하도록 지원해주는 혼합 캐시 매니저
  - CaffeineCacheManager: Java 8로 Guava 캐시를 재작성한 Caffeine 캐시를 사용하는 캐시 매니저
  - JCacheCacheManager: JSR-107 기반의 캐시를 사용하는 캐시 매니저
  - RedisCacheManager : 외부 메모리인 Redis를 캐시로 사용할 때 사용하는 캐시 매니저

- @Cacheable

  - 캐싱하고 싶은 메소드 위에 달아주면 된다.
  - 이 메소드의 캐시 값이 있으면 메소드를 진행하지 않고 캐시 값을 반환하고 없는 경우 진행 후 반환 값을 캐시 메모리에 저장
  - value = "캐시의 키 값에 메소드명으로 붙게 된다"
  - key = "캐시의 키 값"
  - cacheManager =

    ```java
    @Cacheable(key="#userId", value="userInfo")
    public UserDTO getUserInfo(String userId) {

      User user = userRepo.findById(userId);

      return new UserDTO(user);
    }
    ```

    ```Redis
      key : userInfo::"test12345"
      value : {id : test12345, name: 김철수}
    ```

- @CachePut

  - 메소드에 실행에 영향을 주지 않고 캐시를 갱신해야 하는 경우에 사용
  - 사용 법은 위와 같음
  - @Cacheable은 캐시를 사용하여 메소드 실행을 건너뛰지만 @CachePut은 캐시 갱신을 하려고 실행을 강제한다. (특이케이스가 아닌 이상 두 어노테이션은 같이 사용하지 않는 것을 권장한다)

    ```java
        @CachePut(key="#userId", value="userInfo")
        @Transactional
      public UserDTO changeUserName(String userId, String userName) {

        User user = userRepo.findById(userId);
        user.setName(userName);
        return new UserDTO(user);
      }
    ```

- @CacheEvict

  - 캐시를 제거하고자 하는 메소드에 사용
  - value = "캐시 앞에 들어가는 접두사"
  - key = "캐시의 키 값"
  - allEntries=true : key값과 관계없이 모든 캐시를 제거

    ```java
      @Cacheable(value ="userInfos")
      public UserListDTO getUserList(){
        List<User> userList = userRepo.findAll();

        return new UserListDTO(userList);
      }


      @CacheEvict(value="userInfos")
      @Transactional
      public UserDTO signUp(String userId, String userName) {

        User user = new User(userId, userName);
        userRepo.save(user);

        return new UserDTO(user);
      }
    ```

- @Caching

  - 여러 개의 @CacheEvict나 @CachePut을 하나의 메소드에 지정해야 하는 경우에 사용

  ```Java
  @Caching(evict = { @CacheEvict("primary"), @CacheEvict(value = "secondary", key = "#p0") })
  public Book importBooks(String deposit, Date date)
  ```

- 위의 어노테이션이 달린 메소드들은 같은 클래스 내부의 어노테이션이 없는 다른 메소드에 의해서 호출되면 안된다.

  ```Java
  public UserDTO updateUser(String userId, String userNamem, String type){

    UserDTO userDto = null;

    if(type.equals("name") || type.equals("ALL")){
      userDto = updateName(userId, userName);
    }

    return userDto;
  }

  @Cacheable(value="user", key="userId")
  @Transactional
  public UserDTO updateName(String userId, String userName){
    User user = userRepo.findById(userId);
    user.setName(userName);

    return new UserDTO(user);
  }
  ```

  - 위 같은 경우에선 캐시가 되지 않음
  - @Cacheable은 캐싱 처리하기 위해 인터셉터를 통해 적당한 시점에서 요청을 가로채서 필요한 처리를 실행하게 된다.
  - Proxy를 통해 들어오는 외부 메소드 호출만 인터셉트하는 AOP 특성상 동일 클래스 내(this)에서 접근하는 경우에는 동작하지 않는다.

### 참고 사이트

- https://docs.spring.io/spring-framework/docs/5.0.0.M5/spring-framework-reference/html/cache.html

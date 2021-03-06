> [๐ก] JPA์ Propagation์ ๋ํด ์ค๋ชํด์ฃผ์ธ์

## Transaction of Spring

Spring์์ Transaction ์ค์ ์ ํ๊ธฐ ์ํด์๋

1. Annotation
2. AOP

๋ฅผ ํ์ฉํ  ์ ์๋ค.

`@Transactional` ์ด๋ธํ์ด์์ ํ์ฉํ๋ฉด ํธ๋์ญ์ ์ค์ ์ ํ  ์ ์์ผ๋ฉฐ, ๋ช ๊ฐ์ง ์ฃผ์ ์ฌํญ์ด ์กด์ฌํ๋ค.

### Configure Transaction

<details>
<summary>Java Configuration ์ฝ๋</summary>

```java
@Configuration
@EnableTransactionManagement
public class PersistenceJPAConfig{

   @Bean
   public LocalContainerEntityManagerFactoryBean
     entityManagerFactoryBean(){
      //...
   }

   @Bean
   public PlatformTransactionManager transactionManager(){
      JpaTransactionManager transactionManager
        = new JpaTransactionManager();
      transactionManager.setEntityManagerFactory(
        entityManagerFactoryBean().getObject() );
      return transactionManager;
   }
}
```

</details>

<details>
<summary>XML Configuration ์ฝ๋</summary>

```xml
<bean id="txManager" class="org.springframework.orm.jpa.JpaTransactionManager">
   <property name="entityManagerFactory" ref="myEmf" />
</bean>
<tx:annotation-driven transaction-manager="txManager" />
```
</details>
<details>
<summary>Annotation Configuration ์ฝ๋</summary>

```java
@Service
@Transactional
public class FooService {
    //...
}
```
</details>

<br>

Spring 3.1์์๋ `@EnableTransactionManagement` ์ด๋ธํ์ด์๊ณผ `@Configuration` ์ด๋ธํ์ด์์ ์ฌ์ฉํด์ ํธ๋์ญ์์ ์ฌ์ฉํ  ๊ฒ ์ด๋ผ๊ณ  ๋ช์ํด์ผ ํ๋ค.

ํ์ง๋ง Spring Boot๋ฅผ ์ฌ์ฉํ๋ฉด `spring-data-` ํน์ `spring-tx` ์์กด์ฑ์ ์ฌ์ฉํ๋ฉด ํธ๋์ญ์ ์ค์ ์ default๋ก ํ  ์ ์๋ค.

<br>

#### Annotation Configuration 

`@Transactional` Annotation์ ์ฌ์ฉํ๋ฉด ๋ค์๊ณผ ๊ฐ์ ์ค์ ๋ ํ  ์ ์๋ค.

- ํธ๋์ญ์์ **Propagation Type**
- ํธ๋์ญ์์ **Isolation Level**
- ํธ๋์ญ์์ ์ํด ๋ํ๋ ์์์ **Time Out**
- **readOnly** flag - ํธ๋์ญ์์ด ์ฝ๊ธฐ ์ ์ฉ์ธ์ง
- **Rollback** ๊ท์น

default ์ค์ ์ ์ํด rollback์ด runtime์ ๋ฐ์ํ๊ธฐ ๋๋ฌธ์, **unchecked exception**์ด ๋ฐ์ํ  ์ ์๋ค(checked exception์ rollback์ ํธ๋ฆฌ๊ฑฐํ์ง ์๋๋คโโ). `rollbackFor`์ `noRollbackFor`๋ฅผ ํตํด ์ด๋ฅผ ์ ์ดํ  ์ ์๋ค.

<br>

### Transaction Propagation

๋น์ฆ๋์ค ๋ก์ง๊ณผ ํธ๋์ญ์์ ๊ฒฝ๊ณ๋ฅผ ์ ์ํ๋ ์์ค

Spring์ propagation setting์ ๋ฐ๋ผ ํธ๋์ญ์์ ์์ํ๊ณ  ์ค๋จํ๋ฉฐ ๊ด๋ฆฌํ๋ค.

propagation ๋จ๊ณ์ ๋ฐ๋ผ ํธ๋์ญ์์ ๊ฐ์ ธ์ค๊ฑฐ๋ ์์ฑํ๊ธฐ ์ํด `TransactionManager::getTransaction`์ ํธ์ถํ๋ค.

<br>

### Annotation ์ฌ์ฉ์ ์ ์ฌ์  ์ํ

#### Transaction๊ณผ Proxy

Spring์ `@Transactional`์ด ๋ถ์ ํด๋์ค(๋๋ ๋ฉ์๋)์ ๋ํด ํ๋ก์๋ฅผ ์์ฑํ๋ค. ์ด ํ๋ก์๋ ํธ๋์ญ์์ ์์๊ณผ ์ปค๋ฐ์ ์ํด ๋ฉ์๋๋ฅผ ์คํํ๊ธฐ ์  ํธ๋์ญ์ ๋ก์ง์ ์ฃผ์ํ๋ค.

์ค์ํ ๊ฒ์ ๋ง์ฝ Transactional Bean์ด **Interface๋ฅผ implements**ํ  ๊ฒฝ์ฐ, ๊ธฐ๋ณธ์ผ๋ก `Java Dynamic Proxy`๋ฅผ ์ฌ์ฉํ๊ธฐ ๋๋ฌธ์ ํ๋ก์๋ฅผ ํตํด ๋ค์ด์ค๋ ์ธ๋ถ ๋ฉ์๋ ํธ์ถ๋ง ์ฒ๋ฆฌํ๊ฒ ๋๋ค. ๋ฉ์๋์ `@Transactional`๊ฐ ์๋ ๊ฒฝ์ฐ์๋ ํด๋์ค ๋ด๋ถ ํธ์ถ์ ํธ๋์ญ์์ ์์ํ์ง ์๋๋ค.

๋ํ `public` ๋ฉ์๋์๋ง `@Transactional`์ด ์ ์ฉ๋๋ค. ๋ค๋ฅธ visibility๋ฅผ ๊ฐ์ง ๋ฉ์๋๋ ํ๋ก์ ๋์ง ์๊ธฐ ๋๋ฌธ์ ์ค์ ์ด ๋ฌด์๋๋ค.

<br>

#### Isolation Level ๋ณ๊ฒฝ

```java
@Transactional(isolation = Isolation.SERIALIZABLE)
```

isolation ๋ ๋ฒจ ์ค์ ์ Spring 4.1 ์ดํ์๋ง ์ ์ฉ ๊ฐ๋ฅํ๋ค.

<br>

#### Read-Only Transaction

> `readOnly`๊ฐ write access์๋๊ฐ ๋ฐ๋์ ์คํจํ๋ ๊ฒ์ ์๋๋ค โโ transaction manager๋ read-only๋ฅผ ํด์ํ์ง ๋ชปํ  ๋ read-only ์์ฒญ์ ๋ํด ์์ธ๋ฅผ throwํ์ง ์๋๋ค.

์ฆ, `readOnly`๊ฐ ์ค์ ๋์๋ค๊ณ  ์ฝ์ ๋๋ ์์ ์ด ๋ฐ์ํ์ง ์์ ๊ฒ์ด๋ผ ํ์ ํ  ์ ์๋ค. 

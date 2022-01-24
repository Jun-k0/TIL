# JPA
서로 지향하는 바가 다른 2개 영역(객체지향 프로그래밍 언어와 관계형 데이터베이스)을 중간에서 **패러다임 일치**를 시켜주기 위한 기술

즉, 개발자는 **객체지향적으로 프로그래밍**을 하고, JPA가 이를 관계형 데이터베이스에 맞게 SQL을 대신 생성해서 실행함

## Spring Data JPA
JPA는 인터페이스로서 자바 표준명세서이므로 인터페이스인 JPA를 사용하기 위해서는 구현체가 필요함.

Spring Data JPA는 Hibernate를 쓰는 것과 큰 차이가 없지만, 구현체 교체의 용이성과 저장소 교체의 용이성 두 가지의 장점이 있음.

### 구현체 교체의 용이성
만약 Hibernate가 언젠가 수명을 다하고 새로운 **JPA 구현체**가 대세가 된다면, **Spring Data JPA 내부에서 구현체 매핑을 지원**하기 때문에 아주 쉽게 교체 가능함

### 저장소 교체의 용이성
관계형 데이터베이스 외에 다른 저장소로 쉽게 교체하기 위함

## JpaRepository
* Repository는 DB 접근자이며, 인터페이스로 생성 후 JpaRepository<Entity, PK타입>을 상속하면 기본 CRUD가 자동 생성됨
* @Repository를 추가할 필요도 없다. 주의할 점은 **Entity 클래스와 기본 Entity Repository는 함께 위치**해야 한다. 둘은 아주 밀접해서 Entity는 기본 Repository 없이는 제대로 역할을 할 수가 없다.

```java
public interface PostsRepository extends JpaRepository<Posts, Long>{

}
```
Member 클래스를 예를 들어 기본 메소드들을 공부해보자.

### findAll()
* Member 레코드 전체 목록을 조회
* List< Member > 객체 리턴

### findById(id)
- 기본키 필드 값이 id인 레코드 조회
- Optional< Member > 객체 리턴 (Optional 객체를 .get()하면 Member 객체 리턴)

### save(member)
- Member 객체를 Member 테이블에 저장
- 객체의 id(PK) 속성값이 0이면 INSERT / 0이 아니면 UPDATE

### saveAll(memberList)
- Member 객체 목록을 Member 테이블에 저장

### delete(member)
- Member 객체의 id(PK) 속성값과 일치하는 레코드 삭제

### deleteAll(memberList)
- Member 객체 목록을 테이블에서 삭제

### exits(id)
- Member 테이블에서 id에 해당하는 레코드가 있는지 true/false 리턴

---
기본 메소드 + 메소드 명명 규칙을 이용하면 원하는 SQL 쿼리를 짜준다.

### findByXX
- 이후 엔티티 속성 이름을 붙인다. 첫 글자는 대문자로 한다.

### And / Or
- 조건 추가

### Like / NotLike
- 인수에 지정된 텍스트를 포함 / 포함하지 않는 Entity 검색

### StartingWith / EndingWith
- 인수에 지정된 텍스트로 시작 / 끝나는 Entity 검색

### IsNull / IsNotNull
- 값이 null / not null 인지

### True / False
- 값이 true / false 인지

### Before / After
- 시간 기준으로 전 / 후 값 검색

### LessThan / GreaterThan
- 숫자 기준으로 작은 / 큰 값 검색

### Between
- 두 값 사이에 있는 값 검색

### OrderByXX
- Entity의 속성 XX 기준 검색

## @PersistenceContext
EntityManager로 Entity를 저장, 보관하면 영속성 컨텍스트에 Entity를 관리하는 것이다.

여러 EntityManager가 하나의 영속성 컨테이너를 공유하게 되는 컨테이너 환경(ex. spring)에서 JPA는 개발자가 EntityManager를 직접 생성하지 않고 컨테이너에 위임한다. 

일반적으로 스프링은 싱글톤 기반이기 때문에 속성값을 모든 thread가 공유하게 되므로 여러 thread가 동시에 접근하면 동시성 문제가 발생할 수 있다.

이때 @PersistenceContext를 사용하면 EntityManager를 Proxy를 통해서 감싸주고, EntityManager 호출 때 마다 Proxy를 통해서 EntityManager를 생성하게 하므로 thread-safe를 보장해준다.

## Proxy?
자신과 연관된 엔티티를 **실제로 사용할 때** 연관된 엔티티를 조회하는 것을 **지연로딩** 이라고 한다.

DB를 조회한다고 예를 들어보자. 
지연로딩을 하고 싶을 때, 조회를 미루기 위해서 **가짜 엔티티 객체**를 조회해야하는데, 이것이 **프록시**이다.

프록시는 실제 클래스를 상속받아서 만들어지고, 실제 객체의 참조를 보관하여 대기하고 있다.

## 트랜잭션 범위의 영속성 컨텍스트
스프링 컨테이너는 트랜잭션 범위의 영속성 컨텍스트 전략을 사용한다. 

즉, 트랜잭션이 시작할 때 영속성 컨텍스트를 생성하고 트랜잭션이 끝날 때 영속성 컨텍스트를 끝낸다.

또한, 트랜잭션이 같으면 같은 영속성 컨텍스트를 사용한다.

여러 EntityManager를 사용해도 한 트랜잭션에 묶이면 영속성 컨텍스트를 공유하고,
같은 EntityManager를 사용해도 트랜잭션에 따라 접근하는 영속성 컨텍스트가 다르다. 그러므로 멀티스레드에 안전하게 된다.

## Reference
스프링부트와 aws로 혼자 구현하는 웹서비스 - 저자 이동욱  
https://frogand.tistory.com/22   
https://zara49.tistory.com/130  
https://jaeho214.tistory.com/73  
https://ict-nroo.tistory.com/131  

# Lombok
Lombok(롬복)은 Java 라이브러리로 반복되는 getter, setter, toString 등의 메서드 작성 코드를 줄여주는 코드 다이어트 라이브러리이다.

## Getter & Setter
각 필드들의 get, set 메소드 자동 생성

## RequiredArgsConstructor
* 선언된 모든 final 필드가 포함된 생성자를 생성해줌
* final이 없는 필드는 생성자에 포함되지 않음
* **생성자로 Bean을 주입** 해주어, @Autowired를 생략 가능하게 해준다. (생성자를 쓰지 않고 어노테이션을 쓰면 생성자 코드를 일일히 수정하지 않아도 됨)

## NoArgsConstructor
* 기본 생성자 자동 추가

## Builder
Setter를 무작성 생성하게 되면 해당 클래스의 인스턴스 값들이 언제 어디서 변하는지 코드상에서 명확히 구분할 수 없어 위험하다. 그렇다면, Setter를 쓰지않고 어떻게 값을 채워 DB에 삽입 할까?

기본적으로는 생성자를 통하지만, @Builder를 사용하면 **어느 필드에 어떤 값을 채워야 할지** 명확하게 인지할 수 있다.

```java
Example.builder()
    .a(a)
    .b(b)
    .build();
```

+ JPA에서 @GeneratedValue(strategy = GenerationType.IDENTITY)를 썼을시 batch 기능 비활성화 -> 데이터 한번에 모아서 보내기 불가능.
https://semtax.tistory.com/94

# Reference
스프링부트와 aws로 혼자 구현하는 웹서비스 - 저자 이동욱

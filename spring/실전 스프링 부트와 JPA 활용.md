## dependencies 추가
* Spring Web starter - RESTful, Spring MVC, Tomcat, ...
* Thymeleaf - JSP 대신 쓸 template (view)
* Spring Data JPA
* H2 Database - 간단한 내장 DB 
* Lombok - 코드의 간결화(ex. @Getter, @Setter) // IDE에서 추가 설정 필요

## application.yml 설정
db, jpa, logging 등 설정
```java
spring:
 datasource:
  url: jdbc:h2:tcp://localhost/~/jpashop
  username: sa
  password:
  driver-class-name: org.h2.Driver
 jpa: // 설정 공부!
  hibernate:
    ddl-auto: create
    properties:
  hibernate:
    # show_sql: true
    format_sql: true
logging.level:
  org.hibernate.SQL: debug
  # org.hibernate.type: trace
```
# Reference
김영한님의 실전! 스프링 부트와 JPA 활용

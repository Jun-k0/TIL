## 프로젝트 환경설정
프로젝트 JDK, Gradle JDK를 java 11로 설정.   
bulid and run을 IntelliJ로 -> 실행 속도 빨라짐   
#### 스프링 부트 라이브러리
* spring-boot-starter-web : 톰캣(웹서버) , 스프링 웹 MVC
* spring-boot-starter-thymeleaf : 타임리프 템플릿 엔진(View)
* spring-boot-starter(공통) : 스프링 부트 + 스프링 코어 + 로깅
* spring-boot-starter-test : 테스트 라이브러리
## 데이터 넘기기
### 정적 컨텐츠
해당 url에 해당하는 controller가 없으면 resource/static/ 으로 가 .html을 찾아서 view 출력
### MVC와 템플릿 엔진
```java
@Controller
public class HelloController {
 @GetMapping("hello-mvc")
 public String helloMvc(@RequestParam("name") String name, Model model) {
  model.addAttribute("name", name);
  return "hello-template";
 }
}
```
해당 url에 해당하는 controller가 없으면 resource/templates/ 으로 가 .html을 찾아서 view 출력    
( viewResolver를 사용 )
### API
```java
// @ResponseBody 객체 반환

@Controller
public class HelloController {
 @GetMapping("hello-api")
 @ResponseBody
 public Hello helloApi(@RequestParam("name") String name) {
  Hello hello = new Hello();
  hello.setName(name);
  return hello;
 }
 static class Hello {
  private String name;
  public String getName() {
  return name;
 }
 public void setName(String name) {
  this.name = name;
 }
}
```
```@Responsebody```를 사용하면 viewResolver 대신 HttpMassageConverter가 동작하여 데이터(문자, 객체, ...)를 JSON으로 처리해줌

## 일반적인 웹 어플리케이션 구조
<img src="spring_web.PNG" width="60%" height="60%"></img>
* 컨트롤러 : 웹 MVC의 컨트롤러 역할 (service 이용)
* 서비스 : 핵심 비즈니스 로직 구현 (repository 이용)
* 리포지토리 : 데이터베이스에 접근하여 도에인 객체를 저장하고 관리 (db 접근)
* 도메인 : 비즈니스 도메인 객체 (모델), 데이터베이스에 저장하고 관리됨

## 테스트
* ```@Test```를 사용하여 테스트 클래스 생성
* ```@AfterEach``` : 테스트 간 영향이 없도록, 한 테스트가 끝날때 마다 실행 (리포지토리 비우기)
* ```@BeforeEach``` : 테스트 간 영향이 없도록, 한 테스트가 시작하기 전에 실행 (새 리포지토리 생성, 의존성 주입)

## 스프링 빈과 의존관계
ㅇㅁㄴㅇ
### 컴포넌트 스캔 방법

### 직접 스프링 빈에 등록하는 방법

## 유용한 문법들
```Optional<T>``` - null    
```.stream()```, ```.filter()```, ```.findany()```

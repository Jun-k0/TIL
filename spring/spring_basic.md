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

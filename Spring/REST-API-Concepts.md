# API, REST API, RESTful API, @Controller, @RestController 정리

## 1. API (Application Programming Interface)
- 소프트웨어 구성 요소 간의 상호작용을 정의하는 인터페이스
- API는 다른 소프트웨어와 통신하기 위한 규칙과 프로토콜을 제공함
- ex) 날씨 API, 구글 지도 API, 결졔 API 등


## 2. REST API (Representational State Transfer API)
- REST 아키텍처 스타일을 따르는 API
- HTTP 프로토콜을 사용하여 클라이언트와 서버 간의 통신을 수행함
- RESTful API는 자원(리소스)을 URI로 식별하고, HTTP 메서드(GET, POST, PUT, DELETE 등)를 사용하여 자원에 대한 CRUD(Create, Read, Update, Delete) 작업을 수행함


## 3. REST API vs RESTful API
- REST API는 RESTful API의 약어로, RESTful API와 동일한 의미로 사용됨
- RESTful API는 제대로 설계 원칙을 따르는 API를 의미함
- 즉, REST API는 약간 느슨한 느낌이고, RESTful API는 엄격한 느낌임


## 4. @Controller vs @RestController
- @Controller
  - Spring MVC에서 사용되는 애너테이션
  - 주로 JSP와 같은 뷰를 반환하는 컨트롤러에 사용됨
  - 메서드에서 반환하는 값은 뷰 이름으로 해석됨
  - 예시: `return "index";` -> `index.jsp`를 반환함
  ```java
    @Controller
    public class UserController {
        @GetMapping("/user")
        public String getUser(Model model) {
            model.addAttribute("user", new User("John", "Doe"));
            return "user"; // user.jsp를 반환
        }
    }
  ```


- @RestController
  - Spring 4.0부터 추가된 애너테이션
  - @Controller와 @ResponseBody를 결합한 애너테이션
  - 주로 RESTful API를 구현하는 컨트롤러에 사용됨
  - 메서드에서 반환하는 값은 JSON/XML 형식으로 변환되어 응답 본문에 포함됨
  - 예시: `return new User("John", "Doe");` -> JSON 형식으로 변환되어 응답됨
  ```java
    @RestController
    public class UserRestController {     
        @GetMapping("/user")
        public User getUser() {
            return new User("John", "Doe"); // JSON 형식으로 변환되어 응답됨
        }
    } 
    ```
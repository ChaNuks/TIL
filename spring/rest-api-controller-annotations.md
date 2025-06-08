# Request Annotations in Spring

## 1. @RequestMapping
- HTTP 요청을 처리하는 메서드에 매핑하는 애너테이션
- 클래스 레벨과 메서드 레벨에서 사용할 수 있음
- HTTP 메서드(GET, POST, PUT, DELETE 등)를 지정할 수 있음
- 예시:
```java
@RequestMapping(value = "/users", method = RequestMethod.GET)
public List<User> getUsers() {
    return userService.getAllUsers();
}
```

## 2. @GetMapping, @PostMapping, @PutMapping, @DeleteMapping
- `@RequestMapping`의 축약형 애너테이션
- 각 HTTP 메서드에 해당하는 요청을 처리하는 데 사용됨
- 예시:
```java
@GetMapping("/users")
public List<User> getUsers() {
    return userService.getAllUsers();
}
@PostMapping("/users")
public User createUser(@RequestBody User user) {
    return userService.createUser(user);
}
@PutMapping("/users/{id}")
public User updateUser(@PathVariable Long id, @RequestBody User user) {
    return userService.updateUser(id, user);
}
@DeleteMapping("/users/{id}")
public void deleteUser(@PathVariable Long id) {
    userService.deleteUser(id);
}
```


## 3. @RequestParam
- HTTP 요청 파라미터를 메서드 파라미터에 매핑하는 애너테이션
- 쿼리 파라미터나 폼 데이터에서 값을 추출할 때 사용됨
- 예시 : `/users?name=John`에서 `name` 값을 추출
```java
@GetMapping("/users")
public List<User> getUsers(@RequestParam(required = false) String name) {
    if (name != null) {
        return userService.getUsersByName(name);
    }
    return userService.getAllUsers();
}
```


## 4. @PathVariable
- URL 경로 변수 값을 메서드 파라미터에 매핑하는 애너테이션
- URL 경로에서 동적으로 값을 추출할 때 사용됨
- 예시 : `/users/{id}`에서 `id` 값을 추출
```java
@GetMapping("/users/{id}")
public User getUser(@PathVariable Long id) {
    return userService.getUserById(id);
}
```


## 5. @RequestBody
- HTTP 요청 본문을 메서드 파라미터에 매핑하는 애너테이션
- 주로 JSON, XML 등의 데이터를 처리할 때 사용됨
- 예시 : JSON 형태의 사용자 정보를 객체로 변환하여 전달
```java
@PostMapping("/users")
public User createUser(@RequestBody User user) {
    return userService.createUser(user);
}
```


## 6. 요약
- `@RequestMapping`
  - HTTP 요청을 처리하는 메서드에 매핑
- `@GetMapping`, `@PostMapping`, `@PutMapping`, `@DeleteMapping`
  - 각 HTTP 메서드에 해당하는 요청을 처리


- `@RequestParam`
  - HTTP 요청 파라미터를 메서드 파라미터에 매핑
  - 주로 필터링, 검색 등의 기능을 구현할 때 사용


- `@PathVariable`
  - URL 경로 변수 값을 메서드 파라미터에 매핑
  - 주로 RESTful API에서 자원 식별자(예: ID)를 추출할 때 사용
  - 여러 경로 변수를 사용할 수 있으며, 경로 변수의 이름은 URL 경로와 일치해야 함


- `@RequestBody`
  - HTTP 요청 본문을 메서드 파라미터에 매핑
  - JSON, XML 등의 데이터를 처리할 때 사용
  - 요청 본문을 객체로 변환하여 전달함
  - 단일 객체만 처리 가능하며, 배열이나 리스트는 지원하지 않음
  - 주로 회원가입에 사용되는 경우가 많음


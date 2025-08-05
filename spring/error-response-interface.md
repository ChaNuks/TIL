# ErrorResponse Interface

## ErrorResponse Interface
- Spring Framework 6.x부터 추가된 인터페이스
- 패키지 위치 : org.springframework.web
- Controller와 공통 오류 응답 포맷을 위한 표준화된 인터페이스

## 왜 추가됨?
### 1. 기존 Spring에서는 API 예외 응답 형식이 개발자마자 제각각이었음
- GlobalExceptionHandler마다 @ExceptionHandler에서 반환 구조를 일일이 커스터마이징

### 2. 여기서 문제가 발생
1. 응답 구조 일관 X
2. 표준 포맷이 없어서 문서화/자동화 도구(OpenAPI) 연동 어려움
3. @ResponseStatus와 ResponseEntity 혼용 시 혼란 발생

### 3. 그래서?
- 예외 응답을 표준화된 구조로 설계
- 다양한 예외 상황에 공통 형식 제공 (code, message, status, timestamp)

---

## 내부 구성
```java
public interface ErrorResponse {
    String getCode(); // HTTP 상태 코드
    String getMessage();  // 예외 메시지
    int getStatus();  // HTTP 상태 코드
    LocalDateTime getTimestamp();  // 예외 시간
}
```

---

## 언제 사용하면 좋을까?
### 1. REST API에서 일관된 예외 응답이 필요할 때
- 여러 API에서 예외 발생 시 동일한 구조의 응답 포맷을 유지하고 싶을 때 커스텀 예외 클래스 사용
ex)
```json
{
    "code": "404",
    "message": "Not Found",
    "status": 404,
    "timestamp": "2023-01-01T00:00:00"
}
```

### 2. 문서화/자동화 도구(OpenAPI)와 연동
- OpenAPI 문서화/자동화 도구와 연동 시 공통 예외 구조을 사용
ex)
```yaml
openapi: 3.0.1
info:
    title: Todo API
    version: 1.0.0
servers:
    - url: http://localhost:8080
paths:
    /todos:
        get:
            summary: Retrieve a list of todos
            responses:
                '200':
                    description: A list of todos
                    content:
                        application/json:
                            schema:
                                $ref: '#/components/schemas/TodoList'
components:
    schemas:
        ErrorResponse:
            type: object
            properties:
                code:
                    type: string
                    example: "404"
                message:
                    type: string
                    example: "Not Found"
                status:
                    type: integer
                    example: 404
                timestamp:
                    type: string
                    example: "2023-01-01T00:00:00"
```

---

### 3. ExceptionHandler의 중복 처리 방지
- 각 예외에 대해 일일이 ResponseEntity로 구성할 필요 없이, ErrorResponse를 구현한 객체를 바로 리턴

### 4. HTTP 상태 코드와 메시지를 항상 포함하고 싶을 때
- Client 측에서 오류 발생 시 시점과 상태 코드 기반으로 판단 가능
- 디버깅, 로깅 시에도 유리
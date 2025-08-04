# Common Response Format

## 공통 응답?
- API 응답의 구조를 `일관되게 유지`하기 위한 포맷
- API 응답 결과를 일정한 형식으로 감싸는 구조
  - 성공이든 실패든 항상 같은 틀로 응답
  - ex) 
  ```json 
  {
    "status": 200,
    "message": "요청이 성공했습니다.",
    "data": { ... }
  }
  ```

---

## 그럼 왜 쓸까?
- 프론트엔드가 처리하기 쉬움 (status/message 분리)
- 에러 메시지를 명확히 전달 가능
- Swagger 문서화/코드 자동화에 유리

---

## 예시 코드
```java
@Getter
public class CommonResponse<T> {
    private int status;
    private String message;
    private T data;
    
    public CommonResponse(int status, String message, T data) {
        this.status = status;
        this.message = message;
        this.data = data;
    }
}   
```
```java
public class CommonResponse<T> {
    public static <T> CommonResponse<T> of(int status, String message, T data) {
        return new CommonResponse<>(status, message, data);
    }
}
```

---

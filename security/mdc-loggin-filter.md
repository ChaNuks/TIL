# MDC 로딩 필터

## MDC(Mapped Diagnostic Context) 로깅 필터란?
- MDC는 `하나의 HTTP 요청 단위로 로그 추적을 가능하게 해주는 로깅 기능`
- MDC를 사용하면 `각 요청에 대한 고유한 식별자`를 로그에 추가할 수 있어, `분산 시스템에서의 로그 추적`이 용이해짐
- `ThreadLocal`을 기반으로 하며, **현재 요청과 관련된 고유 정보**(예: requestId, URI 등)를 담아 로그 출력에 활용할 수 있음
- Logback, Log4j2 등에서 `MDC.put()` 으로 값을 저장하고, `%X{key}`로 로그에 출력 가능
- 예를 들어, `MDC.put("requestId", UUID.randomUUID().toString())`로 요청 ID를 저장하고, 로그 패턴에서 `%X{requestId}`로 출력할 수 있음

---

## 왜 사용할까?
- 여러 사용자가 동시에 요청을 보내는 서버 환경에서 `로그를 구분하기 힘듦`
- MDC를 사용하면 각 요청에 대한 고유한 식별자를 로그에 추가하여 `요청 단위로 로그를 추적`할 수 있음
- 예를 들어, `requestId`를 MDC에 저장하고 로그에 출력하면, 특정 요청의 로그를 쉽게 찾을 수 있음
- 로그마다 `requestId`, `URI`, `userAgent` 등을 자동으로 출력하여 
  - `문제 발생 시 해당 요청의 로그를 쉽게 추적 가능`
  - `요청 단위로 로그를 필터링하거나 분석할 수 있어, 성능 모니터링 및 디버깅에 유용함`
- 또한, `분산 시스템`에서 여러 서비스 간의 로그를 연관 지을 수 있어, 문제 해결이 용이해짐

---

## 그래서 어떻게 사용하는데?
### 1. 의존성 추가
```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-web</artifactId>
</dependency>
```

### 2. MDC 로딩 필터 구현
```java
import org.slf4j.MDC;
import org.springframework.stereotype.Component;
import jakarta.servlet.Filter;
import jakarta.servlet.FilterChain;
import jakarta.servlet.FilterConfig;
import jakarta.servlet.ServletException;
import jakarta.servlet.ServletRequest;
import jakarta.servlet.ServletResponse;
import jakarta.servlet.http.HttpServletRequest;
import java.io.IOException;
import java.util.UUID;
import org.springframework.web.filter.OncePerRequestFilter;

@Component
public class MdcLoggingFilter extends OncePerRequestFilter {

    @Override
    protected void doFilterInternal(HttpServletRequest request, 
                                    ServletResponse response, 
                                    FilterChain filterChain) throws ServletException, IOException {
        try {
            // 요청 ID 생성 및 MDC에 저장
            String requestId = UUID.randomUUID().toString(); // 고유한 요청 ID 생성
            MDC.put("requestId", requestId);                 // 요청 ID를 MDC에 저장
            MDC.put("uri", request.getRequestURI());         // 요청 URI를 MDC에 저장
            MDC.put("method", request.getMethod());          // 요청 메서드를 MDC에 저장
            
            // 필터 체인 진행
            filterChain.doFilter(request, response); // 다음 필터 또는 컨트롤러로 요청 전달
        } finally {
            // MDC에서 값 제거
            MDC.clear(); // 요청 처리 후 MDC 값을 제거하여 메모리 누수 방지
        }
    }
}
```

### 3. 로그 패턴 설정
```yaml
logging:
  pattern:
    console: '%d{yyyy-MM-dd HH:mm:ss} - %X{requestId} - %X{uri} - %X{method} - %msg%n'
```


### 4. 로그 출력 예시
```java
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
public class MyController {
    private static final Logger logger = LoggerFactory.getLogger(MyController.class);

    public void handleRequest() {
        logger.info("Handling request");
        // 로그 출력 예시: 2023-10-01 12:00:00 - 123e4567-e89b-12d3-a456-426614174000 - /api/example - GET - Handling request
    }
}
```

### 5. 주의사항
- MDC는 `ThreadLocal`을 사용하므로, 멀티스레드 환경에서 주의가 필요함
- 필터가 요청 단위로 MDC를 설정하고, 요청 처리 후 반드시 `MDC.clear()`를 호출하여 값을 제거해야 함
- MDC에 저장된 값은 `현재 스레드`에만 적용되므로, 다른 스레드에서는 접근할 수 없음
- MDC를 사용하여 로그를 남길 때는 `동기화`에 주의해야 하며, 멀티스레드 환경에서는 `각 스레드`가 독립적으로 MDC를 관리하도록 해야 함
- MDC에 저장된 값은 `요청 단위`로 관리되므로, 요청이 끝난 후 반드시 `MDC.clear()`를 호출하여 값을 제거해야 함

---

## 정리
| 항목          | 설명                                                         |
|---------------|------------------------------------------------------------|
| MDC 로깅 필터 | 요청 단위로 로그 추적을 가능하게 해주는 로깅 기능                |
| 사용 이유      | 멀티스레드 환경에서 요청 단위로 로그를 구분하고 추적하기 위함         |
| 구현 방법      | `OncePerRequestFilter`를 상속받아 요청 ID, URI 등을 MDC에 저장하고 로그 출력 |
| 로그 패턴 설정 | `application.yml`에서 MDC 값을 포함한 로그 패턴 설정                |
| 주의사항       | 멀티스레드 환경에서 MDC를 사용 시 주의, 요청 처리 후 반드시 `MDC.clear()` 호출 |
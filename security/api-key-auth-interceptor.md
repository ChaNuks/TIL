## API 키 인증 인터셉터

## API Key Authentication Interceptor란?
- API 요청에 포함된 `API 키를 검증하여 인증을 수행하는 컴포넌트`
- 클라이언트와 서버 간의 신뢰를 구축하고, 인증된 클라이언트만 API에 접근할 수 있도록 함
- 일반적으로 클라이언트 애플리케이션에 의해 생성되며, 서버는 이 키를 사용하여 클라이언트를 식별하고 인증함
- 보통 HTTP 헤더, 쿼리 파라미터, 또는 요청 본문에 포함되어 전송됨

---

## 왜 사용하는가?
- `보안 강화`
  - 인증된 클라이언트만 API에 접근할 수 있도록 하여 보안을 강화함
  - ex) 민감한 데이터나 기능에 대한 접근을 제한
- `접근 제어`
  - 특정 클라이언트에게만 API 접근 권한을 부여하거나 제한할 수 있음
  - ex) API 키를 통해 클라이언트의 권한을 관리
- `트래픽 관리`
  - 클라이언트별로 트래픽을 모니터링하고, 과도한 요청을 제한할 수 있음
  - ex) API 키를 사용하여 클라이언트의 요청 수를 제한하거나, 속도를 조절
- `사용자 추적`
  - 클라이언트의 요청을 추적하고, 사용 패턴을 분석할 수 있음
  - ex) API 키를 통해 클라이언트의 요청 로그를 기록하고, 분석하여 서비스 개선에 활용
---

## 어떻게 사용하는데?
**1. API Key 생성** 
  - 클라이언트 애플리케이션에서 API 키를 생성하거나, 서버에서 클라이언트에게 API 키를 발급함
  - API 키는 일반적으로 고유한 문자열로 구성됨
  

**2. API Key 전달**
  - 클라이언트는 API 요청 시 API 키를 HTTP 헤더, 쿼리 파라미터, 또는 요청 본문에 포함하여 서버로 전송함
  - 예시: HTTP 헤더에 API 키를 포함하는 경우
    ```http
    GET /api/resource HTTP/1.1
    Host: example.com
    Authorization: ApiKey your_api_key_here
    ```
    
**3. API Key 검증**
  - 서버는 요청을 수신하면 API 키를 추출하고, 해당 키가 유효한지 검증함
  - 검증 과정에서 API 키의 존재 여부, 형식, 만료 여부 등을 확인함
  - 유효한 API 키인 경우 요청을 처리하고, 그렇지 않은 경우 인증 오류를 반환함
  - 예시: API 키가 유효하지 않은 경우, 서버는 다음과 같은 응답을 반환함
    ```json
    {
      "status": "error",
      "message": "Invalid API Key"
    }
    ```

**4. 요청 처리**
  - API 키가 유효한 경우, 서버는 요청을 처리하고 응답을 반환함
  - 이 과정에서 API 키를 사용하여 클라이언트를 식별하고, 필요한 권한을 확인함
  - 예시: API 키가 유효한 경우, 서버는 요청에 대한 데이터를 반환함
    ```json
    {
      "status": "success",
      "data": {
        "message": "Hello, authenticated user!"
      }
    }
    ```
---

## API Key 인증 인터셉터 구현 예시
- 인터셉터 등록 (WebMvcConfig 구현)
```java
import org.springframework.context.annotation.Configuration;
import org.springframework.web.servlet.config.annotation.InterceptorRegistry;
import org.springframework.web.servlet.config.annotation.WebMvcConfigurer;

@Configuration
public class WebMvcConfig implements WebMvcConfigurer {
    
    private final LoggingInterceptor loggingInterceptor;

    @Override
    public void addInterceptors(InterceptorRegistry registry) {
        registry.addInterceptor(new ApiKeyAuthInterceptor())
                .addPathPatterns("/v3/**") // API 키 인증이 필요한 경로 설정
                .order(1); // 인터셉터 순서 설정
    }
}
``` 

- API Key 인증 인터셉터 구현
```java
import jakarta.servlet.http.HttpServletRequest;
import jakarta.servlet.http.HttpServletResponse;
import lombok.extern.slf4j.Slf4j;
import org.springframework.stereotype.Component;
import org.springframework.web.servlet.HandlerInterceptor;
import org.springframework.web.servlet.ModelAndView;

@Slf4j
@Component
public class LoggingInterceptor implements HandlerInterceptor {

    @Override
    public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) throws Exception {
        log.info(">>> {} {}", request.getMethod(), request.getRequestURI());
        log.info("User-Agent: {}", request.getHeader("User-Agent"));

        request.setAttribute("start-time", System.currentTimeMillis());
        return true;
    }

    @Override
    public void postHandle(HttpServletRequest request, HttpServletResponse response, Object handler, ModelAndView modelAndView) throws Exception {
        log.info("<<< 응답 완료, Status: {}", response.getStatus());
    }

    @Override
    public void afterCompletion(HttpServletRequest request, HttpServletResponse response, Object handler, Exception ex) throws Exception {
        long startTime = (long) request.getAttribute("start-time");
        long endTime = System.currentTimeMillis();

        log.info("총 소요 시간: {}ms", endTime - startTime);
    }

}
```
---

## 정리
| 항목                | 설명                                                         |
|---------------------|--------------------------------------------------------------|
| API Key 인증 인터셉터 | API 요청에 포함된 API 키를 검증하여 인증을 수행하는 컴포넌트 |
| 사용 목적            | 보안 강화, 접근 제어, 트래픽 관리, 사용자 추적 등                |
| 구현 방법            | API Key 생성, 전달, 검증, 요청 처리 순서로 진행                |

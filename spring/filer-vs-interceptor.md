# 🌐 Filter vs Interceptor

Spring에서 요청을 가로채고 처리 흐름에 관여하는 두 가지 주요 컴포넌트인 `Filter`와 `Interceptor`의 개념과 차이점을 정리합니다.

---

## 1. Filter (javax.servlet.Filter)

- **Servlet 스펙에 정의된 기능**
- **DispatcherServlet 이전**에 실행됨
- Spring 외의 다른 웹 프레임워크에서도 사용 가능
- 요청(Request) 또는 응답(Response)을 **변경**할 수 있음

### ✅ 주요 용도
- 인코딩 설정
- 인증 처리
- 로깅
- XSS 방어 등

### ✅ 코드 예시
```java
@WebFilter(urlPatterns = "/*")
public class LogFilter implements Filter {
    @Override
    public void doFilter(ServletRequest request, ServletResponse response, FilterChain chain)
        throws IOException, ServletException {
        HttpServletRequest httpRequest = (HttpServletRequest) request;
        System.out.println("Filter 실행: " + httpRequest.getRequestURI());
        chain.doFilter(request, response);
    }
}
```
---
## 2. Interceptor (org.springframework.web.servlet.HandlerInterceptor)

- **Spring MVC에 특화된 기능**
- `DispatcherServlet` 이후에, `Controller` 진입 전/후에 실행됨
- Spring의 요청 처리 흐름에만 적용됨
- 요청 처리 전후에 **비즈니스 로직**을 추가할 수 있음
- `@Controller`와 함께 사용하여 특정 컨트롤러에만 적용 가능

### ✅ 주요 용도
- 인증 및 권한 검사
- 로깅
- 트랜잭션 관리
- 요청 처리 전후의 추가 작업
- 응답 수정
- 예외 처리

### ✅ 코드 예시
```java
public class LogInterceptor implements HandlerInterceptor {
    @Override
    public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler)
        throws Exception {
        System.out.println("Interceptor 실행: " + request.getRequestURI());
        return true; // true를 반환하면 다음 인터셉터나 컨트롤러로 진행
    }

    @Override
    public void postHandle(HttpServletRequest request, HttpServletResponse response, Object handler,
                           ModelAndView modelAndView) throws Exception {
        // 컨트롤러 실행 후 추가 작업
    }

    @Override
    public void afterCompletion(HttpServletRequest request, HttpServletResponse response, Object handler,
                                Exception ex) throws Exception {
        // 요청 처리 완료 후 작업
    }
}
```
---
## 3. Filter vs Interceptor 비교
| 항목                | Filter                                                   | Interceptor                             |
|---------------------|----------------------------------------------------------|-----------------------------------------|
| 실행 시점           | Servlet 요청 처리 전후                                         | DispatcherServlet 요청 처리 전후        |
| 적용 범위           | Servlet 스펙에 정의, Spring 외 사용 가능                           | Spring MVC에 특화, Spring 내에서만 사용 |
| 요청/응답 변경 가능 | 가능                                                       | 가능                                    |
| 비즈니스 로직 추가    | 제한적 (주로 요청/응답 처리)                                        | 가능 (컨트롤러 실행 전후)                |
| 설정 방법           | `web.xml` 또는 `@WebFilter` 어노테이션, `FilterRegistrationBean` | `@Configuration` 클래스에서 `addInterceptors` 메서드 사용 |
| 예외 처리           | Servlet 예외 처리 방식 사용                                      | Spring의 예외 처리 메커니즘 사용        |

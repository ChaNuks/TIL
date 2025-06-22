# System.out.println()의 synchronized와 System 클래스 사용 시 주의사항

## System.out.println()의 synchronized
- `System.out.println()`은 내부적으로 `PrintStream` 클래스의 `println()` 메서드를 호출함
- 내부적으로 `synchronized` 키워드가 사용되어, 멀티스레드 환경에서도 안전하게 출력할 수 있도록 보장함
- 내부 구조
```java
public class PrintStream {
    public void println(String x) {
        synchronized (this) {
            print(x);
            newLine();
        }
    }
}
```
---

## 문제점
- `System.out.println()`은 멀티스레드 환경에서 동기화되어 있지만, 성능 저하가 발생할 수 있음
- 여러 스레드가 동시에 `println()`을 호출하면, 출력이 순차적으로 처리되어 병목 현상이 발생할 수 있음
- 특히, 대량의 로그를 출력할 때 성능 저하가 심각해질 수 있음
---

## 대안
- 멀티스레드 환경에서 로그를 출력할 때는 `java.util.logging` 또는 `log4j`와 같은 로깅 프레임워크를 사용하는 것이 좋음
- 이러한 프레임워크는 비동기 로깅, 로그 레벨 설정, 출력 형식 지정 등 다양한 기능을 제공하여 성능과 유연성을 향상시킬 수 있음
- 예시: slf4j와 logback을 사용하는 방법
```java
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
public class MyClass {
    private static final Logger logger = LoggerFactory.getLogger(MyClass.class);

    public void myMethod() {
        logger.info("This is an info message");
        logger.error("This is an error message");
    }
}
```

# Exception

## 예외 클래스 계층 구조
~~~
java.lang.Object
└── java.lang.Throwable
├── java.lang.Error       // 시스템 에러 (프로그램에서 잡지 않음)
└── java.lang.Exception   // 프로그램에서 처리해야 하는 예외
└── java.lang.RuntimeException // 실행 중 발생하는 예외
~~~

---

### ✅ Throwable

- Java에서 **모든 에러와 예외의 최상위 클래스**
- 두 가지 하위 클래스:
    - `Error` : 시스템에서 발생, 복구 불가능한 에러
    - `Exception` : 애플리케이션에서 복구 가능한 예외

---

### ✅ Error (에러)
- JVM 자체의 문제 (예: 메모리 부족, StackOverflow 등)
- `OutOfMemoryError`, `StackOverflowError`
- 일반적으로 **catch 하지 않음**  

### ✅ Exception (예외)
- 애플리케이션에서 발생하는 예외
- **Checked Exception**과 **Unchecked Exception**으로 나뉨 

---
### ✅ checked Exception
- 컴파일 시점에서 예외 처리(try-catch 또는 throws)
- `IOException`, `SQLException`, `ClassNotFoundException` 등


### ✅ unchecked Exception
- 컴파일 시점에서 검사하지 않는 예외 (런타임 시점에서 예외 처리)
- `NullPointerException`, `IndexOutOfBoundsException`, `NumberFormatException` 등

---

## 사용자 정의 예외

```java
public class MyException extends RuntimeException {
    public MyException(String message) {
        super(message);
    }
}
```

# Identity vs Equality

## 1. 동일성(Identity)란?
- 객체가 메모리 상에서 `동일한 위치를 참조`하는지를 판단
- 즉, 두 객체가 `같은 객체`인지 여부를 확인
- `==` 연산자를 사용하여 동일성을 비교

### 예시
```java
public class IdentityExample {
    public static void main(String[] args) {
        String str1 = new String("Hello");
        String str2 = new String("Hello");

        // 동일성 비교
        System.out.println(str1 == str2); // false, 서로 다른 객체를 참조
    }
}
```
---

## 2. 동등성(Equality)란?
- 객체의 `값이 같은지`를 판단
- 즉, 두 객체가 `동일한 값을 가지는지` 여부를 확인
- `equals()` 메서드를 사용하여 동등성을 비교

### 예시
```java
public class EqualityExample {
    public static void main(String[] args) {
        String str1 = new String("Hello");
        String str2 = new String("Hello");

        // 동등성 비교
        System.out.println(str1.equals(str2)); // true, 값이 동일
    }
}
```
---

## 3. 동일성과 동등성의 차이점
| 구분       | 동일성 (Identity)                | 동등성 (Equality)               |
|------------|----------------------------------|---------------------------------|
| 비교 연산자 | `==`                              | `equals()`                      |
| 비교 대상  | 객체의 메모리 주소 (참조)         | 객체의 값 (내용)                 |
| 결과       | 두 객체가 동일한 메모리 위치를 참조하는지 여부 | 두 객체의 값이 동일한지 여부        |
---

## 4. 예제 코드
```java
public class IdentityVsEquality {
    public static void main(String[] args) {
        // 동일성 예제
        String str1 = new String("Hello");
        String str2 = new String("Hello");

        // 동일성 비교
        System.out.println("동일성 비교 (str1 == str2): " + (str1 == str2)); // false

        // 동등성 비교
        System.out.println("동등성 비교 (str1.equals(str2)): " + str1.equals(str2)); // true

        // 동일한 객체를 참조하는 경우
        String str3 = str1;

        // 동일성 비교
        System.out.println("동일성 비교 (str1 == str3): " + (str1 == str3)); // true
    }
}
```

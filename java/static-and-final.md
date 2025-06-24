# Static과 Final

## Static
- 정적 멤버는 클래스가 메모리에 로드될 때 생성
- 정적 메서드는 클래스의 인스턴스 생성 없이 호출 가능

### static 변수
- 클래스의 모든 인스턴스가 공유하는 변수
- 클래스 이름으로 접근
- 정적 변수는 클래스가 메모리에 로드될 때 생성되며, 모든 인스턴스가 동일한 값을 공유
- 정적 변수는 클래스가 메모리에서 제거될 때까지 유지됨
```java
public class StaticExample {
    static int staticVariable = 0; // 정적 변수

    public static void main(String[] args) {
        System.out.println(StaticExample.staticVariable); // 클래스 이름으로 접근
        StaticExample.staticVariable = 10; // 정적 변수 값 변경
        System.out.println(StaticExample.staticVariable); // 10 출력
    }
}
```

### static 메서드
- 클래스의 인스턴스 없이 호출할 수 있는 메서드
- 정적 메서드는 클래스 이름으로 호출
- 정적 메서드는 인스턴스 변수나 인스턴스 메서드에 접근할 수 없음
- 정적 메서드는 클래스 로딩 시점에 메모리에 할당됨
```java
public class StaticMethodExample {
    static void staticMethod() { // 정적 메서드
        System.out.println("This is a static method.");
    }

    public static void main(String[] args) {
        StaticMethodExample.staticMethod(); // 클래스 이름으로 호출
    }
}
```
---

## Final
- 변수, 메서드, 클래스에 적용할 수 있는 키워드
- `final`로 선언된 변수는 값을 변경할 수 없음
- `final`로 선언된 메서드는 오버라이드할 수 없음
- `final`로 선언된 클래스는 상속할 수 없음
- `final` 변수는 초기화 후 값을 변경할 수 없으며, 상수로 사용됨

### final 변수
- 값을 한 번만 할당할 수 있는 변수
- 초기화 후 값을 변경할 수 없음
- 예시
```java
public class FinalVariableExample {
    final int finalVariable = 100; // final 변수

    public void changeValue() {
        // finalVariable = 200; // 컴파일 오류: final 변수는 값을 변경할 수 없음
    }

    public static void main(String[] args) {
        FinalVariableExample example = new FinalVariableExample();
        System.out.println(example.finalVariable); // 100 출력
    }
}
```

### final 메서드
- 오버라이드할 수 없는 메서드
- 상속받은 클래스에서 해당 메서드를 재정의할 수 없음
- 예시
```java
public class FinalMethodExample {
    final void finalMethod() { // final 메서드
        System.out.println("This is a final method.");
    }

    public static void main(String[] args) {
        FinalMethodExample example = new FinalMethodExample();
        example.finalMethod(); // This is a final method. 출력
    }
}
```

### final 클래스
- 상속할 수 없는 클래스
- 해당 클래스를 상속받은 클래스는 존재하지 않음
- 예시
```java
public final class FinalClassExample {
    public void display() {
        System.out.println("This is a final class.");
    }

    public static void main(String[] args) {
        FinalClassExample example = new FinalClassExample();
        example.display(); // This is a final class. 출력
    }
}
```
---
## Static과 Final 요약
| 구분         | Static                                      | Final                                       |
|--------------|---------------------------------------------|---------------------------------------------|
| 변수/메서드/클래스 | 클래스 로딩 시점에 메모리에 할당됨               | 한 번만 할당 가능, 이후 변경 불가                |
| 접근 방식     | 클래스 이름으로 접근 가능                        | 인스턴스 없이 접근 가능, 오버라이드 불가            |
| 사용 목적     | 클래스의 모든 인스턴스가 공유하는 데이터나 메서드 정의 | 상수, 오버라이드 방지, 상속 방지                   |

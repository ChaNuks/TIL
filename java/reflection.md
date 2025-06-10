#  자바 리플렉션 (Reflection)

## 1. 리플렉션이란?

- **실행 중(runtime)** 클래스나 객체에 대한 정보를 조사하거나 조작할 수 있게 해주는 기능
- 컴파일 시점이 아니라 실행 시점에 클래스, 메서드, 필드 등에 접근

> 주로 프레임워크(Spring, Hibernate 등) 내부에서 의존성 주입, 어노테이션 처리 등에 사용

---

## 2. 주요 기능

| 기능                | 설명                                                             |
|---------------------|------------------------------------------------------------------|
| 클래스 정보 조회     | 클래스 이름, 메서드, 필드, 생성자 등 정보를 얻음                 |
| 필드 접근/변경       | private 필드도 접근하여 값을 읽거나 변경 가능                    |
| 메서드 호출          | 이름만 알고 있는 메서드를 실행 가능                              |
| 인스턴스 생성        | 클래스 이름만으로 객체 생성 (생성자 접근)                         |
| 어노테이션 처리      | 클래스나 메서드에 선언된 어노테이션 정보 접근                    |

---

## 3. 예제 코드 (기본적인 사용법)

```java
public class Person {
    private String name = "홍길동";

    public void sayHello() {
        System.out.println("안녕하세요");
    }
}
```

---

## 4. 테스트에서의 사용법

### 4-1. 주요 메서드
- **setField()**: 필드의 값을 변경
- **getField()**: 필드의 값을 찾아오기
- **setStaticField()**: 정적 필드의 값을 변경
- **getStaticField()**: 정적 필드의 값을 찾아오기
- **setStaticMethod()**: 정적 메서드의 실행
- **getStaticMethod()**: 정적 메서드의 실행
- **setMethod()**: 메서드의 실행
- **getMethod()**: 메서드의 실행

```java
Class personClass = Class.forName("Person");
Object person = personClass.newInstance();
Method sayHelloMethod = personClass.getMethod("sayHello");
sayHelloMethod.invoke(person);
```
```
ReflectionTestUtils.setField(obj, "name", "테스트");
String name = (String) ReflectionTestUtils.getField(obj, "name");
```
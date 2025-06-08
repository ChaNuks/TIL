# Lombok

## Lombok이란?
- Lombok은 Java의 보일러플레이트 코드를 줄여주는 라이브러리
- 주로 Getter, Setter, toString, equals, hashCode 등의 메서드를 자동으로 생성해줌
- 어노테이션 기반으로 동작하여, 코드의 가독성을 높이고 유지보수를 용이하게 함


## Lombok을 쓰는 이유?
- **보일러플레이트 코드 감소**: 반복적인 코드 작성을 줄여줌
- **가독성 향상**: 코드가 간결해져 가독성이 높아짐
- **유지보수 용이**: 코드 변경 시, 자동으로 메서드가 생성되므로 유지보수가 쉬워짐
- **생산성 향상**: 개발자가 비즈니스 로직에 집중할 수 있도록 도와줌
- **IDE 통합**: IntelliJ IDEA, Eclipse 등 주요 IDE에서 지원하여 개발 편의성을 높임


## Lombok 어노테이션
| 어노테이션          | 설명                                                         |
|---------------------|--------------------------------------------------------------|
| `@Getter`           | 클래스의 필드에 대한 Getter 메서드를 자동으로 생성            |
| `@Setter`           | 클래스의 필드에 대한 Setter 메서드를 자동으로 생성            |
| `@ToString`         | 클래스의 필드 값을 포함한 toString 메서드를 자동으로 생성     |
| `@EqualsAndHashCode` | 클래스의 필드 값을 기반으로 equals와 hashCode 메서드를 자동으로 생성 |
| `@NoArgsConstructor` | 매개변수가 없는 생성자를 자동으로 생성                      |
| `@AllArgsConstructor` | 모든 필드를 매개변수로 받는 생성자를 자동으로 생성          |
| `@Data`             | Getter, Setter, toString, equals, hashCode 메서드를 자동으로 생성 |
| `@Value`            | 불변 클래스(immutable class)를 생성, 모든 필드를 final로 선언하고 Getter만 생성 |
| `@Builder`          | 빌더 패턴을 적용하여 객체 생성 시 유연성을 제공               |
| `@Slf4j`            | 로깅을 위한 Slf4j Logger를 자동으로 생성                     |
| `@NonNull`          | 매개변수나 필드가 null이 아니어야 함을 명시, null 체크를 자동으로 추가 |

## @Data vs @Value
- `@Data`
  - 클래스에 대한 Getter, Setter, toString, equals, hashCode 메서드를 자동으로 생성
  - 클래스가 변경 가능한(mutable) 경우 사용
  - 불변성 X : 모든 필드가 final로 선언되지 않음
  - 예시:
    ```java
    @Data
    public class User {
        private String name;
        private int age;
        private String email;
    // Getter, Setter, toString, equals, hashCode 메서드가 자동으로 생성됨
    }
    ```
- `@Value`
  - 클래스에 대한 Getter, toString, equals, hashCode 메서드를 자동으로 생성하되, Setter는 생성하지 않음
  - 클래스가 변경 불가능한(immutable) 경우 사용
  - 불변성 O : 모든 필드가 final로 선언됨
  - 예시:
    ```java
    @Value
    public class User {
        String name;
        int age;
    // Getter, toString, equals, hashCode 메서드가 자동으로 생성됨
    }
    ```
    

## @Data vs @Value 요약
| 구분               | @Data                                      | @Value                                     |
|--------------------|--------------------------------------------|--------------------------------------------|
| 생성자 종류         | 모든 필드를 매개변수로 받는 생성자 생성       | 모든 필드를 매개변수로 받는 생성자 생성       |
| 필드 초기화 방식     | 필드가 변경 가능(mutable)                   | 필드가 변경 불가능(immutable)                |
| Getter/Setter      | Getter, Setter 메서드 생성                  | Getter 메서드만 생성, Setter 없음            |


## 그런데 웬만하면 지양하자던데?
- `@Data`와 `@Value`는 편리하지만, 모든 상황에서 사용하기 적합하지 않음
- `@Data`는 모든 필드에 대해 Setter를 생성하므로, 객체의 상태가 변경될 수 있는 경우에만 사용해야 함
- `@Value`는 불변성을 보장하지만, 모든 필드가 final로 선언되어야 하므로, 객체의 상태가 변경될 필요가 있는 경우에는 적합하지 않음
- 또한, `@Data`와 `@Value`는 클래스의 모든 필드에 대해 equals와 hashCode를 자동으로 생성하므로, 성능에 영향을 줄 수 있음
- 따라서, 필요한 경우에만 사용하고, 복잡한 비즈니스 로직이 있는 클래스에서는 명시적으로 메서드를 작성하는 것이 좋음


## @NoArgsConstructor vs @AllArgsConstructor
- `@NoArgsConstructor`
  - 매개변수가 없는 기본 생성자를 자동으로 생성
  - 주로 JPA 엔티티 클래스나 직렬화/역직렬화에 사용됨
  - 예시:
    ```java
    @NoArgsConstructor
    public class User {
        private String name;
        private int age;
    // 매개변수가 없는 생성자가 자동으로 생성됨
    }
    ```


- `@AllArgsConstructor`
  - 모든 필드를 매개변수로 받는 생성자를 자동으로 생성
  - 객체 생성 시 모든 필드를 초기화할 수 있도록 함
  - 예시:
    ```java
    @AllArgsConstructor
    public class User {
        private String name;
        private int age;
    // 모든 필드를 매개변수로 받는 생성자가 자동으로 생성됨
    }
    ```

## @NoArgsConstructor vs @AllArgsConstructor 요약
| 구분               | @NoArgsConstructor                          | @AllArgsConstructor                          |
|--------------------|--------------------------------------------|---------------------------------------------|
| 생성자 종류         | 매개변수가 없는 기본 생성자 생성             | 모든 필드를 매개변수로 받는 생성자 생성       |
| 필드 초기화 방식     | 필드 초기화 없음 (기본값으로 초기화)          | 모든 필드를 매개변수로 받아 초기화             |
| 사용 용도           | JPA 엔티티, 직렬화/역직렬화에 주로 사용       | 객체 생성 시 모든 필드를 초기화할 때 사용      |

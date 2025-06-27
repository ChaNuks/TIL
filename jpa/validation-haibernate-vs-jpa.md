# Hibernate vs JPA Validation

## Hibernate Validation?
- Java Bean Validation의 구현체로, 객체의 유효성을 검사하는 기능을 제공
- JPA와 함께 사용되어 엔티티 클래스의 필드에 대한 유효성 검사를 수행
- 주로 `@Valid`, `@Validated` 어노테이션을 사용하여 유효성 검사를 수행
- Spring Boot에서는 `spring-boot-starter-validation` 의존성을 추가하여 사용
- Spring에서는 주로 **DTO/Entity** 클래스에 유효성 검사를 적용하여, 클라이언트로부터 받은 입력값의 유효성 검사
```java
import javax.validation.constraints.NotNull;
import javax.validation.constraints.Size;
public class User {
    @NotNull(message = "Name cannot be null")
    @Size(min = 2, max = 30, message = "Name must be between 2 and 30 characters")
    private String name;

    @NotNull(message = "Email cannot be null")
    @Email(message = "Email should be valid")
    private String email;
}
```
---
## JPA Validation?
- JPA(Java Persistence API)는 객체-관계 매핑(ORM) 기술로, 데이터베이스와 객체 간의 매핑을 제공
- JPA 자체에는 유효성 검사 기능이 포함되어 있지 않지만, Hibernate Validation과 함께 사용하여 엔티티 클래스의 필드에 대한 유효성 검사를 수행할 수 있음
- JPA 엔티티 클래스에 Hibernate Validation 어노테이션을 사용하여 유효성 검사를 적용
- JPA에서는 주로 **엔티티** 클래스에 유효성 검사를 적용하여, 데이터베이스에 저장되는 객체의 유효성 검사
```java
import javax.persistence.Entity;
import javax.persistence.Id;
import javax.validation.constraints.NotNull;
import javax.validation.constraints.Size;
@Entity
public class User {
    @Id
    private Long id;

    @NotNull(message = "이름은 null일 수 없습니다")
    @Size(min = 2, max = 30, message = "이름은 2자 이상 30자 이하이어야 합니다")
    private String name;

    @NotNull(message = "이메일은 null일 수 없습니다")
    @Email(message = "이메일 형식이 올바르지 않습니다")
    private String email;
}
```
---

## Hibernate vs JPA Validation 비교
| **특징**           | **Hibernate Validation**                          | **JPA Validation**                               |
|--------------------|---------------------------------------------------|--------------------------------------------------|
| **유효성 검사 위치** | DTO/Entity 클래스에 적용                          | JPA 엔티티 클래스에 적용                          |
| **유효성 검사 어노테이션** | `@Valid`, `@Validated` 등 사용                     | Hibernate Validation 어노테이션 사용             |
| **유효성 검사 대상** | 클라이언트 입력값의 유효성 검사                    | 데이터베이스에 저장되는 객체의 유효성 검사         |
| **의존성**         | `spring-boot-starter-validation` 의존성 필요       | JPA와 함께 Hibernate Validation 사용              |
| **유효성 검사 실행 시점** | 컨트롤러에서 요청 처리 시점에 유효성 검사 수행       | JPA 엔티티가 데이터베이스에 저장되기 전 유효성 검사 수행 |
| **유효성 검사 결과 처리** | 유효성 검사 실패 시 예외 발생, 적절한 응답 반환         | 유효성 검사 실패 시 예외 발생, 트랜잭션 롤백         |
| **유효성 검사 메시지** | 어노테이션의 `message` 속성을 사용하여 커스터마이징 가능 | 어노테이션의 `message` 속성을 사용하여 커스터마이징 가능 |
| **유효성 검사 그룹** | `@GroupSequence` 등을 사용하여 그룹화 가능          | JPA 엔티티 클래스에서 그룹화 가능                   |
| **유효성 검사 커스터마이징** | 커스텀 유효성 검사 어노테이션 생성 가능            | JPA 엔티티 클래스에서 커스텀 유효성 검사 어노테이션 생성 가능 |
| **유효성 검사 통합** | Spring의 유효성 검사와 통합 가능                   | JPA와 Hibernate Validation 통합 가능              |
| **유효성 검사 성능** | 유효성 검사 로직이 복잡할 경우 성능 저하 가능         | JPA 엔티티의 복잡성에 따라 성능 저하 가능            |
| **유효성 검사 예외 처리** | Spring의 예외 처리 메커니즘을 사용하여 처리 가능    | JPA 예외 처리 메커니즘을 사용하여 처리 가능         |

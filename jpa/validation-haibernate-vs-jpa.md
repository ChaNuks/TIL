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
    @NotNull(message = "이름은 null일 수 없습니다")
    @Size(min = 2, max = 30, message = "이름은 2자 이상 30자 이하이어야 합니다")
    private String name;

    @NotNull(message = "이메일은 null일 수 없습니다")
    @Email(message = "이메일 형식이 올바르지 않습니다")
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
### ✅ 하이버네이트 검증 (Hibernate Validator)
- `동작 시점`: Java 애플리케이션 내에서 수행 (런타임)
- `주요 어노테이션`: `@NotNull`, `@NotBlank`, `@Size`, `@Email`, `@Min` 등
- `예외 타입`: `ConstraintViolationException`, `MethodArgumentNotValidException`
- `장점`: 빠르고 상세한 메시지, DTO에도 사용 가능

### ✅ JPA 제약 조건 (DB 제약)
- `동작 시점`: DB에 INSERT 또는 UPDATE 요청이 들어갈 때
- `주요 어노테이션`: `@Column(nullable = false, length = 50)`
- `예외 타입`: `PersistenceException`, `DataIntegrityViolationException`
- `장점`: DB 무결성 보장, 실제 저장 차단

---
## 예외 비교
| 구분 | 예외 타입 | 시점 |
|------|-----------|------|
| 하이버네이트 검증 실패 | `ConstraintViolationException` | 컨트롤러/서비스 계층 |
| DB 제약 실패 (nullable 등) | `DataIntegrityViolationException`, `PersistenceException` | DB 커밋 시 |
---

## 어떤 상황에서 어떤 방식을 사용하면 좋을까?
- **Hibernate Validation**
  - 주로 클라이언트로부터 받은 입력값의 유효성을 검사할 때 사용
  - DTO나 엔티티 클래스에 적용하여, 애플리케이션 레벨에서 유효성 검사를 수행
- **JPA Validation**
  - 데이터베이스에 저장되는 객체의 유효성을 검사할 때 사용 
  - 엔티티 클래스에 적용하여, 데이터베이스 레벨에서 유효성 검사를 수행
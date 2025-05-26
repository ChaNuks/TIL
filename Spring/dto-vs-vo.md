# DTO vs VO

## 1. DTO (Data Transfer Object)

### 특징
- **순수 데이터 전송을 위한 객체** (getter, setter 중심 = POJO)
- 필드 값 변경이 가능 (mutable)
- 주로 Service 계층과 Controller 간의 데이터 전송에 사용됨
- DTO는 주로 API 요청 및 응답에 사용되며, DB와 직접적인 상호작용은 하지 않음
- DTO는 주로 데이터 전송을 위한 객체로 사용되며, 비즈니스 로직을 포함하지 않음

### 예시
```java
public class UserDTO {
    private String name;
    private String email;

    // Getter and Setter methods
    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public String getEmail() {
        return email;
    }

    public void setEmail(String email) {
        this.email = email;
    }
}
```
<br>

## 2. VO (Value Object)

### 특징
- **불변 객체** (immutable)
- "값 자체"를 의미하며, 객체의 **동등성(equals())**을 비교하는 데 사용됨
- VO는 주로 비즈니스 로직을 포함하며, 값의 변경이 불가능함
- VO는 주로 도메인 모델에서 사용되며, 비즈니스 로직을 포함할 수 있음
- VO는 주로 값의 동등성을 비교하는 데 사용되며, equals()와 hashCode() 메서드를 오버라이드하여 값 비교를 수행함

### 예시
```java
public class UserVO {
    private final String name;
    private final String email;

    public UserVO(String name, String email) {
        this.name = name;
        this.email = email;
    }

    public String getName() {
        return name;
    }

    public String getEmail() {
        return email;
    }

    @Override
    public boolean equals(Object obj) {
        if (this == obj) return true;
        if (!(obj instanceof UserVO)) return false;
        UserVO other = (UserVO) obj;
        return name.equals(other.name) && email.equals(other.email);
    }

    @Override
    public int hashCode() {
        return Objects.hash(name, email);
    }
}
```
<br>

## 요약
- DTO는 데이터 전송을 위한 객체로, mutable하며 비즈니스 로직을 포함하지 않음
- VO는 불변 객체로, 값의 동등성을 비교하는 데 사용되며 비즈니스 로직을 포함할 수 있음
- DTO는 주로 API 요청 및 응답에 사용되며, VO는 도메인 모델에서 사용됨
- DTO는 Service 계층과 Controller 간의 데이터 전송에 사용되며, VO는 도메인 모델에서 사용됨
- DTO는 주로 POJO 형태로 구현되며, VO는 불변 객체로 구현됨
- DTO는 주로 getter, setter 메서드를 포함하며, VO는 생성자와 getter 메서드를 포함함
- DTO는 주로 JSON 형태로 변환되어 전송되며, VO는 주로 객체 형태로 사용됨


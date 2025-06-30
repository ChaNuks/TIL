# JPA 복합키 매핑

## 복합키란?
- `두 개 이상의 필드`를 조합하여 하나의 키로 사용하는 것을 의미
- 복합키는 데이터베이스에서 유일성을 보장하기 위해 사용됨
- 예를 들어, 학생과 과목의 관계를 나타내는 테이블에서 학생 ID와 과목 ID를 조합하여 복합키를 만들 수 있음

---
## 복합키 매핑
- JPA에서 복합키를 매핑하는 방법은 크게 두 가지가 있음
- `@IdClass`와 `@EmbeddedId`를 사용하여 복합키를 정의할 수 있음

---

## 1. @IdClass 사용
- `@IdClass`는 복합키를 정의하기 위해 별도의 클래스를 생성하여 사용
- 복합키 클래스를 `Serializable` 인터페이스를 구현해야 함
- 복합키 클래스는 `equals()`와 `hashCode()` 메서드를 오버라이드해야 함
- 복합키 클래스는 엔티티 클래스와 동일한 패키지에 위치해야 함

### 예제
```java
import javax.persistence.*;
import java.io.Serializable;
@Entity
@IdClass(StudentCourseId.class)
public class StudentCourse {
    @Id
    private Long studentId;

    @Id
    private Long courseId;

    private String grade;

    // Getters and Setters
}


import java.io.Serializable;
import java.util.Objects;
public class StudentCourseId implements Serializable {
    private Long studentId;
    private Long courseId;

    public StudentCourseId() {}

    public StudentCourseId(Long studentId, Long courseId) {
        this.studentId = studentId;
        this.courseId = courseId;
    }

    @Override
    public boolean equals(Object o) {
        if (this == o) return true;
        if (!(o instanceof StudentCourseId)) return false;
        StudentCourseId that = (StudentCourseId) o;
        return Objects.equals(studentId, that.studentId) && Objects.equals(courseId, that.courseId);
    }

    @Override
    public int hashCode() {
        return Objects.hash(studentId, courseId);
    }
}
```
---
## 2. @EmbeddedId 사용
- `@EmbeddedId`는 복합키를 정의하기 위해 `@Embeddable` 클래스를 사용
- 복합키 클래스를 `@Embeddable`로 정의하고, 엔티티 클래스에서 `@EmbeddedId`로 사용
- 복합키 클래스는 `Serializable` 인터페이스를 구현해야 함
- 복합키 클래스는 엔티티 클래스와 동일한 패키지에 위치해야 함

### 예제
```java
import javax.persistence.*;
import java.io.Serializable;
@Embeddable
public class StudentCourseId implements Serializable {
    private Long studentId;
    private Long courseId;

    public StudentCourseId() {}

    public StudentCourseId(Long studentId, Long courseId) {
        this.studentId = studentId;
        this.courseId = courseId;
    }

    @Override
    public boolean equals(Object o) {
        if (this == o) return true;
        if (!(o instanceof StudentCourseId)) return false;
        StudentCourseId that = (StudentCourseId) o;
        return Objects.equals(studentId, that.studentId) && Objects.equals(courseId, that.courseId);
    }

    @Override
    public int hashCode() {
        return Objects.hash(studentId, courseId);
    }
}


import javax.persistence.*;
@Entity
public class StudentCourse {
    @EmbeddedId
    private StudentCourseId id;

    private String grade;

    // Getters and Setters
}
```
---
## 비교
| 특징          | @IdClass 사용                          | @EmbeddedId 사용                      |
|---------------|----------------------------------------|---------------------------------------|
| 복합키 클래스 | 별도의 클래스를 생성하여 사용           | `@Embeddable` 클래스를 사용            |
| 코드 길이     | 복합키 클래스와 엔티티 클래스가 분리됨   | 복합키 클래스가 엔티티 클래스에 포함됨  |
| equals/hashCode | 복합키 클래스에서 직접 구현해야 함     | `@Embeddable` 클래스에서 구현됨         |
| 사용 용도     | 복합키가 간단한 경우에 적합             | 복합키가 복잡한 경우에 적합             |
---
## 결론
- JPA에서 복합키를 매핑하는 방법은 `@IdClass`와 `@EmbeddedId`가 있음
- 복합키를 정의할 때는 복합키 클래스가 `Serializable` 인터페이스를 구현하고, `equals()`와 `hashCode()` 메서드를 오버라이드해야 함
- 복합키 클래스는 엔티티 클래스와 동일한 패키지에 위치해야 함
- 복합키를 매핑할 때는 상황에 따라 `@IdClass` 또는 `@EmbeddedId`를 선택하여 사용하면 됨
- 복합키를 사용하면 데이터베이스에서 유일성을 보장할 수 있으며, 복잡한 관계를 표현할 수 있음
# 검증


## 검증 종류
- **데이터 검증**
- **비즈니스 검증** 


## 데이터 검증이란?
- 데이터의 형식, 범위, 존재 여부 등을 확인하는 과정
- 예: 이메일 형식, 숫자 범위, 필수 입력 여부 등
- 주로 프론트엔드에서 처리되지만, 백엔드에서도 검증 필요
- 예시:
  - 이메일 형식: `@`가 포함되어야 함
  - 숫자 범위: 1부터 100 사이의 값이어야 함
  - 필수 입력 여부: 빈 값이 아니어야 함


## 비즈니스 검증이란?
- 비즈니스 로직에 따라 데이터의 유효성을 검증하는 과정
- 예: 특정 조건을 만족해야 하는 경우, 데이터 간의 관계를 검증하는 경우 등
- 주로 백엔드에서 처리되며, 프론트엔드에서도 일부 검증 가능
- 예시:
  - 주문 금액이 최소 금액 이상이어야 함
  - 특정 날짜 이후에만 예약 가능
  - 사용자 권한에 따라 접근 가능한 데이터 제한


## Bean Validation
- Java Bean Validation을 위한 표준 API
- 주로 Hibernate Validator를 구현체로 사용
- DTO 클래스에 어노테이션을 사용하여 검증 규칙 정의
- 예시 코드 :
```java
import javax.validation.constraints.*;
public class UserDTO {
    @NotNull(message = "이름은 필수 입력입니다.")
    private String name;

    @Email(message = "유효한 이메일 주소를 입력해주세요.")
    private String email;

    @Min(value = 18, message = "최소 18세 이상이어야 합니다.")
    @Max(value = 100, message = "최대 100세 이하이어야 합니다.")
    private int age;
    
}
```


## Spring Validation
- Spring Framework에서 Bean Validation을 통합하여 사용
- `@Validated` 어노테이션을 사용하여 검증 활성화
- `BindingResult`를 통해 검증 결과를 처리
- 검증 실패 시, `MethodArgumentNotValidException` 예외 발생
- 예외 처리 핸들러를 통해 사용자에게 적절한 오류 메시지 전달
- `@ResponseStatus(HttpStatus.BAD_REQUEST)`를 사용하여 400 Bad Request 응답 반환
- 예시 코드 :
```java
import org.springframework.validation.annotation.Validated;
import org.springframework.web.bind.annotation.*;
@RestController
@RequestMapping("/users")
public class UserController {

    @PostMapping
    public ResponseEntity<String> createUser(@Validated @RequestBody UserDTO userDTO, BindingResult bindingResult) {
        if (bindingResult.hasErrors()) {
            throw new MethodArgumentNotValidException(null, bindingResult);
        }
        // 사용자 생성 로직
        return ResponseEntity.ok("사용자 생성 성공");
    }
}
```

## 검증 어노테이션 
| 어노테이션          | 설명                                      | 예시 사용법                          |
|---------------------|-------------------------------------------|--------------------------------------|
| `@NotNull`          | 값이 null이 아니어야 함                    | `@NotNull private String name;`      |
| `@NotBlank`         | 빈 문자열이 아니어야 함 (공백 제외)         | `@NotBlank private String description;` |
| `@NotEmpty`         | 빈 컬렉션이나 배열이 아니어야 함            | `@NotEmpty private List<String> items;` |
| `@Size`             | 문자열, 배열, 컬렉션의 크기를 제한         | `@Size(min = 1, max = 100) private String title;` |
| `@Min`              | 숫자의 최소값을 제한                       | `@Min(1) private int age;`           |
| `@Max`              | 숫자의 최대값을 제한                       | `@Max(100) private int score;`       |
| `@Email`            | 유효한 이메일 형식이어야 함                 | `@Email private String email;`       |
| `@Pattern`          | 정규 표현식에 맞는 문자열이어야 함         | `@Pattern(regexp = "^[a-zA-Z0-9]+$") private String username;` |
| `@Future`           | 미래 날짜여야 함                           | `@Future private LocalDate eventDate;` |
| `@Past`             | 과거 날짜여야 함                           | `@Past private LocalDate birthDate;` |


## @NotNull vs @NotBlank vs @NotEmpty 차이점
- `@NotNull`
  - 값이 null이 아니어야 함
  - null 체크만 수행, 빈 문자열은 허용
  - 사용 예: 객체의 필수 속성 검증
  ```java
    @NotNull(message = "이름은 필수 입력입니다.")
    private String name;
    ```


- `@NotBlank`
  - 값이 null이 아니고, 빈 문자열이 아니어야 함 (공백 제외)
  - 문자열의 공백을 허용하지 않음
  - 사용 예: 사용자 입력 필드 검증
  ```java
    @NotBlank(message = "설명은 필수 입력입니다.")
    private String description;
    ```
  

- `@NotEmpty`
  - 빈 컬렉션이나 배열이 아니어야 함
  - null 체크와 빈 컬렉션/배열 체크를 모두 수행
  - 사용 예: 리스트, 배열 등의 필수 입력 검증
  ```java
    @NotEmpty(message = "아이템 목록은 필수 입력입니다.")
    private List<String> items;
  ```
  
- 정리
  - `@NotNull`은 null 체크만 수행, `@NotBlank`는 빈 문자열을 제외한 문자열 검증, `@NotEmpty`는 빈 컬렉션이나 배열 검증을 수행함


## Hibernate Validator란?
- Java Bean Validation의 구현체로, Spring Framework와 통합되어 사용됨
- 검증 결과를 로깅하거나, 다른 서비스와 통합하여 검증 결과를 처리할 수 있음
- 검증 결과를 JSON 형식으로 반환할 수 있는 기능 제공
- 검증 결과를 국제화(i18n)하여 다국어 지원 가능
- 검증 결과를 캐싱하여 성능 향상 가능
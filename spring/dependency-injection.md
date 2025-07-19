# Dependency Injection

## 의존성 주입(Dependency Injection
- 객체 간의 의존 관계를 외부에서 주입하는 방식
- 객체의 생성과 의존 관계를 분리하여 유연성과 테스트 용이성을 높임
- Spring에서는 `@Autowired`, `@Inject`, `@Resource` 등을 사용하여 의존성 주입을 수행

---

## 의존성 주입 방식
`1. 생성자 주입(Constructor Injection)` - 권장
  - 생성자를 통해 의존성을 주입
  - 장점: 불변성 유지, 필수 의존성 명확화
  - 단점: 생성자가 복잡해질 수 있음
  - ex)
  ```java
  @Service
  public class UserService {

      private final UserRepository userRepository;

      // 생성자 주입
      @Autowired
      public UserService(UserRepository userRepository) {
          this.userRepository = userRepository;
      }

      public void doSomething() {
          userRepository.save();
      }
  }
  ```
- 생성자 주입은 의존성이 변경되지 않는 경우에 가장 적합하며, 객체가 생성될 때 모든 의존성을 명시적으로 주입할 수 있어 테스트와 유지보수가 용이함
            

`2.세터 주입(Setter Injection)`
  - 세터 메서드를 통해 의존성을 주입
  - 장점: 선택적 의존성 주입 가능, 객체 생성 후 의존성 변경 가능
  - 단점: 불변성 유지 어려움, 필수 의존성 명확화 어려움
  - ex)
  ```java
  @Service
  public class UserService {

      private UserRepository userRepository;

      // 세터 주입
      @Autowired
      public void setUserRepository(UserRepository userRepository) {
          this.userRepository = userRepository;
      }

      public void doSomething() {
          userRepository.save();
      }
  } 
  ```
- 세터 주입은 객체 생성 후 의존성을 변경할 수 있어 유연성이 필요할 때 유용하지만, 필수 의존성을 명확히 하기 어려워 테스트가 복잡해질 수 있음



`3. 필드 주입(Field Injection)` - 권장하지 않음
  - 필드에 직접 의존성을 주입
  - 장점: 코드가 간결해짐, 의존성 주입이 명시적이지 않음
  - 단점: 테스트 어려움, 불변성 유지 어려움, 순환 참조 문제 발생 가능
  - ex)
  ```java
  @Service
  public class UserService {

      @Autowired
      private UserRepository userRepository; // 필드 주입

      public void doSomething() {
          userRepository.save();
      }
  }
  ```
- 필드 주입은 코드가 간결해지는 장점이 있지만, 테스트가 어려워지고 의존성이 명시적이지 않아 유지보수가 어려워질 수 있어 권장하지 않음
---

## 의존성 주입 흐름
1. **객체 생성**: Spring 컨테이너가 객체를 생성
2. **의존성 주입**: 생성자, 세터, 필드 등을 통해 의존성을 주입
3. **초기화**: `@PostConstruct` 어노테이션이 있는 메서드가 호출되어 초기화 작업 수행
4. **사용**: 의존성이 주입된 객체를 사용
5. **소멸**: `@PreDestroy` 어노테이션이 있는 메서드가 호출되어 소멸 작업 수행

```java
@Component
public class UserService {

    private final UserRepository userRepository;

    @Autowired
    public UserService(UserRepository userRepository) {
        this.userRepository = userRepository;
    }

    @PostConstruct
    public void init() {
        // 초기화 작업
        System.out.println("UserService initialized with UserRepository");
    }

    @PreDestroy
    public void cleanup() {
        // 소멸 작업
        System.out.println("UserService is being destroyed");
    }
}
```
---

## 주의점
- **순환 참조(Circular Dependency)**
  - A가 B를 의존하고 B가 A를 의존하는 경우, 생성자 주입을 사용하면 순환 참조 문제가 발생할 수 있음. 이 경우 세터 주입이나 필드 주입을 고려해야 함

- **@Autowired(required = false)**
  - 의존성이 선택적일 때 사용. 해당 의존성이 없더라도 애플리케이션이 정상적으로 동작하도록 함
  - 예시: `@Autowired(required = false) private Optional<UserRepository> userRepository;`

- **@Inject**와 **@Resource**
  - `@Inject`: Java 표준 의존성 주입 어노테이션, `@Autowired`와 유사하지만, 생성자 주입에만 사용 가능
  - `@Resource`: JSR-250 표준 어노테이션, 이름 기반 주입을 지원하며, 필드나 세터 메서드에 사용 가능

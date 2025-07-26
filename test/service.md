# Service Test

## 1. 서비스 테스트란?

- 소프트웨어의 특정 기능이나 모듈이 예상대로 작동하는지 확인하기 위해 작성된 테스트
- 주로 비즈니스 로직, 데이터 처리, 외부 API 호출 등을 검증

## 2. 예제 코드

- 회원가입 테스트

```java
public class MemberDummy {

    public static Member create() {
        return new Member("dummy@example.com", "홍길동");
    }

    public static Member withEmail(String email) {
        return new Member(email, "홍길동");
    }

    public static Member withName(String name) {
        return new Member("dummy@example.com", name);
    }
}
```

```java
@ExtendWith(MockitoExtension.class)
class MemberServiceTest {

    @Mock
    private MemberRepository memberRepository;

    @InjectMocks
    private MemberService memberService;

    @Test
    @DisplayName("회원가입 테스트")
    void register() {
        // Given
        Member dummy = MemberDummy.create();
        given(memberRepository.existsByEmail(dummy.getEmail())).willReturn(false);
        given(memberRepository.save(any(Member.class))).willReturn(dummy);

        // When
        Member result = memberService.join(dummy);

        // Then
        assertThat(result.getEmail()).isEqualTo(dummy.getEmail());
        assertThat(result.getName()).isEqualTo(dummy.getName());
    }
}
```

## 3. 테스트 방법

- `Mockito`: Mock 객체를 생성하여 의존성을 주입하고, 메서드 호출을 검증
- `@Mock`: Mock 객체를 생성
- `@InjectMocks`: Mock 객체를 주입받는 클래스의 인스턴스를
- `@Test`: 테스트 메서드를 정의
- `@DisplayName`: 테스트의 설명을 지정
- `given()`: Mock 객체의 동작을 정의
- `when()`: 실제 메서드 호출을 수행
- `assertThat()`: 결과를 검증
- `any()`: 어떤 인자든 허용하는 Matcher
- `willReturn()`: Mock 객체가 특정 값을 반환하도록 설정
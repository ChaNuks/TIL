# 🧪 Spring 계층별 테스트 정리

Spring에서의 테스트는 계층(Controller, Service, Repository)에 따라 사용하는 어노테이션, 목적, 전략이 다름

---

## ✅ 전체 요약 표

| 계층 | 어노테이션 | 테스트 전략 | 설명 |
|------|------------|-------------|------|
| Controller | `@WebMvcTest` | MockMvc 사용, Service Mocking | 웹 계층만 테스트 |
| Service | `@SpringBootTest` + `@MockBean` or 단위 테스트 | 의존성 Mock 처리 | 비즈니스 로직 중심 테스트 |
| Repository | `@DataJpaTest` | 실제 DB or H2 사용 | JPA 쿼리 및 영속성 테스트 |

---
## 🔹 Controller 테스트

### 목적
- 요청/응답(JSON, 상태 코드) 처리 확인
- 입력값 검증, URI 매핑, 예외 처리 테스트

### 어노테이션 조합

```java
@WebMvcTest(YourController.class)
@AutoConfigureMockMvc
class YourControllerTest {
    @Autowired MockMvc mockMvc;

    @MockBean YourService yourService; // 내부 의존성 목킹
}
```

### 특징
- Controller Layer만 로드
- 단위 테스트에 가까움

---
## 🔹 Service 테스트 - Mockito 사용

### 목적
- 핵심 로직 검증 (조건, 분기, 계산 등)
- 이 때 외부 의존성(Repository, 외부 API 등)은 실제 객체 대신 'Mock' 객체를 사용

### 어노테이션 조합

```java
@ExtendWith(MockitoExtension.class)  // Junit5에서 Mockito 활성화
@SpringBootTest
class ProductServiceTest {

    @Mock // 가짜 객체 생성
    ProductRepository productRepository;

    @InjectMocks  // Mock들을 주입해서 테스트 대상 객체 생성
    ProductService productService;

    @Test
    void getProduct_정상조회() {
        // given
        Product product = new Product(1L, "망고");
        given(productRepository.findById(1L)).willReturn(Optional.of(product));

        // when
        Product result = productService.getProduct(1L);

        // then
        assertEquals("망고", result.getName());
        verify(productRepository).findById(1L);
    }
}
```

---
## 🔹 Repository 테스트

### 목적
- JPA 쿼리 기능, 영속성 검증
- Entity 저장/조회/삭제 동작 확인

### 어노테이션 조합

```java
@DataJpaTest
class ProductRepositoryTest {

    @Autowired
    ProductRepository productRepository;

    @Test
    void saveProduct() {
        // given
        Product product = new Product(1L, "망고");

        // when
        productRepository.save(product);

        // then
        Product savedProduct = productRepository.findById(1L).get();
        assertEquals("망고", savedProduct.getName());
    }
}
```
---

## 🔹 계층별 테스트 전략
| 계층 | 어노테이션 | 도구 | 설명 |
|------|------------|------|------|
| Controller | `@WebMvcTest` | `MockMvc` | 웹 계층만 로딩, 서비스는 Mock 처리 |
| Service | `@SpringBootTest`, `@MockBean` / 순수 Mockito | `Mockito`, `JUnit` | 비즈니스 로직 테스트, 외부 의존성 목 처리 |
| Repository | `@DataJpaTest` | Spring Data JPA, 내장 DB(H2) | 실제 쿼리 실행, 영속성 테스트 |

---

## 🔹 테스트 도구 정리

| 도구/어노테이션 | 종류 | 대상 계층 | 특징 및 용도 |
|------------------|--------|--------------|----------------|
| `@WebMvcTest` | 슬라이스 테스트 | Controller | 웹 계층만 로딩, 빠르고 가벼움 |
| `@MockMvc` | 테스트 도구 | Controller | HTTP 요청/응답 테스트 도구 |
| `@SpringBootTest` | 통합 테스트 | 전체 | 전체 컨텍스트 로딩, 느리지만 실제 환경과 유사 |
| `@DataJpaTest` | 슬라이스 테스트 | Repository | JPA 관련 Bean만 로딩, H2 DB 자동 설정 |
| `@MockBean` | Spring Mock | Service 등 | Spring context 안에서 의존 Bean을 Mock 처리 |
| `@Mock` | Mockito | Service | 순수 자바 환경에서 Mock 객체 생성 |
| `@InjectMocks` | Mockito | Service | Mock을 주입한 실제 테스트 대상 객체 생성 |
| `@ExtendWith(MockitoExtension.class)` | Mockito 설정 | Service | JUnit5에서 Mockito 사용시 필수 |
| `@Transactional` | DB 테스트 보조 | Repository | 테스트 후 자동 롤백 처리 |
| `TestRestTemplate` | HTTP 통합 테스트 | Controller | 실제 HTTP 요청 시 사용 (SpringBootTest에서만 가능) |
| `@BeforeEach`, `@AfterEach` | JUnit | 공통 | 각 테스트 전/후 공통 로직 작성 시 사용 |

---

## 📌 테스트 유형별 비교

| 항목 | 단위 테스트 (Unit) | 슬라이스 테스트 | 통합 테스트 |
|------|---------------------|------------------|----------------|
| 속도 | 매우 빠름 | 빠름 | 느림 |
| Spring Context | ❌ | ✅ 일부만 | ✅ 전체 |
| 외부 의존성 | Mock | Mock or 실제 | 실제 |
| 대표 어노테이션 | `@Mock`, `@InjectMocks` | `@WebMvcTest`, `@DataJpaTest` | `@SpringBootTest` |
| 테스트 대상 | 개별 메서드, 로직 | 계층별 일부 Bean | 전체 흐름 |

---

## 🧪 예시 조합

| 목적 | 어노테이션 조합 | 비고 |
|------|------------------|------|
| 컨트롤러 테스트 | `@WebMvcTest` + `MockMvc` + `@MockBean` | REST API 응답 확인 |
| 서비스 단위 테스트 | `@Mock`, `@InjectMocks`, `MockitoExtension` | 외부 의존성 목킹 |
| 서비스 통합 테스트 | `@SpringBootTest` + `@MockBean` | context 포함, 느리지만 정확 |
| JPA 기능 테스트 | `@DataJpaTest` + H2 | 영속성, 쿼리 정확도 확인 |

---

## 🔍 기타 참고

- ❗ `@SpringBootTest`는 무겁기 때문에 자주 사용하지 말고 필요한 경우에만
- ✅ 슬라이스 테스트는 빠르고 정확한 계층 테스트에 유용
- 🔥 `Mockito`는 테스트를 빠르게 하면서도 의존성을 줄일 수 있는 강력한 도구

---
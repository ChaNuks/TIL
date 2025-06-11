# API 문서화 : OAS vs Spring REST Docs

## 1. OAS(OpenAPI Specification)

### 정의
- REST API를 명세하는 **표준 형식(JSON/YAML)**
- Swagger로 알려졌지만, 현재는 OpenAPI가 공식 명칭
- 문서, 테스트, Mock 서버 자동화에 활용 가능

### 사용 도구
- `springdoc-openapi` (Spring Boot와 통합)
- `Swagger UI`, `Redoc` (시각화)
- `Postman`, `Stoplight` 등도 OAS 지원

### 예시
```yaml
openapi: 3.0.1
info:
    title: Todo API
    version: 1.0.0
servers:
    - url: http://localhost:8080
paths:
    /todos:
        get:
            summary: Retrieve a list of todos
            responses:
                '200':
                    description: A list of todos
                    content:
                        application/json:
                            schema:
                                $ref: '#/components/schemas/Todo'
components:
    schemas:
        Todo:
            type: object
            properties:
                id:
                    type: integer
                    format: int64
                title:
                    type: string
                description:
                    type: string
                completed:
                    type: boolean
```
---
## 2. Spring REST Docs

### 정의
- 실제 테스트 코드 기반으로 API 문서 생성
- MockMvs / WebTestClient / RestAssured / JUnit5 활용
- Spring REST Docs를 다언한 테스트 프레임워크와 통합가능
- AsciiDoc, Markdown, HTML, PDF, JSON, YAML, CSV, Excel 등의 문서 자동화가 가능

### 예시
```java
// Controller 예시
@RestController
@RequestMapping("/api")
public class HelloController {

    @GetMapping("/hello")
    public Map<String, String> hello(@RequestParam String name) {
        return Map.of("message", "Hello, " + name);
    }
}

// 테스트 코드 예시
@WebMvcTest(HelloController.class)
@AutoConfigureRestDocs(outputDir = "build/generated-snippets")
class HelloControllerTest {

    @Autowired
    private MockMvc mockMvc;

    @Test
    void hello_api_문서화() throws Exception {
        mockMvc.perform(get("/api/hello")
                        .param("name", "ChatGPT"))
                .andExpect(status().isOk())
                .andDo(document("hello-api",
                        requestParameters(
                                parameterWithName("name").description("사용자 이름")
                        ),
                        responseFields(
                                fieldWithPath("message").description("환영 메시지")
                        )
                ));
    }
}
```
---
## 3. OAS vs Spring REST Docs 비교

| 항목 | Spring REST Docs | OpenAPI (Swagger) |
|------|------------------|--------------------|
| 📌 문서 생성 방식 | 테스트 기반 (실제 API 호출 기반) | 애노테이션 기반 (정적 분석) |
| 🔧 설정 방식 | `MockMvc` 또는 `WebTestClient` + AsciiDoc | `@Operation`, `@Schema` 등 Swagger 어노테이션 |
| 🧪 문서 정확성 | 실제 테스트 기반 → 높은 정확도 | 코드와 불일치 가능성 있음 |
| 🎨 UI 제공 | 기본 없음 (AsciiDoctor HTML 가능) | Swagger UI, Redoc 등 자동 문서 UI |
| 💡 자동화 | 수동 작성 필요 (스니펫 기반) | 문서 자동 생성 |
| 🔁 클라이언트 SDK 생성 | ❌ 불가 | ✅ 자동 생성 가능 (Swagger Codegen 등) |
| 💬 협업 효율 | 기술 기반, 문서 포맷 자유로움 | 비개발자와 협업에 유리한 시각화 UI |
| 🧩 커스터마이징 | 문서 구조, 텍스트 유연하게 설정 가능 | 제한적 (어노테이션 수준) |
| 📁 빌드 도구 통합 | Gradle Asciidoctor 플러그인 | springdoc-openapi, springfox 등 |
| 🧠 러닝 커브 | 문서 포맷 학습 필요 (AsciiDoc 등) | UI 기반 접근 쉬움 |
| 🧰 대표 라이브러리 | `spring-restdocs-mockmvc`, `asciidoctor` | `springdoc-openapi`, `swagger-ui` |
---

## 📎 선택 기준 요약

| 상황 | 추천 도구 |
|------|-----------|
| API 테스트 기반 문서 자동화 | ✅ Spring REST Docs |
| 프론트/외부 협업용 빠른 시각적 문서 | ✅ Swagger (OpenAPI) |
| 실제 스펙과 100% 일치하는 문서가 필요할 때 | ✅ Spring REST Docs |
| 클라이언트 코드 자동 생성이 필요한 경우 | ✅ OpenAPI (Swagger Codegen) |
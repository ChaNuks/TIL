# 테스트 코드 작성

## 테스트 코드란?
- 기능이 잘 작동하는지 확인하기 위해 작성하는 코드

---

## 왜 테스트 코드를 작성해야 할까?
| 이유 | 설명 |
| --- | --- |
| 버그 예방 | 코드 변경 시 기존 기능이 깨지는 것을 방지 |
| 코드 품질 향상 | 코드의 가독성과 유지보수성을 높임 |
| 문서화 | 코드의 사용법과 동작을 문서화하는 역할 |
| 자동화 | 반복적인 테스트를 자동으로 수행 가능 |
| 신뢰성 향상 | 코드 변경 후에도 기능이 정상 작동하는지 확인 가능 |

---

## 테스트 코드 작성 방법
### 1. 테스트 프레임워크 선택
- JUnit, TestNG 등 다양한 테스트 프레임워크가 있음

### 2. 테스트 케이스 작성
- 각 기능에 대해 테스트 케이스를 작성
- 테스트 메서드는 `@Test` 어노테이션을 사용하여 정의


### 3. 어서션 사용
- 테스트 결과를 검증하기 위해 어서션 사용
- 예: `assertEquals(expected, actual)`, `assertTrue(condition)`


### 4. 테스트 실행
- IDE나 빌드 도구(예: Maven, Gradle)를 사용하여 테스트 실행
- 테스트 결과를 확인하고, 실패한 테스트를 수정


### 5. 테스트 코드 유지보수
- 코드 변경 시 테스트 코드도 함께 수정
- 테스트 케이스를 주기적으로 검토하고, 필요에 따라 추가하거나 수정

---

## 기본 구조 (Given-When-Then 패턴)
```java
@Test
public void testExample() {
    // Given: 테스트 준비
    int expected = 5;
    int input = 2;

    // When: 테스트 실행
    int actual = someMethod(input);

    // Then: 결과 검증
    assertEquals(expected, actual);
}
```

## 계층별 테스트 코드 구조(controller, service, repository)
### 1. Controller 테스트
### 2. Service 테스트
### 3. Repository 테스트

- 추후 작성


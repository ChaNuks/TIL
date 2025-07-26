# Multi Module

## 멀티 모듈(Multi Module)이란?
- 하나의 프로젝트를 여러 개의 모듈로 나누어 관리하는 것을 의미
- 각 모듈은 독립적으로 개발, 빌드, 테스트, 배포가 가능
- 계층별 책임을 명확히 분리하고, 재사용성과 관리 효율성을 높임

---

## 멀티 모듈 구조 예시
```
spring-multi-module
├── api
│   └── src
│       └── main
│           └── java
│               └── com
│                   └── example
│                       └── api
├── service
│   └── src
│       └── main
│           └── java
│               └── com
│                   └── example
│                       └── service
├── repository
│   └── src
│       └── main
│           └── java
│               └── com
│                   └── example
│                       └── repository
└── common
    └── src
        └── main
            └── java
                └── com
                    └── example
                        └── common
```
- `api`: API 레이어, 클라이언트와의 인터페이스를 담당
- `service`: 비즈니스 로직을 처리하는 서비스 레이어
- `repository`: 데이터베이스와의 상호작용을 담당하는 레이어
- `common`: 공통적으로 사용되는 유틸리티 클래스나 상수 등을 포함하는 모듈
- 각 모듈은 독립적으로 개발 및 테스트가 가능하며, 필요에 따라 다른 모듈을 의존할 수 있음
- 모듈 간 의존성은 `pom.xml` 또는 `build.gradle` 파일에서 관리

---

## 그럼 어떻게 세팅하는데?
### 1. Gradle 설정
- `settings.gradle` 파일에 모듈을 추가
```groovy
rootProject.name = 'spring-multi-module'
include 'api', 'service', 'repository', 'common'
```

### 2. 각 모듈의 `build.gradle` 파일 설정
- `api/build.gradle`
```groovy
dependencies {
    implementation project(':service')
    implementation project(':repository')
    implementation project(':common')
    // 추가적인 의존성
}
```

- `service/build.gradle`
```groovy
dependencies {
    implementation project(':repository')
    implementation project(':common')
    // 추가적인 의존성
}
```

- `repository/build.gradle`
```groovy
dependencies {
    implementation project(':common')
    // 추가적인 의존성
}
```

- `common/build.gradle`
```groovy
dependencies {
    // 공통적으로 필요한 의존성
}
```

---

## 3. 모듈 간 의존성 관리
- 각 모듈은 필요한 다른 모듈을 의존성으로 추가하여 사용할
- 예를 들어, `api` 모듈은 `service`, `repository`, `common` 모듈을 의존하여 API 요청을 처리하고, 비즈니스 로직을 호출하며, 데이터베이스와 상호작용함
- 모듈 간 의존성은 `implementation` 또는 `api` 키워드를 사용하여 설정
- `implementation`은 해당 모듈의 API를 외부에 노출하지 않고, `api`는 외부에 노출함
- 의존성 관리 시, 모듈 간의 순환 의존성은 피해야 함
- 순환 의존성이 발생하면 빌드 오류가 발생하므로, 모듈 간의 책임을 명확히 분리하고, 필요한 경우 인터페이스를 사용하여 의존성을 관리함


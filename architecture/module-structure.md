# 모듈은 어떻게 나누면 좋을까?

## 모듈이란?
- 관련된 기능을 묶어놓은 단위
- 모듈은 독립적으로 개발, 테스트, 배포가 가능
- 모듈은 다른 모듈과 의존성을 가질 수 있음
- 멀티 모듈 프로젝트는 모듈 간의 의존성을 관리하는 것이 중요

---

## 모듈을 나누는 기준
| 기준          | 설명                                                         |
|---------------|--------------------------------------------------------------|
| 기능          | 관련된 기능을 묶어서 모듈로 나눈다.                          |
| 도메인        | 도메인별로 모듈을 나눈다. 예: 사용자, 상품, 주문 등          |
| 레이어        | 레이어별로 모듈을 나눈다. 예: 프레젠테이션, 서비스, 데이터 접근 |
| 기술 스택     | 기술 스택별로 모듈을 나눈다. 예: Spring, JPA, MyBatis 등     |
| 팀            | 팀별로 모듈을 나눈다. 팀이 담당하는 기능을 모듈로 묶는다.      |
| 재사용성      | 재사용 가능한 기능을 모듈로 나눈다. 예: 공통 라이브러리, 유틸리티 등 |
| 테스트 용이성 | 테스트가 용이하도록 모듈을 나눈다. 예: 테스트 전용 모듈, Mock 모듈 등 |
| 배포 용이성   | 배포가 용이하도록 모듈을 나눈다. 예: 독립적으로 배포 가능한 모듈 |

---

# 모듈 구조 및 역할
## 1. 모듈 구성
- `v3/`: v3 버전의 모듈 디렉토리
- `bmarketapi/`: Bmarket API 모듈
  - Controller, Service, Repository, DTO 등 API 관련 코드 포함
- `storage/`: 스토리지 모듈
  - Entity, Repository, Base Entity, JPA Auditing 등 스토리지 관련 코드 포함


## 2. 모듈 역할
### bmarketapi 모듈
- **Controller**: HTTP 요청을 처리하고 응답을 반환하는 역할
- **Service**: 비즈니스 로직을 처리하는 역할
- **Repository**: 데이터베이스와의 상호작용을 담당하는 역할 (storage 모듈의 Repository를 상속 받아 사용)
- **DTO**: 데이터 전송 객체로, API 요청 및 응답에 사용되는 데이터 구조
- **CommandService**: 도메인 명령(Command) 패턴을 사용하여 비즈니스 로직을 처리하는 역할 (선택 사항)

### storage 모듈
- **Entity**: 데이터베이스 테이블과 매핑되는 클래스
- **Repository**: Entity와 데이터베이스 간의 CRUD 작업을 수행하는 인터페이스
- **Base Entity**: 공통 필드(예: 생성일, 수정일 등)를 포함하는 기본 엔티티 클래스
- **JPA Auditing**: 엔티티의 생성 및 수정 시간을 자동으로 관리하는 기능
- **Configuration**: JPA 및 데이터베이스 관련 설정을 포함하는 클래스

---

## 3. 모듈 간 의존성
### 1. `bmarketapi` 모듈은 `storage` 모듈에 의존
  - Repository는 `storage` 모듈의 Repository를 상속받아 사용
  - Service는 `storage` 모듈의 Entity를 사용하여 비즈니스 로직을 처리
  - Controller는 `bmarketapi` 모듈의 Service를 호출
  - DTO는 `storage` 모듈의 Entity를 기반으로 생성되어 API 요청 및 응답에 사용됨
  

### 2. `storage` 모듈은 `bmarketapi` 모듈에 의존하지 않음
  - 독립적으로 데이터베이스와의 상호작용을 처리
  - Entity와 Repository는 `bmarketapi` 모듈에서 사용되지만, `storage` 모듈 자체는 `bmarketapi` 모듈에 의존하지 않음
  - JPA 및 데이터베이스 관련 설정을 포함하여, 다른 모듈에서도 재사용 가능하도록 설계됨
  - JPA Auditing 기능을 통해 엔티티의 생성 및 수정 시간을 자동으로 관리함으로써, 데이터베이스와의 상호작용을 효율적으로 처리함
  - Base Entity는 공통 필드를 포함하여, 코드 중복을 줄이고 유지보수를 용이하게 함
  - Repository는 JPA를 사용하여 데이터베이스와의 CRUD 작업을 수행하며, `bmarketapi` 모듈에서 상속받아 사용됨
  - Entity는 데이터베이스 테이블과 매핑되어, 데이터베이스와의 상호작용을 효율적으로 처리함

---

## 4. 모듈 구조 예시 (도메인 기반 디렉토리 구조)
```
bmarketapi/
└── user/
    ├── UserController.java
    ├── UserService.java
    ├── UserDto.java
    ├── UserCommandService.java (선택)
    └── UserRepository.java (storage 모듈의 Repository 상속)

storage/
└── user/
    ├── UserEntity.java
    ├── UserRepository.java
    ├── BaseEntity.java
    ├── JPAConfig.java
    └── JPAAuditingConfig.java
```

### 1. `bmarketapi/user/` : 사용자 관련 API 코드
  - `UserController.java`: 사용자 관련 HTTP 요청을 처리하는 컨트롤러
  - `UserService.java`: 사용자 관련 비즈니스 로직을 처리하는 서비스
  - `UserDto.java`: 사용자 관련 데이터 전송 객체
  - `UserCommandService.java`: 사용자 관련 도메인 명령(Command) 패턴을 사용하여 비즈니스 로직을 처리하는 서비스 (선택 사항)
  - `UserRepository.java`: 사용자 관련 데이터베이스 작업을 수행하는 리포지토리 (storage 모듈의 Repository를 상속)

### 2. `storage/user/` : 사용자 관련 스토리지 코드
  - `UserEntity.java`: 사용자 관련 데이터베이스 테이블과 매핑되는 엔티 클래스
  - `UserRepository.java`: 사용자 관련 데이터베이스 CRUD 작업을 수행하는 리포지토리
  - `BaseEntity.java`: 공통 필드를 포함하는 기본 엔티티 클래스
  - `JPAConfig.java`: JPA 관련 설정을 포함하는 클래스
  - `JPAAuditingConfig.java`: JPA Auditing 기능을 설정하는 클래스

### 3. 공통 모듈
- `common/`: 공통 모듈
  - 공통 유틸리티 클래스, 상수, 예외 처리 등을 포함
  - 다른 모듈에서 재사용 가능한 기능을 제공
  - 예시:
  ```
  common/
  ├── util/
  │   ├── DateUtil.java
  │   ├── StringUtil.java
  ├── exception/
  │   ├── CustomException.java
  │   ├── ErrorCode.java
  └── config/
      ├── AppConfig.java
      ├── SecurityConfig.java
  ```
  
### 4. 테스트 모듈
- `test/`: 테스트 모듈
  - 단위 테스트, 통합 테스트 등을 포함
  - 각 모듈의 테스트 코드를 포함하여, 모듈 간의 의존성을 관리
  - 예시:
  ```
  test/
  ├── user/
  │   ├── UserControllerTest.java
  │   ├── UserServiceTest.java
  ├── storage/
  │   ├── UserRepositoryTest.java
  └── common/
      ├── DateUtilTest.java
      ├── StringUtilTest.java
  ```
---

## 좋은 모듈 분리하려면 어떻게 해아하나?
- `기능 중심` 
  - 관련된 기능을 묶어서 모듈로 나누자
  - 예를 들어, 사용자 관리, 상품 관리, 주문 관리 등 도메인별로 모듈을 나누자


- `독립성 유지`
  - 모듈은 독립적으로 개발, 테스트, 배포가 가능해야 함
  - 모듈 간의 의존성을 최소화하여, 모듈이 독립적으로 동작할 수 있도록 하자
  - 예를 들어, `bmarketapi` 모듈과 `storage` 모듈을 분리하여, 각각 독립적으로 개발하고 배포할 수 있도록 하자


- `재사용성 고려`
  - 재사용 가능한 기능을 모듈로 나누자. 
  - 예를 들어, 공통 라이브러리, 유틸리티 클래스 등을 별도의 모듈로 나누어 다른 모듈에서도 재사용할 수 있도록 하자
  - `common` 모듈을 만들어 공통 유틸리티 클래스, 상수, 예외 처리 등을 포함


- `테스트 용이성`
  - 테스트가 용이하도록 모듈을 나누자. 
  - 예를 들어, 단위 테스트, 통합 테스트 등을 별도의 모듈로 나누어 테스트를 용이하게 하자
  - `test` 모듈을 만들어 각 모듈의 테스트 코드를 포함


- `배포 용이성`
  - 배포가 용이하도록 모듈을 나누자. 
  - 예를 들어, 독립적으로 배포 가능한 모듈을 만들어 배포를 용이하게 하자
  - `bmarketapi` 모듈과 `storage` 모듈을 분리하여, 각각 독립적으로 배포할 수 있도록 하자
# RESTful API란?

## 1. 정의
- REST(Representational State Transfer) 아키텍처 스타일을 따르는 웹 서비스
- RESTful API는 HTTP 프로토콜을 사용하여 클라이언트와 서버 간의 통신을 수행함
- RESTful API는 자원(리소스)을 URI로 식별하고, HTTP 메서드(GET, POST, PUT, DELETE 등)를 사용하여 자원에 대한 CRUD(Create, Read, Update, Delete) 작업을 수행함


## 2. 특징
- **Stateless**
  - 클라이언트와 서버 간의 상태를 유지하지 않음
  - 각 요청은 독립적이며, 서버는 클라이언트의 상태를 저장하지 않음
  - 클라이언트는 필요한 모든 정보를 요청에 포함해야 함
  

- **Cacheable** 
  - 응답은 캐시 가능
  - 클라이언트는 서버의 응답을 캐시하여 성능을 향상시킬 수 있음
  - 서버는 응답에 캐시 관련 정보를 포함할 수 있음


- **Layered System** 
  - 클라이언트는 서버와 직접 통신하지 않고, 중간 계층을 통해 통신할 수 있음
  - 중간 계층은 로드 밸런서, 캐시 서버 등 다양한 역할을 수행할 수 있음


- **Uniform Interface** 
  - 클라이언트와 서버 간의 인터페이스가 일관됨
  - 자원은 URI로 식별되며, HTTP 메서드를 사용하여 자원에 대한 작업을 수행함
  - 자원의 표현(Representation)은 JSON, XML 등 다양한 형식으로 제공될 수 있음


- **Client-Server Architecture** 
  - 클라이언트와 서버는 독립적으로 개발 및 배포될 수 있음
  - 클라이언트는 서버의 구현에 의존하지 않으며, 서버는 클라이언트의 구현에 의존하지 않음
  - 클라이언트와 서버는 서로 다른 기술 스택을 사용할 수 있음


## 3. RESTful API 설계 원칙
  - 자원은 URI로 식별됨
  - URI는 명사 형태로 작성하고, 동사는 사용하지 않음
  - URI는 계층 구조를 반영해야 함
  - 예시: `/users`, `/users/{id}`, `/products/{id}/reviews`


## 4. HTTP 메서드
- **GET** : 자원 조회
  - 서버에서 자원을 가져옴
  - 요청 본문 없음
  - 안전하고 idempotent(멱등성)함


- **POST** : 자원 생성
  - 서버에 새로운 자원을 생성함
  - 요청 본문에 데이터를 포함할 수 있음
  - 안전하지 않음


- **PUT** : 자원 업데이트
  - 서버에 기존 자원을 업데이트함
  - 요청 본문에 데이터를 포함할 수 있음
  - 안전하지 않음
  - idempotent(멱등성)함


- **DELETE** : 자원 삭제
  - 서버에서 자원을 삭제함
  - 요청 본문 없음
  - 안전하지 않음
  - idempotent(멱등성)함
  
## 5. HTTP 상태 코드
- **200 OK** : 요청이 성공적으로 처리됨
- **201 Created** : 자원이 성공적으로 생성됨
- **204 No Content** : 요청이 성공적으로 처리되었지만, 응답 본문 없음


- **301 Moved Permanently** : 요청한 자원이 영구적으로 이동됨
- **302 Found** : 요청한 자원이 임시로 이동됨
- **304 Not Modified** : 요청한 자원이 수정되지 않음


- **400 Bad Request** : 클라이언트의 요청이 잘못됨
- **401 Unauthorized** : 인증이 필요함
- **403 Forbidden** : 요청이 금지됨
- **404 Not Found** : 요청한 자원이 없음


- **500 Internal Server Error** : 서버에서 오류가 발생함
- **503 Service Unavailable** : 서버가 일시적으로 사용할 수 없음
- **504 Gateway Timeout** : 서버가 요청을 처리하는 데 시간이 초과됨
- **403 Forbidden** : 요청이 금지됨

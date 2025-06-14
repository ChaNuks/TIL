# v1 스타일 - 장바구니 상품 추가 / 조회 / 수량 변경 / 상품 제거

## 구조 (Legacy 방식)
- Controller
- DTO
- Repository
- Service

## 기능
### 1. 상품 추가
~~~
POST /v1/carts/items
입력값 : name, price, quantity
검증 : @Valid
인메모리 DB에 상품 정보 저장
성공 시 응답으로 id, name, price, quantity, sellStatus 응답
~~~

### 2. 상품 조회
~~~
GET /v1/carts/{cartId}
입력값 : 
검증 : @Valid
인메모리 DB에서 상품 정보 검색
성공 시 응답으로 id, name, price, quantity, sellStatus 응답
~~~

### 3. 상품 수량 변경
~~~
PATCH /v1/carts/{cartId}/products/{productId}
입력값 : quantity 
검증 : @Valid
인메모리 DB에서 상품 수량 변경
성공 시 응답으로 cartId, productId, quantity, sellStatus 응답
~~~

### 4. 상품 제거
~~~
DELETE /v1/carts/{cartId}/products/{productId}
입력값 : 
검증 : @Valid
인메모리 DB에서 상품 제거
성공 시 응답으로 "상품 제거가 완료되었습니다."출력
~~~


## 추가로 구현
- JPA로 DB 연동
- 비밀번호 암호화
- 세션 or 토큰(JWT) 기반 로그인
- 사용자 인증/인가 처리
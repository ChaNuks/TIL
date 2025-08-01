# 장바구니 API 구현

## 기능 요구사항
### 1. 장바구니에 단일 상품 추가
### 2. 장바구니에서 상품 목록 조회
### 3. 장바구니에 담긴 상품 목록에서 특정 상품 수량 변경
### 4. 장바구니에 담긴 상품 목록에서 특정 상품 제거

---

## 요청과 응답
### 1. 추가
```
POST /carts HTTP/1.1
Content-Type: application/json

{
  "cartId": 1,
  "products": [
    {
      "id": 1,
      "name": "청상추",
      "price": 2000,
      "quantity": 5,
      "sellStatus": "ON_SALE"
    },
    {
      "id": 2,
      "name": "양송이 버섯",
      "price": 1500,
      "quantity": 6,
      "sellStatus": "ON_SALE"
    }
  ]
}
```
```
{
  "createdAt": "2025-06-15T21:10:48",
  "createdBy": "ChaNuks"
}
```


### 2. 목록 조회
```
GET /carts/{cartId} HTTP/1.1
Content-Type: application/json

{
  "cartId": 1,
  "products": [
    {
      "productId": 1,
      "name": "청상추",
      "price": 2000,
      "quantity": 5,
      "sellStatus": "ON_SALE"
    },
    {
      "productId": 2,
      "name": "양송이 버섯",
      "price": 1500,
      "quantity": 6,
      "sellStatus": "ON_SALE"
    }
  ]
}
```

### 3. 상품수량 변경
```
PATCH /carts/{cartId}/products/{productId} HTTP/1.1
Content-Type: application/json

{
  "quantity": 0,
  "sellStatus": "OUT_OF_STOCK"
}
```
```
{
  "cartId": 1,
  "productId": 1,
  "quantity": 0,
  "sellStatus": "OUT_OF_STOCK"
}
```

### 4. 상품 제거
```
DELETE /carts/{cartId}/products/{productId} HTTP/1.1
```

---

## 설계
### 1. bmarket-api 모듈, storage 모듈 구성
### 2. bmarket-api 모듈의 Controller, Service, Repository, DTO 구성
### 3. storage 모듈의 Entity, Repository, Base Entity, JPA Auditing 구성
### 4. postgresql 데이터베이스 생성
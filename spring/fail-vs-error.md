# Fail과 Error

## Fail와 Error차이
### 1. Fail
- 요청은 정상적으로 처리되었지만, 비즈니스 로직상 실패
- 주로 200 OK로 응답, 응답 body에서 실패 여부 전달 (success : false)
- 서버는 요청을 이해하고 정상 처리함
- 정상적인 공통 응답 포맷
- ex) 로그인 실패, 재고 부족, 결제 거절

### 2. Error
- 요청 처리 중 예기치 못한 예외나 시스템 문제가 발생한 것
- 주로 400, 500 상태 코드
- 서버에서 요청 처리 중 에러 발생
- 에러 핸들러에 의해 응답 포맷이 다를 수 있음
- ex) NullPointerException, DB 연결 실패, 500 Internal Server Error

---

## 예시
### 1. Fail
- 요청은 성공했지만, 논리적 실패
- 서버는 요청 잘 받고 정상 응답을 했지만, 비즈니스 로직 기준에선 실패
```http
POST /v3/login

HTTP/1.1 200 OK
{
  "success": false,
  "message": "비밀번호가 틀렸습니다",
  "data": {
    "code": "LOGIN_FAIL",
    "message": "아이디 또는 비밀번호가 일치하지 않습니다"
  }
}
```

### 2. Error
- 요청 처리 중 예외나 시스템 문제가 발생한 것
- 서버에서 요청 처리 중에 예외 발생
- 예외 핸들러에 의해 응답 포맷이 다를 수 있음
```http
POST /v3/login

HTTP/1.1 500 Internal Server Error
{
  "success": false,
  "message": "실패",
  "data": {
    "code": "INTERNAL_ERROR",
    "message": "예상치 못한 오류가 발생했습니다"
  }
} 
```
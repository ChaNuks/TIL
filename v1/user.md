# v1 스타일 - 회원가입 / 로그인 / 로그아웃 구현

## 구조 (Legacy 방식)
- Controller
- DTO
- Repository
- Service

## 기능
### 1. 회원가입
~~~
POST /v1/users/signup
입력값 : name, email, password
검증 : @Valid 또는 @Validation 사용(이메일 중복 확인)
인메모리 DB에 사용자 정보 저장
성공 시 콘솔에 "회원가입이 완료되었습니다."출력
~~~

### 2. 로그인
~~~
POST /v1/users/login
입력값 : email, password
검증 : @Valid 또는 @Validation 사용(이메일, 비밀번호 검증)
인메모리 DB에서 사용자 정보 검색
성공 시 콘솔에 "로그인에 성공했습니다."출력
~~~

### 3. 로그아웃
~~~
GET /v1/users/logout
검증 : 
인메모리 DB에서 사용자 정보 검색
성공 시 콘솔에 "로그아웃 되었습니다."출력
HTTP 응답 : 204 No Content
~~~

## 추가로 구현
- JPA로 DB 연동
- 비밀번호 암호화
- 세션 or 토큰(JWT) 기반 로그인
- 사용자 인증/인가 처리
# SQL 명령어
- SQL(Structured Query Language)은 관계형 데이터베이스에서 데이터를 정의하고 조작하기 위한 표준 언어입니다. SQL 명령어는 크게 DDL(Data Definition Language), DML(Data Manipulation Language), DCL(Data Control Language)로 구분
- DDL, DML, DCL이 무엇인지 알아보자
---

## SQL 명령어 종류
| 종류 | 설명 |
|------|------|
| DDL  | 데이터베이스 구조를 정의하는 명령어로, 테이블, 인덱스, 뷰 등을 생성, 수정, 삭제하는 데 사용 |
| DML  | 데이터베이스의 데이터를 조작하는 명령어로, 데이터 삽입, 조회, 수정, 삭제 등을 수행 |
| DCL  | 데이터베이스 사용자 권한을 제어하는 명령어로, 사용자 생성, 권한 부여, 권한 회수 등을 수행 |
---

## DDL (Data Definition Language)
- 데이터베이스 객체의 구조를 정의하고 변경하는 명령어
- 주로 테이블, 인덱스, 뷰 등을 생성, 수정, 삭제하는 데 사용

| 명령어 | 설명 |
|--------|------|
| CREATE | 새로운 데이터베이스 객체(테이블, 인덱스, 뷰 등)를 생성 |
| ALTER  | 기존 데이터베이스 객체의 구조를 변경 |
| DROP   | 데이터베이스 객체를 삭제 |
| TRUNCATE | 테이블의 모든 데이터를 삭제하지만, 테이블 구조는 유지 |
| RENAME | 데이터베이스 객체의 이름을 변경 |
| SHOW   | 데이터베이스 객체의 정보를 조회 |
| USE    | 특정 데이터베이스를 선택 |
| DESCRIBE | 테이블의 구조를 조회 |
| CREATE INDEX | 테이블에 인덱스를 생성 |
| DROP INDEX | 테이블에서 인덱스를 삭제 |


## DML (Data Manipulation Language)
- 데이터베이스의 데이터를 조작하는 명령어
- 데이터 삽입, 조회, 수정, 삭제 등을 수행

| 명령어 | 설명 |
|--------|------|
| SELECT | 데이터베이스에서 데이터를 조회 |
| INSERT | 데이터베이스에 새로운 데이터를 삽입 |
| UPDATE | 데이터베이스의 기존 데이터를 수정 |
| DELETE | 데이터베이스에서 데이터를 삭제 |


## DCL (Data Control Language)
- 데이터베이스 사용자 권한을 제어하는 명령어
- 사용자 생성, 권한 부여, 권한 회수 등을 수행

| 명령어 | 설명 |
|--------|------|
| GRANT  | 사용자에게 특정 권한을 부여 |
| REVOKE | 사용자에게 부여된 권한을 회수 |
| COMMIT | 트랜잭션을 커밋하여 변경 사항을 영구적으로 저장 |
| ROLLBACK | 트랜잭션을 롤백하여 변경 사항을 취소 |


## SQL 명령어 예시
```sql
-- DDL 예시
CREATE TABLE users (
    id INT PRIMARY KEY,
    name VARCHAR(100),
    email VARCHAR(100) UNIQUE
);
ALTER TABLE users ADD COLUMN age INT;
DROP TABLE users;
TRUNCATE TABLE users;
RENAME TABLE users TO customers;

-- DML 예시
SELECT * FROM users;
UPDATE users SET age = 30 WHERE id = 1;
DELETE FROM users WHERE id = 1;
INSERT INTO users (id, name, email) VALUES (1, 'John Doe', 'jd@gmail.com');

-- DCL 예시
GRANT SELECT, INSERT ON users TO 'user1';
REVOKE INSERT ON users FROM 'user1';
COMMIT;
ROLLBACK;
```

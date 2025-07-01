# ERD (Entity Relationship Diagram)

## 1. ERD란?
- 데이터베이스의 구조를 시각적으로 표현한 다이어그램
- `엔티티(Entity)`, `속성(Attribute)`, `관계(Relationship)`를 나타냄
- 데이터베이스 설계 및 이해를 돕기 위해 사용됨

---

## 2. ERD 구성 요소
### 2.1 엔티티(Entity)
- 데이터베이스에서 관리되는 객체
- 예: 사용자, 주문, 제품 등
- 각 엔티티는 고유한 식별자(Primary Key)를 가짐
- 엔티티는 사각형으로 표현됨

### 2.2 속성(Attribute)
- 엔티티의 특성이나 정보를 나타냄
- 예: 사용자 엔티티의 이름, 이메일, 전화번호 등
- 속성은 엔티티 내부에 속하는 타원형으로 표현됨

### 2.3 관계(Relationship)
- 엔티티 간의 연관성을 나타냄
- 예: 사용자와 주문 간의 관계, 제품과 카테고리 간의 관계 등
- 관계는 선으로 연결되며, 관계의 종류(1:1, 1:N, M:N 등)에 따라 다르게 표현됨

--- 

## 3. ERD 설계 원칙
### 3.1 정규화(Normalization)
- 데이터 중복을 최소화하고, 데이터 무결성을 유지하기 위해 테이블을 분리하는 과정
- 1NF, 2NF, 3NF 등의 정규형을 따름
- 정규화를 통해 데이터베이스의 구조를 최적화하고, 데이터의 일관성을 유지함
- 예: 사용자와 주문 정보를 분리하여 각각의 엔티티로 관리

### 3.2 관계의 종류
- **1:1 관계**: 한 엔티티가 다른 엔티티와 1:1로 연결됨
  - 예: 사용자와 프로필
  - ERD 표현: 두 엔티티를 선으로 연결하고, 선 위에 "1"을 표시
    ```plaintext
    User 1 ── 1 Profile
    ```
- **1:N 관계**: 한 엔티티가 다른 엔티티와 1:N으로 연결됨
  - 예: 사용자와 주문
  - ERD 표현: 한 엔티티에서 다른 엔티티로 향하는 선 위에 "1"과 "N"을 표시
    ```plaintext
    User 1 ── N Order
    ```
- **M:N 관계**: 두 엔티티가 N:M으로 연결됨
  - 예: 학생과 강의
  - ERD 표현: 두 엔티티를 연결하는 중간 테이블을 생성하여 각각의 엔티티와 1:N 관계로 연결
    ```plaintext
      Student N ── M Enrollment ── N Course
    ```
### 3.3 식별자(Identifier)
- 각 엔티티는 고유한 식별자를 가져야 함
- 식별자는 엔티티의 속성 중 하나로, 주로 기본 키(Primary Key)로 사용됨
- 식별자는 엔티티의 유일성을 보장하고, 다른 엔티티와의 관계를 정의하는 데 사용됨
  - 예: 사용자 엔티티의 ID, 주문 엔티티의 주문 번호 등

### 3.4 관계의 카디널리티(Cardinality)
- 관계의 카디널리티는 엔티티 간의 관계에서 각 엔티티가 가질 수 있는 인스턴스의 수를 나타냄
- 카디널리티는 ERD에서 관계의 선 위에 표시됨
  - 예: 1:1, 1:N, M:N 등
- 카디널리티는 데이터베이스의 무결성을 유지하고, 관계의 의미를 명확히 하는 데 중요함
  - 예: 사용자와 주문 간의 관계에서, 한 사용자는 여러 개의 주문을 가질 수 있지만, 각 주문은 하나의 사용자에게만 속함
    ```plaintext
    User 1 ── N Order
    ```
---

## 4. 테이블 구성 팁
### 4.1 created_at, / updated_at / created_by / updated_by
- 각 엔티티에 생성일(created_at), 수정일(updated_at), 생성자(created_by), 수정자(updated_by) 등의 메타데이터를 포함하는 것이 좋음
- 이러한 메타데이터는 데이터의 추적성과 관리에 유용함
- 예시:
```plaintext
CREATE TABLE User (
    id INT PRIMARY KEY,
    name VARCHAR(100),
    email VARCHAR(100),
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
    created_by VARCHAR(50),
    updated_by VARCHAR(50)
);
```

### 4.2 인덱스(Index)
- 자주 조회되는 컬럼에 인덱스를 추가하여 조회 성능을 향상시킬 수 있음
- 인덱스는 검색 속도를 높이지만, 데이터 삽입/수정 시 성능 저하를 초래할 수 있으므로 신중하게 사용해야 함
- 예시:
```plaintext
CREATE INDEX idx_user_email ON User(email);
```

### 4.3 외래 키(Foreign Key)
- 엔티티 간의 관계를 정의할 때 외래 키를 사용하여 데이터 무결성을 유지할 수 있음
- 외래 키는 다른 테이블의 기본 키를 참조하여 관계를 설정함
- 예시:
```plaintext
CREATE TABLE Order (
    id INT PRIMARY KEY,
    user_id INT,
    order_date TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    FOREIGN KEY (user_id) REFERENCES User(id)
);
```

### 4.4 제약 조건(Constraints)
- 데이터의 무결성을 보장하기 위해 제약 조건을 설정할 수 있음
- 예시:
  - NOT NULL: 컬럼이 NULL 값을 가질 수 없도록 설정
  - UNIQUE: 컬럼의 값이 유일하도록 설정
  - CHECK: 특정 조건을 만족해야 함
  - DEFAULT: 컬럼의 기본값을 설정
  - 예시:
    ```plaintext
    CREATE TABLE User (
        id INT PRIMARY KEY,
        name VARCHAR(100) NOT NULL,
        email VARCHAR(100) UNIQUE,
        created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
        updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP
    );
    ```
### 4.5 데이터 타입(Data Types)
- 각 컬럼에 적절한 데이터 타입을 선택하여 저장 공간을 최적화하고 성능을 향상시킬 수 있음
- 예시:
  - 문자열: VARCHAR, CHAR
  - 정수: INT, BIGINT
  - 실수: FLOAT, DOUBLE
  - 날짜/시간: DATE, TIMESTAMP
  - 불리언: BOOLEAN
  - 예시:
    ```plaintext
    CREATE TABLE User (
        id INT PRIMARY KEY,
        name VARCHAR(100),
        email VARCHAR(100) UNIQUE,
        created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
        updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP
    );
    ```
### 4.6 중간 테이블로 N:M 관계 처리
- M:N 관계를 처리하기 위해 중간 테이블을 생성하여 각 엔티티 간의 관계를 정의할 수 있음
- 중간 테이블은 두 엔티티의 기본 키를 외래 키로 사용하여 관계를 설정함
- 예시:
```plaintext
CREATE TABLE Student (
    id INT PRIMARY KEY,
    name VARCHAR(100)
);
CREATE TABLE Course (
    id INT PRIMARY KEY,
    title VARCHAR(100)
);
CREATE TABLE Enrollment (
    student_id INT,
    course_id INT,
    PRIMARY KEY (student_id, course_id),
    FOREIGN KEY (student_id) REFERENCES Student(id),
    FOREIGN KEY (course_id) REFERENCES Course(id)
);
```
### 4.7 상태 값은 별도의 테이블로 관리
- 상태 값이 있는 엔티티는 상태 값을 별도의 테이블로 관리하여 유연성을 높일 수 있음
- 상태 값 테이블은 상태의 정의와 관련된 정보를 포함함
- 예시:
```plaintext
CREATE TABLE OrderStatus (
    id INT PRIMARY KEY,
    status_name VARCHAR(50) NOT NULL
);
CREATE TABLE Order (
    id INT PRIMARY KEY,
    user_id INT,
    order_date TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    status_id INT,
    FOREIGN KEY (user_id) REFERENCES User(id),
    FOREIGN KEY (status_id) REFERENCES OrderStatus(id)
);
```
### 4.8 다대다 관계 처리
- 다대다(N:M) 관계를 처리하기 위해 중간 테이블을 생성하여 각 엔티티 간의 관계를 정의할 수 있음
- 중간 테이블은 두 엔티티의 기본 키를 외래 키로 사용하여 관계를 설정함
- 예시:
```plaintext
CREATE TABLE Student (
    id INT PRIMARY KEY,
    name VARCHAR(100)
);
CREATE TABLE Course (
    id INT PRIMARY KEY,
    title VARCHAR(100)
);
CREATE TABLE Enrollment (
    student_id INT,
    course_id INT,
    PRIMARY KEY (student_id, course_id),
    FOREIGN KEY (student_id) REFERENCES Student(id),
    FOREIGN KEY (course_id) REFERENCES Course(id)
);
```
### 4.9 복합 키(Composite Key)
- 두 개 이상의 컬럼을 조합하여 기본 키를 구성할 수 있음
- 복합 키는 엔티티의 유일성을 보장하는 데 사용됨
- 예시:
```plaintext
CREATE TABLE Enrollment (
    student_id INT,
    course_id INT,
    PRIMARY KEY (student_id, course_id),
    FOREIGN KEY (student_id) REFERENCES Student(id),
    FOREIGN KEY (course_id) REFERENCES Course(id)
);
```
### 4.10 데이터베이스 정규화
- 데이터베이스 정규화는 데이터 중복을 최소화하고, 데이터 무결성을 유지하기 위해 테이블을 분리하는 과정
- 정규화는 1NF, 2NF, 3NF 등의 정규형을 따름
- 정규화를 통해 데이터베이스의 구조를 최적화하고, 데이터의 일관성을 유지함
- 예시:
```plaintext
CREATE TABLE User (
    id INT PRIMARY KEY,
    name VARCHAR(100),
    email VARCHAR(100) UNIQUE
);
CREATE TABLE Order (
    id INT PRIMARY KEY,
    user_id INT,
    order_date TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    FOREIGN KEY (user_id) REFERENCES User(id)
);
```
### 4.11 데이터베이스 인덱스
- 데이터베이스 인덱스는 검색 성능을 향상시키기 위해 사용됨
- 자주 조회되는 컬럼에 인덱스를 추가하여 조회 성능을 향상시킬 수 있음
- 인덱스는 검색 속도를 높이지만, 데이터 삽입/수정 시 성능 저하를 초래할 수 있으므로 신중하게 사용해야 함
- 예시:
```plaintext
CREATE INDEX idx_user_email ON User(email);
```
### 4.12 트랜잭션 관리
- 데이터베이스 트랜잭션은 일련의 데이터베이스 작업을 하나의 단위로 묶어 처리하는 것을 의미함
- 트랜잭션은 원자성, 일관성, 격리성, 지속성(ACID) 속성을 만족해야 함
- 트랜잭션은 데이터베이스의 무결성을 유지하고, 데이터의 일관성을 보장하는 데 중요함
- 예시:
```plaintext
BEGIN TRANSACTION;
UPDATE User SET balance = balance - 100 WHERE id = 1;
UPDATE User SET balance = balance + 100 WHERE id = 2;
COMMIT;
```
### 4.13 데이터베이스 백업 및 복구
- 데이터베이스 백업은 데이터 손실을 방지하기 위해 주기적으로 수행해야 함
- 백업은 데이터베이스의 전체 또는 일부 데이터를 저장하는 것을 의미함
- 복구는 데이터베이스의 손상이나 오류로부터 데이터를 복원하는 과정을 의미함
- 백업 및 복구 전략은 데이터베이스의 중요성과 사용 패턴에 따라 다르게 설정해야 함
- 예시:
```plaintext
BACKUP DATABASE mydb TO DISK = 'C:\backup\mydb.bak';
RESTORE DATABASE mydb FROM DISK = 'C:\backup\mydb.bak';
```

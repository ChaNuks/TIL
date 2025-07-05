# PostgreSQL 설치

## 설치 및 사용 (macOS)
### 1. Homebrew를 이용한 설치
```bash
brew install postgresql
```
### 2. PostgreSQL 서비스 시작
```bash
brew services start postgresql
```
### 3. PostgreSQL 버전 확인
```bash
psql --version
```
### 4. PostgreSQL 데이터베이스 접속
```bash
psql postgres
```
### 5. PostgreSQL 데이터베이스 생성
```sql
CREATE DATABASE mydatabase;
```
### 6. PostgreSQL 사용자 생성
```sql
CREATE USER
myuser WITH PASSWORD 'mypassword';
```
### 7. PostgreSQL 사용자에게 권한 부여
```sql
GRANT ALL PRIVILEGES ON DATABASE mydatabase TO myuser;
```
### 8. PostgreSQL 접속 테스트
```bash
psql -U myuser -d mydatabase
```
### 9. PostgreSQL 서비스 중지
```bash
brew services stop postgresql
```
### 10. PostgreSQL 데이터베이스 삭제
```sql
DROP DATABASE mydatabase;
```
### 11. PostgreSQL 사용자 삭제
```sql
DROP USER myuser;
```
### 12. PostgreSQL 데이터베이스 백업
```bash
pg_dump mydatabase > mydatabase_backup.sql
```
### 13. PostgreSQL 데이터베이스 복원
```bash
psql mydatabase < mydatabase_backup.sql
```
### 14. PostgreSQL 설정 파일 위치
```bash
cat /usr/local/var/postgres/postgresql.conf
```
### 15. PostgreSQL 로그 파일 위치
```bash
cat /usr/local/var/postgres/server.log
```
### 16. PostgreSQL 클라이언트 설치
```bash
brew install libpq
```
### 17. PostgreSQL 클라이언트 실행
```bash
pgcli -h localhost -U myuser -d mydatabase
```
### 18. PostgreSQL 클라이언트 접속
```bash
psql -h localhost -U myuser -d mydatabase
```
### 19. PostgreSQL 클라이언트 종료
```sql
\q
```
### 20. PostgreSQL 데이터베이스 상태 확인
```bash
pg_ctl status
```
### 21. PostgreSQL 데이터베이스 재시작
```bash
pg_ctl restart
```
### 22. PostgreSQL 데이터베이스 설정 변경
```bash
nano /usr/local/var/postgres/postgresql.conf
```
### 23. PostgreSQL 데이터베이스 설정 적용
```bash
pg_ctl reload
```
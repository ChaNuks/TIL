# 🐳 Docker 설치 방법

## Docker 설치

### macOS
```bash
brew install --cask docker
```
OR
```bash
1. Docker Desktop for Mac (Apple Chip) 다운로드
2. .dmg 실행 → Docker.app을 Applications 폴더에 드래그
3. /Applications/Docker.app 실행
4. Docker 실행 후 터미널에서 docker --version 으로 확인
```

### Docker Desktop CLI 확인
```bash
docker --version
docker-compose --version
docker exec -it local-market-postgres-dbms psql -U myuser -d mydb (PostgreSQL 접속)
```

### 실행
```bash
cd ~/Desktop/project/local-market/docker (프로젝트 경로로 이동)
docker compose up -d (백그라운드 실행)
```

---
### Windows
1. [Docker Desktop for Windows](https://www.docker.com/products/docker-desktop) 다운로드
2. 설치 후 Docker Desktop 실행
3. Docker가 정상적으로 실행되는지 확인
4. Docker Desktop 설정에서 WSL 2를 활성화 (Windows Subsystem for Linux 2 사용)
5. Docker Desktop에서 WSL 2 통합 활성화
6. Docker Desktop에서 Linux 배포판 선택 (예: Ubuntu)
7. Docker Desktop에서 Docker CLI 사용 가능
8. Docker Desktop에서 Docker Compose 사용 가능
9. Docker Desktop에서 Kubernetes 활성화 (선택 사항)
10. Docker Desktop에서 Docker Hub 계정 로그인 (선택 사항)
11. Docker Desktop에서 이미지 및 컨테이너 관리
12. Docker Desktop에서 설정 변경 (예: 리소스 할당, 프록시 설정 등)
13. Docker Desktop에서 업데이트 확인 및 설치
14. Docker Desktop에서 로그아웃 및 종료
15. Docker Desktop에서 재시작

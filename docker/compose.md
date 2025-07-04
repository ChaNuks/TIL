# 🐳 Docker Compose 정리

## Docker Compose란?
- 여러 개의 Docker 컨테이너를 정의하고 실행할 수 있는 도구
- YAML 파일을 사용하여 애플리케이션의 서비스, 네트워크, 볼륨 등을 정의
- `docker-compose up` 명령어로 모든 서비스를 한 번에 시작 가능
- 개발 환경에서 여러 컨테이너를 쉽게 관리하고 배포할 수 있도록 도와줌

---

## Docker Compose의 구성 요소
| 구성 요소 | 설명 |
| --- | --- |
| 서비스 | 컨테이너의 실행 단위로, Docker 이미지와 설정을 포함 |
| 네트워크 | 컨테이너 간 통신을 위한 가상 네트워크 |
| 볼륨 | 컨테이너 간 데이터 공유를 위한 저장소 |
| 환경 변수 | 컨테이너 실행 시 사용할 환경 변수 설정 |
| 의존성 | 서비스 간의 의존성을 정의하여 순차적으로 시작 가능 |
| 이미지 | 컨테이너를 실행하기 위한 템플릿 |
| Dockerfile | 서비스의 Docker 이미지를 정의하는 파일 |
| Compose 파일 | 여러 서비스를 정의하는 YAML 파일 |
| Compose CLI | Docker Compose를 관리하는 명령줄 인터페이스 |
| Compose 프로젝트 | 여러 서비스와 설정을 포함하는 전체 애플리케이션 단위 |
```yaml
version: '3.8'
services:
  web:
    image: nginx:latest
    ports:
      - "80:80"
    networks:
      - my_network
  db:
    image: mysql:latest
    environment:
      MYSQL_ROOT_PASSWORD: example
    networks:
      - my_network
networks:
    my_network:
        driver: bridge
```
---

## Docker Compose 명령어
| 명령어 | 설명 |
| --- | --- |
| `docker-compose up` | 모든 서비스를 시작하고 실행 |
| `docker-compose down` | 모든 서비스를 중지하고 네트워크 및 볼륨을 제거 |
| `docker-compose build` | 서비스의 이미지를 빌드 |
| `docker-compose logs` | 서비스의 로그를 출력 |
| `docker-compose ps` | 현재 실행 중인 서비스의 상태를 확인 |
| `docker-compose exec` | 실행 중인 컨테이너에 명령어를 실행 |
| `docker-compose stop` | 실행 중인 서비스를 중지 |
| `docker-compose start` | 중지된 서비스를 시작 |

---

## Docker Compose 파일 구조
```yaml
version: '3.8'  # Compose 파일 버전
services:  # 서비스 정의
  web:  # 서비스 이름
    image: nginx:latest  # 사용할 Docker 이미지
    ports:  # 포트 매핑
      - "80:80"  # 호스트 포트:컨테이너 포트
    networks:  # 네트워크 설정
      - my_network  # 사용할 네트워크 이름
  db:  # 또 다른 서비스 이름
    image: mysql:latest  # 사용할 Docker 이미지
    environment:  # 환경 변수 설정
      MYSQL_ROOT_PASSWORD: example  # MySQL 루트 비밀번호 설정
    networks:
      - my_network  # 사용할 네트워크 이름
networks:  # 네트워크 정의
    my_network:  # 네트워크 이름
        driver: bridge  # 네트워크 드라이버 설정
```
---

## .env 파일
- Docker Compose에서 환경 변수를 관리하기 위한 파일
- `.env` 파일은 Docker Compose 파일과 동일한 디렉토리에 위치해야 함
- 환경 변수는 `KEY=VALUE` 형식으로 정의
- Docker Compose 파일에서 `${KEY}` 형식으로 참조 가능
- 예시:
```env
DB_HOST=localhost
DB_PORT=5432
DB_USER=user
DB_PASSWORD=password
```
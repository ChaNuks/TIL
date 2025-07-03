# 🐳 Docker 명령어

## Docker 명령어
### 1. Docker 이미지 관련 명령어
```bash
# 이미지 목록 조회
docker images

# 이미지 삭제
docker rmi <이미지_ID>

# 이미지 빌드
docker build -t <이미지_이름>:<태그> <디렉토리>

# 이미지 태그
docker tag <이미지_ID> <새로운_이미지_이름>:<태그>

# 이미지 푸시
docker push <이미지_이름>:<태그>

# 이미지 풀
docker pull <이미지_이름>:<태그>

# 이미지 검색
docker search <검색어>

# 이미지 상세 정보 조회
docker inspect <이미지_ID>

# 이미지 내 파일 목록 조회
docker run --rm <이미지_ID> ls -l /path/to/directory

# 이미지 내 파일 검색
docker run --rm <이미지_ID> find /path/to/directory -name "<파일_이름>"

# 이미지 내 파일 내용 조회
docker run --rm <이미지_ID> cat /path/to/file

# 이미지 내 파일 수정
docker run --rm -v <호스트_경로>:/path/to/directory <이미지_ID> sh -c "echo '내용' > /path/to/directory/파일명"

# 이미지 내 파일 삭제
docker run --rm -v <호스트_경로>:/path/to/directory <이미지_ID> sh -c "rm /path/to/directory/파일명"

# 이미지 내 파일 복사
docker run --rm -v <호스트_경로>:/path/to/directory <이미지_ID> sh -c "cp /path/to/directory/파일명 /path/to/directory/새로운_파일명"

# 이미지 내 파일 이동
docker run --rm -v <호스트_경로>:/path/to/directory <이미지_ID> sh -c "mv /path/to/directory/파일명 /path/to/directory/새로운_파일명"

# 이미지 내 파일 권한 변경
docker run --rm -v <호스트_경로>:/path/to/directory <이미지_ID> sh -c "chmod 644 /path/to/directory/파일명"

# 이미지 내 파일 소유자 변경
docker run --rm -v <호스트_경로>:/path/to/directory <이미지_ID> sh -c "chown user:group /path/to/directory/파일명"

# 이미지 내 파일 압축
docker run --rm -v <호스트_경로>:/path/to/directory <이미지_ID> sh -c "tar -czf /path/to/directory/압축파일.tar.gz /path/to/directory/파일명"

# 이미지 내 파일 압축 해제
docker run --rm -v <호스트_경로>:/path/to/directory <이미지_ID> sh -c "tar -xzf /path/to/directory/압축파일.tar.gz -C /path/to/directory"
```

### 2. Docker 컨테이너 관련 명령어
```bash
# 컨테이너 목록 조회
docker ps

# 모든 컨테이너 목록 조회 (중지된 컨테이너 포함)
docker ps -a

# 컨테이너 시작
docker start <컨테이너_ID>

# 컨테이너 중지
docker stop <컨테이너_ID>

# 컨테이너 재시작
docker restart <컨테이너_ID>

# 컨테이너 삭제
docker rm <컨테이너_ID>

# 컨테이너 실행
docker run -d --name <컨테이너_이름> <이미지_이름>:<태그>

# 컨테이너 실행 (포트 매핑)
docker run -d --name <컨테이너_이름> -p <호스트_포트>:<컨테이너_포트> <이미지_이름>:<태그>

# 컨테이너 실행 (환경 변수 설정)
docker run -d --name <컨테이너_이름> -e <환경_변수>=<값> <이미지_이름>:<태그>

# 컨테이너 실행 (볼륨 마운트)
docker run -d --name <컨테이너_이름> -v <호스트_경로>:/path/in/container <이미지_이름>:<태그>

# 컨테이너 실행 (네트워크 설정)
docker run -d --name <컨테이너_이름> --network <네트워크_이름> <이미지_이름>:<태그>

# 컨테이너 실행 (백그라운드 모드)
docker run -d --name <컨테이너_이름> <이미지_이름>:<태그>

# 컨테이너 실행 (인터랙티브 모드)
docker run -it --name <컨테이너_이름> <이미지_이름>:<태그>

# 컨테이너 실행 (쉘 접속)
docker run -it --name <컨테이너_이름> <이미지_이름>:<태그> /bin/bash

# 컨테이너 실행 (명령어 실행)
docker run --rm <이미지_이름>:<태그> <명령어>

# 컨테이너 로그 조회
docker logs <컨테이너_ID>

# 컨테이너 로그 실시간 조회
docker logs -f <컨테이너_ID>

# 컨테이너 상태 조회
docker inspect <컨테이너_ID>

# 컨테이너 상태 확인
docker ps -f "id=<컨테이너_ID>"

# 컨테이너 상태 변경
docker update --restart=always <컨테이너_ID>

# 컨테이너 상태 변경 (메모리 제한)
docker update --memory=<메모리_크기> <컨테이너_ID>

# 컨테이너 상태 변경 (CPU 제한)
docker update --cpus=<CPU_수> <컨테이너_ID>

# 컨테이너 상태 변경 (환경 변수 추가)
docker update --env-add <환경_변수>=<값> <컨테이너_ID>

# 컨테이너 상태 변경 (볼륨 추가)
docker update --mount-add type=bind,source=<호스트_경로>,target=/path/in/container <컨테이너_ID>

# 컨테이너 상태 변경 (네트워크 추가)
docker update --network-add <네트워크_이름> <컨테이너_ID>

# 컨테이너 상태 변경 (리소스 제한)
docker update --memory=<메모리_크기> --cpus=<CPU_수> <컨테이너_ID>

# 컨테이너 상태 변경 (재시작 정책 설정)
docker update --restart=<정책> <컨테이너_ID>
```

# 컨테이너 실행 (백그라운드 모드)
```bash
docker run -d --name <컨테이너_이름> <이미지_이름>:<태그>
```

# 컨테이너 실행 (인터랙티브 모드)
```bash
docker run -it --name <컨테이너_이름> <이미지_이름>:<태그>
```

# 컨테이너 실행 (쉘 접속)
```bash
docker run -it --name <컨테이너_이름> <이미지_이름>:<태그> /bin/bash
```

# 컨테이너 실행 (명령어 실행)
```bash
docker run --rm <이미지_이름>:<태그> <명령어>
```


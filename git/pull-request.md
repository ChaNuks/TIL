# Pull Request

## 1. Pull Request란?
- 소스 코드 변경 사항을 다른 개발자에게 검토받고 병합하기 위한 요청
- Git에서 브랜치를 사용하여 작업한 후, 변경 사항을 메인 브랜치에 병합하기 위해 사용됨
- 협업 개발에서 코드 품질을 높이고, 버그를 사전에 방지하기 위한 중요한 과정


## 2. Pull Request 작성 방법
### 2.1. 브랜치 생성
- 작업할 기능이나 버그 수정에 대한 브랜치를 생성
```bash
git checkout -b feature/your-feature-name
```

### 2.2. 코드 변경
- 필요한 코드 변경을 수행
- 변경 사항을 커밋
```bash
git add .
git commit -m "Add your feature or fix description"
```

### 2.3. 원격 저장소에 푸시
- 변경 사항을 원격 저장소에 푸시
```bash
git push origin feature/your-feature-name
```


### 2.4. Pull Request 생성
- GitHub, GitLab 등에서 원격 저장소의 브랜치에 대해 Pull Request를 생성
- 제목과 설명을 작성하여 변경 사항을 명확히 설명
- 검토자(Reviewer)를 지정
- 필요한 경우 라벨(Label)이나 프로젝트(Project)를 추가
- "Create Pull Request" 버튼을 클릭하여 요청을 생성
- Pull Request가 생성되면, 변경 사항에 대한 리뷰를 요청할 수 있음
- 리뷰어는 코드 변경 사항을 검토하고, 피드백을 제공

### 2.5. Pull Request 리뷰
- 리뷰어가 코드 변경 사항을 검토하고, 필요한 경우 수정 요청
- 코드 스타일, 성능, 보안, 테스트 등을 검토
- 코드 변경 사항에 대한 의견을 남기거나, 추가 커밋을 요청할 수 있음
- 리뷰어가 승인하면, Pull Request를 병합할 수 있음
- 병합 후, 브랜치를 삭제하여 작업을 정리할 수 있음
```bash
git branch -d feature/your-feature-name
```


### 2.6. Pull Request 병합
- 리뷰가 완료되면, Pull Request를 병합(Merge)하여 메인 브랜치에 변경 사항을 반영
- 병합 방법은 Fast-forward, Squash and merge, Rebase and merge 등 여러 가지가 있음
- 병합 후, 원격 저장소의 메인 브랜치가 최신 상태로 업데이트됨
- 병합 후, 브랜치를 삭제하여 작업을 정리할 수 있음
```bash
git branch -d feature/your-feature-name
```


### 2.7. Pull Request 병합 후 작업
- 병합 후, 로컬 메인 브랜치를 업데이트
```bash
git checkout main
git pull origin main
```



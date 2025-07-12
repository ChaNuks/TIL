# 1. develop으로 이동해서 최신 코드 가져오기
- git checkout develop
- git pull origin develop

# 2. 다시 mission/12로 가서 develop 내용 머지 or 리베이스
- git checkout mission/12
- git rebase develop
# 또는
- git merge develop
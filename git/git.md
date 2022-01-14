# Git 명령어 

깃에는 매우 많은 명령어가 있지만, <br> 
나는 거의 Github에 push하여 버전관리, 협업, 원격저장소 이용 등으로 사용해왔다. <br>
내가 자주 사용하는 명령어 들을 정리해 보았다.

-----

1. **원격 저장소에 push 까지의 과정**
```
git init // Git 저장소 생성 (현재 디렉토리 기준)
git remote add origin 원격저장소 // 원격저장소 지정
git add . // 모든 파일 스테이징 
git commit -m "커밋메시지" // 커밋메시지로 커밋
git branch -M main // main 디폴트 브랜치로 설정
git push -u origin main // 원격 저장소로 push
```

2. **branch로 push** (1과정을 통해 원격저장소 지정 되어있을때)
```
git pull origin 브랜치 // 브랜치 코드를 pull
git checkout 브랜치 // 브랜치로 접근 (새로 생성과 접근을 같이 하고 싶을때 브랜치 앞 -b 추가)
git add . // 모든 파일 스테이징 
git commit -m "커밋메시지" // 커밋메시지로 커밋
git push origin 브랜치 // 브랜치로 push
```

3. **원격 저장소 push 취소**
```
git reset 돌아갈커밋시점 --hard // 돌아갈 시점 상태로 만들어줌 
git push origin -f // 코드 변경 이력으로 덮어 씌어준다.
```

4. **최근 commit 취소, commit명 변경**
```
git log // commit 목록 확인
git reset --soft HEAD^ // 가장 최근의 commit을 취소하고 add 상태는 보존 (안전지향)

git commit --amend // 커밋 명 변경
```

5. **불필요한 commit 합치기: rebase**
```
git rebase -i HEAD~개수 // 개수 만큼 합칠 커밋 지정( 지정할 pick을 정하고 합쳐질 커밋은 s로 바꿔준다. )
git push origin -f // 강제 push
```
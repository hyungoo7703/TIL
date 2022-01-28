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

6. **branch 생성 및 삭제**
```
git checkout -b 브랜치 // 브랜치 생성 및 접근
git branch // 브랜치 목록
git branch -D 브랜치 // 브랜치 제거 (로컬)
git push origin --delete 브랜치 // 원격 저장소에 push된 브랜치 제거
```

7. **git init 취소**
```
rm -r .git
```

8. **Pull requests, Merge requests 과정** <br>

결론: main 브랜치를 카피해서 병합 후 덮어 씌어주기 
```
git checkout main // main브랜치를 지웟다가 다시 하는걸 권장 -> 최신화를 위해
git pull origin main // 메인 브랜치 최신화
git branch copy // 메인 브랜치 카피 -> copy 브랜치를 생성
git checkout copy 
git merge 합치고픈브랜치 --squash // 이 과정에서 충돌 있다면 수정
git commit -am "main에 보내가고자하는 커밋 메시지" // add 및 commit -> 이때 main으로 보내고자 하는 커밋메세지를 입력해준다.
git push origin 합치고픈브랜치 -f // copy 브랜치에서 합치고픈 브랜치에 덮어 씌어준다.
git branch -D 합치고픈브랜치 // 예전 커밋 log가 보이기 때문에 지웟다가
git branch 합치고픈브랜치
git checkout 합치고픈브랜치
```
로그 재확인 후 main으로 보내고자 하는 커밋메세지가 있으면 완료된 것, 이후 깃헙 사이트에서 요청

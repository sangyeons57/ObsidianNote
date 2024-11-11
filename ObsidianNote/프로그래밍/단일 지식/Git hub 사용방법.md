git init
	.git폴더 생성

git status
	프로젝트 파일들의 상태를 보여준다

git add
	스테이징 영역에 추가
	주로 git add . 으로 전부 스테이징 시킨다

git rm {파일이름}
	파일을 지우거나 스테이지에서 해제 할때 사용합니다.

git restore {파일명}
	unstaged 상태의 변경 파일을 복원 해주는 역할

git restore --staged {파일명}
	staging된 파일을 unstaged상태로 되도림

git clean
	추적되지 않은 상태의 파일을 삭제합니다
``` cmd
# 디렉토리를 제외한 파일만 삭제
git clean -f 

# 디렉토리포함 삭제
git clean -f -d

# .gitignore 에 설정된 파일도 삭제
git clean -f -d -x

# 가상 실행
git clean -n
```

git commit
	변경된 내용을 저장한다 
	git commit -m "first commit"
	메세지를 추가해서 commit한다

git log
	commit목로을 보기 많은 옵션이 있어, git log -help 명령어로 확인을 해야한다.
```cmd
# branch 그래프를 추가하여 보기
git log --graph

# 모든 branch 보기
git log --all

# commit 메시지 제목만 한줄로 보기
git log --oneline
```

git show
	commit의 상세정보를 확인한다
```cmd
# 현재 branch의 가장 최근 commit 정보를 확인
git show

# 특정 commit 정보를 확인
git show [commit 해시값]

# 특정 branch의 가장 최근 commit 정보를 확인
git show [branch 명]
```

git remote
	원경저장소 관리
```cmd
# 설정된 원격 저장소 보기
git remote -v

# test 라는 이름으로 원격 저장소 추가하기
git remote add test https://github.com/test/test
```

git push
	원격 저장소에 코드 변경을 업로드
```cmd
# 기본 사용법
git push [저장소명] [branch]

# 최초 1회 저장소, branch 지정. 이 후, 생략 가능
git push -u [저장소명] [branch]

# 로컬에서 생성한 branch를 push
git push --set-upstream [저장소명] [branch]
```

git branch
	branch관련 명령어
```cmd
# 로컬 branch 목록 확인
git branch

# 원격 저장소를 포함한 모든 branch 목록 확인
git branch -a

# test 라는 branch 생성하기
git branch test

# test 로컬 branch를 origin이라는 원격 저장소의 test branch에 연결
git branch --set-upstream-to=origin/test test

# test branch 삭제
git branch -d test

# test branch 강제 삭제
git branch -D test
```

git switch
	branch를 변경하는 명령어
```cmd
# test branch로 변경하기
git switch test

# test2 라는 branch를 새로 생성하고 test2 branch로 변경하기
git switch -c test
```

git chekcout
	branch를 변경하고 변경점을 복원하는 명령어 swithch restore를 사용하는게 좋다고 하낟.
```
# test branch로 변경하기
git checkout test

# test2 라는 branch를 새로 생성하고 test2 branch로 변경하기
git checkout -b test2

# Unstaged 상태의 파일을 원래대로 되돌림
git checkout -- [파일명]

# Unstaged 상태의 현재 경로의 모든 파일을 원래대로 되돌림
git checkout -- .
```

git pull
	원경저장소의 데이터를 가져온다

git blame
	수정이력을 확인할수있다
```cmd
# test.txt 파일의 수정 이력을 확인
git blame test.txt

# test.txt 파일의 5부터 10번 라인까지만 확인
git blame -L 5,10 test.txt

# 파일명이 변경 되었다면, 변경전의 파일명과 함께 확인
git blame -C newTest.txt

# 공백 변경을 무시
git blame -w test.txt
```

git diff
	소스를 비교하여 볼 수 있다.
```cmd
# 마지막으로 커밋된 소스와 현재 Unstaged 상태의 변경점과 비교
git diff

# 마지막으로 커밋된 소스와 현재 Staging 된 변경점과 비교
git diff --staged

# 커밋간 비교
git diff [커밋해시1]..[커밋해시2]

# branch간 비교
git diff [branch1] [branch2]
```

git revert
	지정한 커밋으로 되돌려 커밋한다.
```cmd
# 특정 커밋으로 되돌리고 커밋
git revert [커밋해시]

# 특정 태그로 되돌리고 커밋
git revert [태그명]

# 특정 커밋으로 되돌리지만 커밋은 안한채로 Staging 상태
git revert [커밋해시] -n

# 병합한 커밋으로 되돌릴 때 메인이 되는 커밋을 지정하여 되돌리기
git revert [커밋해시] -m 1
```

git merge
	브랜치 소스와 병합
```cmd
# master branch를 병합
git merge master

# 병합 충돌(Conflict) 발생 시 취소
git merge --abort

# 공백으로 인한 병합 충돌을 무시하고 병합
git merge -Xignore-all-space
```
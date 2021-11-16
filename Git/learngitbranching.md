# Git 명령 배우기

### 11/ 13

- git rebase "대상" : 현재 브랜치에서 대상 브랜치로 base를 변경한다. 현재 브랜치의 커밋 내용을 대상브랜치에 복사해서 붙인다.
- git rebase A B : A로 B를 옮긴다.
- git checkout을 통해 HEAD를 옮길 수 있다.
- branch가 아닌 commit도 HEAD로 가리킬 수 있다. git checkout을 이용하여 commit의 해시를 가리키면 된다.
- 하지만 해시를 가리키는 것은 불편하다. 여기에서 Git의 상대 참조(Relative Ref)를 이용한다.
    - 한번에 한 커밋 위로 움직이는 캐럿 연산자 ^
       - main^ : main의 부모와 같은 의미
       - main^^ : main의 조부모 같은 의미
       - HEAD^를 통해 참조(HEAD)의 참조도 가능하다.
    - 한번에 여러 커밋 위로 올라가는 틸트 연산자 ~\<num\>
       - git checkout HEAD~2 : 현재 HEAD가 가리키는 커밋의 2단계 전을 가리킨다.
    - 상대 참조를 사용하는 일반적인 방법은 브랜치를 옮길 때이다.
    - -f 옵션을 통해 브랜치를 특정 커밋에 직접적으로 재지정 할 수 있다.
       - git branch -f main HEAD~3 : (강제로) main 브랜치를 HEAD에서 3번 뒤로 옮긴다. 즉 main 브랜치에서 가리키고 있던 커밋을 HEAD에서 3번 전 커밋을 가리키도록 한다.
       - 아래서 c4를 가리키고 있던 main이 c1으로 옮겨짐을 확인할 수 있다.
       - 해당 위치에 새로운 브랜치 이름을 작성하는 것도 가능하다.
       - HEAD~ : 1칸 올리기, HEAD~x : x번 올리기, HEAD^2 : 부모 브랜치 중 2번째 부모 선택
       - HEAD~^2~2 : 현재 head에서 위로 1칸, 2번째 부모 선택 후 위로 2칸인 커밋 선택하기
       - git branch -f main C3 : main을 C3를 가리키도록 변경

<p align="center">
<img src="https://user-images.githubusercontent.com/62360009/141611204-428ac1ce-9975-4ac3-96b5-aefd924c7329.png"> </center>
</p> 

<br><br>
- Git에서 작업 되돌리기
   - 낮은 수준의 일(개발 파일이나 묶음을 스테이징 하는 것), 높은 수준의 일(실제 변경이 복구되는 방법)이 있다. 
   - 높은 수준에 집중해서 알아본다.
   - git reset, git revert를 통해 변경 내용을 되돌릴 수 있다.
       - git reset : 브랜치로 하여금 예전의 커밋을 가리키도록 이동시키는 방식으로 되돌린다. 즉 히스토리를 고쳐쓴다라고 볼 수 있다. git reset HEAD~1과 같이 사용한다.(돌아가고 싶은 커밋을 직접 입력해도 된다)
       - git reset <option> <돌아가고 싶은 커밋> 형태이며, --hard 옵션으로 돌아가려는 이력 이후의 모든 내용을 지울 수도 있다.
       - git revert : 각자의 컴퓨터 로컬 브랜치에서는 reset을 사용할 수 있지만 히스토리를 고쳐쓴다는 점 때문에 리모트 브랜치에는 쓸 수 없다. 변경분을 되돌리고, 다른사람과 공유하기 위해 revert를 사용한다.
       - git revert [commit ID] : 돌아가고자 하는 commit ID를 적어준다. 즉 특정 커밋을 되돌리는 작업도 하나의 커밋으로 간주하여 커밋 히스토리에 추가하는 방식이다! 파일은 돌아간 시점으로 변경된다.
  
  
- 원격 브랜치(remote branch) 활용하기
   - <remote name>/<branch name> : origin/main
   - main commit -> o/main 체크아웃 -> 커밋
   - git fetch : git의 원격 작업들은 결국 서로 다른 저장소에서 주고 받는 것에 불과하다. 원격 저장소에서 데이터를 가져오기 위한 명령어이다. 원격 저장소에만 있는 파일을 로컬에 적용한다.
   - git pull : git fetch + merge
   - 원격 저장소에 가짜 만들기 : git fakeTeamwork
   - git commit --amend : 커밋 내용 변경
   - git rebase -i HEAD~2: 현 위치 포함 두 개를, 두 개 위만큼을 base로 새로운 branch를 형성한다.
   - git cherry-pick: 현 위치에서 원하는 노드를 순서대로 붙여 새로운 branch를 만든다.
   - git commit --amend : 현 위치의 커밋을 메시지, 내용 수정해서 복사된 커밋을 만들 수 있다.
   - git push origin main : main(로컬)에 있는 커밋(원격에 반영 안된)을 origin(원격) main 브랜치로 가서 push 한다. 현재 HEAD위치에 상관 없이 push가 발생한다.
   - git fetch origin main : main(로컬)로 origin(원격) 내용을 fetch 한다.
   - git fetch origin source:destination : origin(원격)의 source에 있는 커밋을 local의 destination branch

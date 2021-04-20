---
title: "Git : 명령어"
excerpt_separator: <!--more-->
categories:
  - Git
tags:
  - Git
  - "Git Command"
toc: true
toc_sticky: true
toc_label: 목차
---

## Git 명령어

**git + 명령어 형태로 이루어져있습니다.**

[git 공식홈페이지 Reference](https://git-scm.com/docs)에 가면 모든 명령어를 확인해 볼 수 있습니다.

---

##### Ctrl + l (Mac은 Ctrl + K)

​	콘솔창을 깨끗하게 비워줍니다.

---

##### git config

- -h

  명령어 다음에 --h를 입력하면 간단하게 정보확인이 가능합니다.(자세히 보려면 Reference로)

- --list

  git을 설치하게되면 git에 관련된 모든 환경설정이 .gitconfig라는 파일안에 저장이 되는데 이파일을 터미널에서 간단하게 확인할 수 있습니다.
  
- --global

  - -e

    ​	.gtconfig라는 파일을 터미널에서 vi로 열어봅니다.

    ​	**유용한 Tip **

    ​		사용자 설정 [user]밑에 push와 pull을 추가해 줍니다.

    ```
    [user]
    		...
    [push]	
    		default = current 
    # 로컬에 있는 브랜치 이름이 항상 리무트와 동일하다고 설정해주어서 push를 할 때
    # 일일이 `git push --set-upstream origin master` 옵션을 작성하지 않아도 됩니다.
    [pull]	
    		rebase = true	
    # merge와 rebase 옵션을 선택해서 동작할 수 있습니다.
    ```

  - core.editor "code [--wait]"

    ​	--wait를 붙이게 되면 editor를 종료하기 전 까지 터미널을 이용할 수 없습니다.

    ​	code를 결합하여 사용할 수 있습니다.

  - user.name "splin"

    ​	유저이름을 설정해 줄 수 있습니다.

  - user.email "dev.splin@gmail.com"

    ​	유저 이메일을 설정해 줄 수 있습니다.

  - core.autocrlf true(Mac에선 put)
    ​	window에서 true로 설정하게 되면 git에 저장할때 \r를 삭제하게 되고 다시 git에서 window로 가져올 때는 \r이 자동으로 붙게 합니다.

    ​	mac에서 input으로 설정하게되면 git에서 가져올때 별다른 수정이 일어나지 않지만 저장할때는 \r를 삭제하게 됩니다.

    ​	[core.autocrlf에 대한 자세한 설명](https://dev-splin.github.io/git/Git-core.autocrlf/ )

  - alias.st(원하는 단어) status(명령어)

    ​	원하는 명령어(status)를 원하는 이름(st)으로 간단하게 사용할 수 있습니다.

---

##### code .

​	터미널에서 연결하는 것이 불편하면 쓰기 편한 text editor를 연결해서 사용할 수 있습니다.

​	터미널에 (code)라는 명령어를 통해서 editor를 열고싶다면 운영체제에 맞게 설정할 수 있는 방법이 달라집니다. 

---

##### git init

​	git이 초기화되고 기본적으로 master branch가 생성이 됩니다. (현재 폴더에 숨겨진파일 .git 파일이 생깁니다.)

- rm -rf .git

  ​	.git을 삭제해주면 더이상 git 프로젝트가 아니게 됩니다.

---

##### git status

​	git의 상태를 볼 수 있습니다.

- -s 

  ​	조금 더 간단하게 볼 수 있습니다.

---

##### git add

​	working directory 에서 staging area로 파일을 옮길 수 있습니다.

​	확장자에 따라 `*.확장자` 로 특정한 확장자를 가진 파일만 추가할 수 있습니다.

---

##### git rm

​	working directory 와 staging area에 있는 파일을 삭제해 줍니다.

​	그냥 rm은 working directory 에서 삭제하는 것이기 때문에 staging area에 삭제를 적용하려면 add를 해야합니다.

- --cached

  ​	staging area에 있는 파일들을 wokking directory로 옮길 수 있습니다. (untracked 상태가 됨)

---

##### git mv

​	working directory 와 staging area에 있는 파일의 이름을 변경해줍니다.

​	그냥 mv를 사용할시 rm과 마찬가지로 적용됩니다.

---

##### git clean

- -fd

  ​	모든 untracked 파일을 삭제합니다.

---

##### echo *.log > .gitignore

​	.gitignore 안에 있는 확장자를 가진 파일들이나 특정 디렉토리 안에 있는 파일들을 add 하지 않게 합니다. (Untracked에 나오지 않게 함)

---

##### git diff

​	working directory에 있는 수정한 파일의 내용과 정보들을 보여줍니다.

- master(branch이름)..test(branch이름)

  ​	master branch와 test branch 사이의 정보들을 확인할 수 있습니다.

- --staged / --cached 

  ​	staging area에 있는 파일들을 보여줍니다.

- b8e485 c38c4c414

  ​	두가지의 hashcode에 해당하는 commit을 서로 비교해줍니다.

  - light_theme.txt(원하는 파일이름)

    ​	두가지의 hashcode commint 중에서 light_theme.txt(원하는 파일이름) 파일만 비교해줍니다.	

    ```
    diff --git a/c.txt b/c.txt		# git 커맨드를 이용해서 a(이전버전)과 b(지금버전)을 비교
    index 12a8798..893b969 100644	# 파일 참고할 때 쓰이는 인덱스
    --- a/c.txt
    +++ b/c.txt
    @@ -1 +1,2 @@					# -는 이전버전을 +는 지금버전
    								# -1 > 이전버전 첫번째 줄을 참고
                                    # +1,2 > 지금버전 첫번째 줄에서 두번째까지 참고해라 라는 뜻 입니다.
    hello world!
    +add 							# add라는 단어가 추가되었습니다.
    ```

- <u>`git config --global -e` 를 이용해 .config 파일을 열어서 diff를 할 때 editor로 열 수 있습니다.</u>

  ``` 
  [diff]
          tool = vscode
  # diff 할 때 사용하는 tool은 vscode를 이용합니다.
  [difftool "vscode"]
          cmd = code --wait --diff $LOCAL $REMOTE
  # vscode를 사용할 때 명령어는 code(editor 열기)다음에 --wait(editor를 열었을 때 터미널은 대기하기) --diff를 이용해서 local과 remote를 비교한다는 의미입니다.
  ```


---

##### git show 

- 0ad2db(hashcode or tag)

  ​	해당 하는 hashcode나 tag의 commit의 내용이나 tag 내용을 정확하게 확인할 수 있습니다.

  - :user_repository.txt	

    ​	해당 하는 hashcode commit의 내용 중에 user_repository.txt파일에 해당하는 내용만 확인할 수 있습니다.

- --name-only 0ad2db(hashcode or tag)

  ​	변경된 파일의 이름만 보여줍니다.

- --name-status 0ad2db(hashcode or tag)

  ​	변경된 파일의 이름과 상태만 보여줍니다.

---

##### git commit

​	vi가 나오면 제목과 설명을 적고 저장 후 종료하면 staging area에 있는 파일들이 .git directory로 옮겨갑니다.

- -m "쓸 내용"

  ​	간단하게 commit내용을 작성 후 commit할 수 있습니다.

  ​	<u>WIP (Working In Progress) : commit에 많이 쓰이는 메시지 입니다. `아직 일이 진행중이다`라는 의미를 가지고 있고 WIP 다음에 어떤 일을 하고 있는지 작성 해야 합니다.</u>

- -a

  ​	staging are와 working directory에 있는 모든파일을 commit합니다.
  
- --amend

  ​	새로운 commit을 만들지 않고 현재 HEAD의 commit을 지금 staging area에 있는 내용으로 바꿀 수 있습니다.

  <u> `git push`를 이용해서 서버에 변경 사항을 업데이트하지 않았을 때만 이용해야 합니다!!</u>

  - -m "원하는 내용"

    ​	현재 HEAD의 commit의 메시지를 원하는내용으로 바꿀 수 있습니다.

---

##### git log

​	commit한 history를 확인할 수 있습니다.

​	HEAD는 제일최근에 commit한 버전을 가리키게 됩니다.

​	[HEAD란?](https://dev-splin.github.io/git/Git-HEAD/)

- about.txt

  ​	파일 이름이 about.txt인 commit의 로그를 볼 수 있습니다.

- HEAD (HEAD~1, HEAD~2....)

  ​	특정 HEAD까지 볼 수 있습니다. (HEAD 참조로 다양하게 사용 가능)

  ​	<u>ex) HEAD a, b, c, d가 있다고 한다면 현재 HEAD가 d라고 했을 때 git log HEAD~2 라고 하면 a,b를 볼 수 있는 것입니다.</u>

- -3(원하는숫자)

  ​	최근 commit중 3개만 볼 수 있습니다.

- -p

  ​	git diff를 이용해서 보는 것처럼 수정된 파일의 내용을 commit한 history와 함께 볼 수 있습니다.

- --author="splin"

  ​	작성한 사람이 splin인 것만 보여줍니다.

- --oneline

  ​	history를 간단하게 hashcode와, commit메세지만 보여줍니다.

- --reverse

  ​	가장 오래된 commit부터 순서대로 볼 수 있습니다.

- --before="2020-09-08"

  ​	2020-09-08 이전의 commit들만 볼 수 있습니다.

- --grep="project"

  ​	타이틀에 project 단어가 포함된 commit들만 볼 수 있습니다.

- -S "about"

  ​	commit 내용에 about이 포함된 commit들만 볼 수 있습니다.

- --graph --all

  ​	만약 fix라는 branch로 이동했을 때 log를 살펴보면 어디서부터 어떤 commit이 fix인지 알 수가 없다.

  ​	이 때, 이 명령어를 사용하면 유용하게 쓸 수 있습니다.

  ```
  // 최근 commit이 맨 위에 오기 때문에 밑에서 부터 봐야합니다.
  * d643a6e (master) Update Welcome page		# 그 이후에 master branch에 commit이 하나 더 만들어 졌습니다.
  | * c38c4c4 (HEAD -> fix) Fix light theme	# 여기서 fix라는 branch로 나눠져서 commit이 만들어 졌고
  |/
  * b8e485f Add light theme					# master branch가 여기까지 쭉 오다가
  * bd7bd28 Add About page
  * 328708d Add Welcome page
  * 0ad2dbb Add UserRepository module
  * 9186a41 Add LoginService module
  * 1563681 Initialise project				# master branch
  ```
  
- --pretty 
  
  ​	git log를 했을 때 자기가 원하는대로 출력하게 할 수 있습니다.
  
  - =oneline 
  
    ​	pretty를 붙인 oneline은 hashcode가 전부 다 나옵니다.
  
  - =format
  
    ​	자기가 원하는 방식대로 예쁘게 포멧을 만들 수 있습니다. (자세한 사항은 Reference로!!)
  
    ```
    git log --pretty=format:"%h %an %ar %s"
    # %h(hashcode), %an(저자의 이름), %ar(commit된 날짜) %s(타이틀)만 출력되게 합니다.
    ```
  - git config --global alias.원하는이름 "git log --pretty=format:"%h %an %ar %s""
  
      ​	log를 출력할 때 마다 많은 포멧들을 입력해주기는 너무 번거롭기 때문에 alias로 설정해서 간단하게 사용할 수 있습니다.

- master(branch이름)..test(barnch이름)

  ​	master branch와 test branch 사이의 commit들을 확인할 수 있습니다.

---

##### git checkout

-   fix(branch이름 or tag이름 or hashcode)

  ​	해당하는 branch나 tag로 이동하거나 hashcode에 해당하는 버전으로 이동합니다.

- -b testing v2.0.0

  ​	testing이라는 branch를 만들면서 v2.0.0으로 이동한다.	

---

##### git tag

​	모든 tag를 보여줍니다.

-  v1.0.0(원하는 문자열) 0ad2dbb(hashcode)

  ​	hashcode에 해당하는 commit에 원하는 tag를 붙여줍니다. 

  ​	`git checkout v1.0.0` 명령어를 이용해 해당하는 tag로 이동할 수 있습니다.

  ​	`git checkout -b testing v2.0.0` 명령어를 이용해 v2.0.0으로 이동하는 동시에 testing이라는 branch를 만들 수 있습니다.

- -am(annotate, message의 약자) "원하는 메세지"

  ​	tag에 관련된 정보들을 포함하고 싶을 때 사용합니다.

  ​	`git show v1.0.0(tag이름)`로 tag의 자세한 내용을 볼 수 있습니다.  (git log로 히스토리를 보면 tag의 이름만 나오고 내용은 나오지 않습니다.)

- -l "v1.0.*"

  ​	"v1.0."이 포함된 모든 tag를 보여줍니다.

- -d v1.0.0

  ​	tag를 삭제할 수 있습니다.

[tag를 사용하는 이유](https://dev-splin.github.io/git/Git-tag/)

---

##### git branch

​	지금 현재 Repository에 있는 branch들을 확인할 수 있습니다.

- --all

  ​	원격 GIthub와 같은 서버와 연결된 Repository일 경우 서버에 있는 branch들의 모든 정보를 보여줍니다.

- new-branch(만들 branch 이름)

  ​	'new-branch'라는 이름의 branch를 만들어줍니다.

- -v

  ​	간단하게 최신 commit들도 같이 확인할 수 있습니다.

- --merged

  ​	현재 branch에 merge가 된 branch들을 확인해 볼 수 있습니다.

- --no-merge

  ​	현재 branch에 merge가 되지 않은 branch들을 확인해 볼 수 있습니다.

- -d new-branch2(삭제할 branch 이름)

   new-branch2라는 barnch를 삭제할 수 있습니다.

- --move fix(변경할 branch 이름) fix-welcome(변경될 branch 이름)

  ​	`fix`라는 brach를 `fix-welcome`이라는 이름으로 변경합니다.
  
- --track (remote이름/branch이름)

   - branch를 만들 때 `remote이름/branch이름`을 upstream으로 설정합니다.

---

##### git switch

- new-branch(branch 이름)

  ​	new-branch(branch 이름)로 이동합니다.

- -C new-branch2(만들 branch 이름)

  ​	new-branch2라는 branch를 만들고 동시에 이동합니다.

---

##### git merge

- feature-a(branch 이름)

  ​	master branch에 feature-a(branch 이름) branch가 merge 됩니다. 

- --no-ff feature-c

  ​	fast-forward를 사용하지 않고 새로운 merge commit을 만들어 적용하는 three-way 방식으로 merge합니다.

- --continue

  ​	conflict을 해결하고 난 후 계속해서 merge를 실행해줍니다.

- --abort

  ​	merge를 취소합니다.

  - 만약 `.orig` 파일이 남아 있다면, `git clean -f` 를 이용해 정리해줄 수 있습니다.

<u>merge 후 `git branch -d` 명령어를 이용해 feture-a branch나 feature-c brachn(기존의 branch)를 삭제해줍니다.</u>

---

##### git rebase

- master

  ​	현재 branch를 최신 master branch 위치에 rebase 합니다.

- --onto master service(branch 이름) ui(branch 이름)

  ​	master branch에 service branch에서 파생된 ui branch를 rebase 합니다.

- -i HEAD or hashcode

  ​	지정한 HEAD 나 hashcode의 다음 commit부터 최신 commit까지를 interactive rebasing 합니다

  ​	<u>[interactive rebasing이란?](https://dev-splin.github.io/git/Git-Rebase/)</u>

- --abort

  ​	rebase를 취소합니다.

---

##### git cherry-pick

- f2b9178 (가져올 기능이 있는 hashcode)

  ​	현재 branch에 f2b9178에 해당하는 commit을 가져옵니다.

---

##### git stash

​	아무것도 붙이지 않으면 옵션 push(git push 아닙니다)와 동일한 기능을 합니다.

- push

  ​	working directory와 staging area에 있는 파일들을 Stash Stack에 넣습니다.	

- -m

  ​	타이틀을 지정해 줄 수 있습니다.

- --keep-index

  ​	staging area에 있는 것을 유지하면서 Stash Stack에 저장할 수 있습니다.

- -u

  ​	untracked 파일은 자동으로 Stash Stack에 저장되지 않지만 이 옵션을 사용하면 포함해서 저장할 수 있습니다.

- list

  ​	Stash Stack을 확인할 수 있습니다. (제일 아래에 있는 거부터 시작해서 쌓이는 것을 확인할 수 있습니다.) `stash@{0}` 은 stash의 id 같은 것이라고 보면 됩니다.

- show stash@{0}(원하는 stash id)

  ​	해당 stash의 내용을 간단하게 확인해 볼 수 있습니다.

  - -p

    ​	내용을 조금 더 자세하게 확인해 볼 수 있습니다.

- apply stash@{0}(원하는 stash id)

  ​	stash@{0}(원하는 stash id)를 working directory나 staging area에 적용합니다. apply 다음에 아무것도 입력하지 않을 시에는 Stash Stack에서 제일 위에 있는 stash를 적용합니다. 

  <u>stash 목록에서 제거되지는 않습니다.</u>

- pop stash@{0}(원하는 stash id)

  ​	stash@{0}(원하는 stash id)를 working directory나 staging area에 적용합니다. pop 다음에 아무것도 입력하지 않을 시에는 Stash Stack에서 제일 위에 있는 stash를 적용합니다. 

  <u>stash 목록에서 제거됩니다.</u>

- drop stash@{0}(원하는 stash id)

  ​	Stash Stack에서 stash@{0}(원하는 stash id)를 삭제합니다.

- clear

  ​	Stash Stack을 비워줍니다. (모두 삭제)

- branch newBranch(새로 만들 branch이름)

  ​	newBranch(새로 만들 branch 이름)가 만들어지면서 Stash Stack에서 제일 위에 있는 stash를 적용하고 newBranch로 이동합니다.

  <u>stash 목록에서 제거됩니다.</u>

---

##### git restore

- payment-ui.txt (파일 이름) or .(온점 하나 맞습니다. 모든 파일을 의미 합니다. )

  ​	working directory에 있는 paymetn-ui.txt(파일 이름) 파일을 초기화하거나,  모든 파일들을 초기화합니다.

  ​	 <u>새로 추가된 파일은 restore로 지워지지 않기 때문에 `git clean -fd`를 이용해 지워야 합니다.</u>

- --staged payment-ui.txt (파일 이름) or .

  ​	staging area에 있는 payment-ui.txt(파일 이름) 파일을 초기화하거나,  모든 파일들을 초기화합니다. (working directory로 가져옵니다.)

- --source=HEAD~2(HEAD or hashcode) payment-ui.txt

  ​	payment-ui.txt 파일을 현재 HEAD의 2단계 전 (hascode에 해당하는) commit으로 되돌립니다.

---

##### git reset

- HEAD or hashcode

  ​	아무 옵션도 없으면 default로 --mixed를 사용하는 것과 동일하게 작동합니다.

- --mixed HEAD or hashcode

  ​	특정 commit으로 초기화 하는데, 현재 HEAD의 위치와 그 사이의 commit들은 삭제하고 작업내용은 working directory에 옮겨놓습니다.

  ​	<u>예를 들어 1~5 까지의 commit이 있고 1에 HEAD가 있다고 가정할 때, `git reset HEAD~2` 를 하게 되면 3에 HEAD가 위치해 있고 1~2의 commit은 사라지고 1~2의 작업내용은 working directory에 남아있게 됩니다.</u>

- --soft HEAD or hashcode

  ​	특정 commit으로 초기화 하는데, 그 사이의 commit들은 삭제하고 작업내용은 staging area에 옮겨놓습니다.

- --hard HEAD or hashcode or reflog의 HEAD

  ​	특정 commit으로 초기화 하는데, 그 사이의 commit들과 작업내용을 삭제합니다.

  ​	reflog의 HEAD를 입력 시 그 HEAD로 이동할 수 있습니다.
  
  ​	<u>만약 `git reset --hard HEAD(현재 HEAD)`를 하게된다면 현재 HEAD의 commit 시점으로 초기화 한다는 것이므로, 커밋한 시점 부터 지금까지 작업한 내용 모두를 초기화하게 됩니다.</u>

---

##### git reflog

​	log를 참조(reference)한다는 뜻의 명령어입니다. 여태까지 실행했던 명령어들과 그것으로 인해서 변경되었던 HEAD가 가리키고 있었던 포인트까지 확인할 수 있습니다. 

​	<u>reflog를 실행해 나와있는 log의 HEAD를(HEAD@{1}) 를 가지고 `git reset --hard HEAD@{1}` 을 이용해서 그 시점으로 돌아갈 수 있습니다. (만약 commit하지 않은 상태에서 `git reset hard`를 이용해서 초기화를 했다면 다시 돌아갈 확률이 작습니다. -> 각 IDE마다 있는(없을 수도..) local history를 이용해 예전 버전으로 돌아갈 수 있다.)</u>

---

##### git revert

- hashcode or HEAD

  ​	해당 hashcode 나 HEAD의 commit에서 변경했던 모든 내용들을 다 삭제 해주는 commit을 만듭니다.

- --no-commit hashcode or HEAD

  ​	해당 hashcode 나 HEAD의 commit에서 변경했던 모든 내용들을 다 삭제 해주는데, commit을 바로 만들지 않고 staging area에 추가해줍니다.

  <u>보통은 revert하는 그 내용만 작성하기 때문에 자동으로 생성되게 하는 것이 일반적입니다.</u>

---

##### git clone

- https://github.com/dev-splin/test.git (github에서 복사해온 주소) test-project(원하는 이름)

  ​	해당하는 repository를 원격에서 받아옵니다. 

  ​	주소까지만 입력할 시 repository 이름 그대로 해당 경로에 새로운 폴더가 생성됩니다.

  ​	원하는 이름을 입력하면 repository 이름 대신 원하는 이름으로 폴더가 생성됩니다.

---

##### git remote

​	서버에 관련된 정보들을 확인할 수 있습니다. (기본적으로 server있는 이름은 origin이라고 설정되어 있습니다.)

- -v

  ​	서버에 있는 정보들을 조금 더 자세히 확인할 수 있습니다.

- add server(원하는 이름) https://github.com/dev-splin/test.git (github 에서 받아온 주소 <u>동일한 링크는 하면 안 됩니다!</u>)

  ​	오픈소스 프로젝트에 참가할 때, fork한 repository에서는 다수의 origin을 원하는 이름으로 설정할 수 있습니다.

- show origin(정보를 확인할 이름)

  ​	origin(정보를 확인할 이름)의 관련된 정보를 자세히 볼 수 있습니다.
  
- remove origin(server의 이름)

  ​	해당하는 server를 삭제합니다.

---

##### git push

​	ID와 Password를 입력한 후 commit한 것들을 server에 올릴 수 있습니다.

​	<u>git환경에서 설정된 이름과 이메일을 따라 github에 올라가기 때문에 신경써주어야 합니다.</u>

- -f

  ​	만약 서버에 a라는 파일의 변경사항이 있는데 local에서 rebase(서버에서 받아오는 것을 말합니다.)하지 않고 a라는 파일을 변경했을 때, `git push`  를 이용하면 conflict이 발생합니다. 하지만, force(f)를 사용하면 서버에 있는 변경사항과는 상관없이  local에 있는 변경사항을 적용합니다. 

  <u>웬만하면  server에 변동된 사항이 있다면 `git pull` 또는 `git fetch`를 이용해 server에 맞게 업데이트 한 다음에 push를 하는게 맞습니다. 하지만 간혹 기존의 git history를 rebase를 이용해서 변경하거나 history를 변경했다면 부득이하게 force라는 옵션을 이용해서 push를 해야 합니다.</u>

- origin

  - v1.0.0

    ​	origin이라는 이름의 server에 v1.0.0 이라는 tag를 업로드합니다.

  - --tags

    ​	origin이라는 이름의 server에 모든 tag를 업로드 합니다.

  - --delete v1.0.0(branch이름 or tag 이름)

    ​	만약 지금 Repository가 서버에도 있다면, 그래서 branch나 tag를 삭제한 것을 원격에도 업데이트하고 싶다면 git push 명령어를 이용해서 origin(원격이 있는 곳)에서 --delete(삭제) 할 수 있습니다.

  - --set-upstream origin fix-welcome

    ​	fix-welcome으로 이름이 변경된 branch를 원격에 업데이트 할 수 있습니다.

---

##### git fetch

​	내가 현재 작업하고 있는 HEAD는 그대로 유지하면서 server에 업데이트된 history 정보만 가지고 옵니다.

​	<u>server에서 어떤 일들이 지금 발생하고 있는지, 누가 어떤 일을 했는지 확인하고 싶은 경우에 많이 쓰입니다.</u>

- origin (server의 이름)

  ​	해당하는 server의 history를 가지고 옵니다.

  - master (branch의 이름)

    ​	해당하는 origin server의 master branch만 가지고 옵니다.

---

##### git pull

​	server에 있는 내용을 받아와서 나의 local 버전도 server와 함께 동일하게 만들고 싶을 때 사용합니다.

​	<u>default로 merge를 이용하기 때문에 conflict이 났을 때 conflict을 해결하면 두개의 commit을 통합한 새로운 commit이 생깁니다.</u>

- --rebase

  ​	server에 있는 내용을 받아온 후 나의 local 버전을 적용시킵니다.

  ​	<u>conflict이 났을 때 conflict을 해결하면 rebase를 이용해 local의 commit만 새로운 commit 되고 server에서 받아온 commit은 그대로 유지 되어 fast-foward 방식으로 merge할 수 있게됩니다.</u>

---

##### git blame

- fixtures/dom/src/tags.js (원하는 파일명)

  ​	해당 파일에 존재하는 코드들이 어떤 commit에 의해서 도입 되었는지 자세하게 확인해서 디버깅할 수 있습니다.

  ​	<u>다양한 IDE에서 extension(vscode의 경우는 Git Lens)을 이용해서  IDE에서 바로 확인해 볼 수도 있습니다!</u>

---

##### git bisect

​	개발tool로 디버깅하기가 힘든 경우 버그를 포함하고 있는 commit을 이진탐색알고리즘을 이용해 빠르게 찾아낼 수 있도록 도와주는 명령어입니다.

- start

  ​	bisect를 시작합니다.

- bad

  ​	문제가 있는 commit이라고 마크해둡니다.

- good

  ​	문제가 없는 commit이라고 마크해둡니다.

- reset

  ​	bisect를 다 이용했을 때, 원래 branch로 돌아갑니다.

---

출처 : https://www.youtube.com/watch?v=Z9dvM7qgN9s&t=1800s

https://academy.dream-coding.com/


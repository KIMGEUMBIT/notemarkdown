```
* Git와 GitHub의 차이
Git은 Git 리포지토리라고 불리는 데이터 저장소에 소스 코드 등을 넣어서 이용하는것으로
GitHub : 인터넷상에서 제공하는 서비스(호스팅 서비스)가
```

1일차(git 설치 및 개요 및 사용법)
---------------------------------

#### 브랜치(branch)

-	프로젝트 작업영역(개인)

```
브랜치(branch)?=>프로젝트 작업영역(개인)

완벽한 분산 환경에서 빠르고 단순하게 수백 수천 개의 동시
발적인 브랜치 작업을 수행하는 것을 목표로 하는 버전 관리 시스템

버전 관리 시스템(형상관리)

->여러명의 공통 프로젝트를 관리->시스템(공통으로 사용하는 파일,폴더)
```

#### Git의 특징

1.	로컬 및 원격 저장소 생성
2.	로컬 저장소에 파일 생성 및 추가
3.	수정 내역을 로컬 저장소에 저장
4.	파일 수정 내역 추적(변경사항을 체크하기위해)
5.	원격 저장소에 제출된 수정 내역을 로컬 저장소에 적용
6.	Master에 영향을 끼치지 않는 브랜치 생성
7.	브랜치 사이의 병합(Merge)
8.	브랜치를 병합하는 도중의 충돌 감지

---

### Git

#### git 설치

-	https://git-scm.com/

##### kitcoop@kitcoop-PC MINGW64 ~

#### 로컬저장소란?

-	각자 데이터를 따로 저장할 수 있는 영역

#### 원격저장소

-	팀원끼리 데이터를 공유
-	GitHub에 가입

#### 브랜치란

-	**공통**으로 사용이 되는 파일을 각자의 영역에서 **아무런 충돌없이 사용** 할 수 있도록 해주는 영역 - ----

#### Git 명령어

-	pwd

```
* 현재위치

$ pwd
/c/Users/kitcoop
```

-	git config -–global user.name “사용자 이름”

-	git config -–global user.email “이메일”

-	cd ..

```

```

-	**$ mkdir 디렉토리명** <br> 디렉토리(폴더) 생성

```
mkdir imsi
```

-	$git --help <br>도움말

-	**$ git init**

```
해당 영역에 나만의 사용 공간 (로컬저장소) 만드는 명령어
.git은 보이지 않는 파일
```

```
kitcoop@kitcoop-PC MINGW64 /c/imsi
$ git init
Initialized empty Git repository in C:/imsi/.git/

kitcoop@kitcoop-PC MINGW64 /c/imsi (master)

```

#### (master)

-	$git init로 로컬영역을 생성하면 <br> 브랜치 하나 생성해준다(master)

-	vi <br> vim 생성시킬 파일명 <br> 파일이 하나 생성 <br> 편집기가 생성

```
해당경로에 text.txt 파일을 생성하겠다.
vim test.txt
```

-	vim test.txt ==> 해당 파일 생성<br> i ==> 파일에 내용 추가하고<br> 커서가 이동 <br> 글자을 입력<br> esc + :wq ==> 파일저장하고 해당 파일 종료 다시 화면으로 빠져나온다

-	$ cat test.txt ==> 파일 내용 확인

```
kitcoop@kitcoop-PC MINGW64 /c/imsi (master)
$ cat test.txt
print("hello world")
```

-	**$ git add(새로 추가된 경우에만 한번만 지정)<br> \$ git commit(수정시 매번 해야함.)** <br> 특정파일 내용을 추가, 수정 할 경우 반드시 위 2개의 명령어를 사용하애햔다. <br> 특정 다른 영역으로 이동안된다(checkout)

-	**$ git commit -a** <br> 모든 파일 commit ?????

-	**$ git add** 명령어 <br> git에게 알려주는 역할, 변동되어 수정된 내용을 기록

-	**$ git status** <br> 현재 상태를 확인(commit을 이미 한 상태)할 수 있고 commmit을 해야 된다고 알려준다

##### 커밋 안했을 경우 이렇게 하여라

1.	각자 작업한 파일의 내용을 기록하여라(git)

```
$ git add  test.txt
warning: LF will be replaced by CRLF in test.txt.
The file will have its original line endings in your working directory.
```

> 리눅스 환경에서 파일이 저장되어서 윈도우형태로 바꾸어줬다능... 경고메세지가 뜬 것이다. 저장에는 문제 없다

1.	$ git commit => $ i => 아무거나 입력 => $ git

> 파일에다가 커밋메세지를 작성하면 해당 파일로 이동

-	$ clear<br> 화면지우기
-	**$ git branch** <br> 브랜치 목록을 확인

```
kitcoop@kitcoop-PC MINGW64 /c/imsi (master)
$ git branch
* master   //현재 작업중인 브랜치명 앞에 *이 표시
```

-	**$ git branch 명칭** <br> 새로운 branch 생성

```
kitcoop@kitcoop-PC MINGW64 /c/imsi (master)
$ git branch hotfix

kitcoop@kitcoop-PC MINGW64 /c/imsi (master)
$ git branch
  hotfix
* master
```

-	**$ git checkout 이동 할 브랜치명**

```
kitcoop@kitcoop-PC MINGW64 /c/imsi (master)
$ git checkout hotfix
Switched to branch 'hotfix'
A       test2.txt
```

-	ls<br> 파일list 목록

```
kitcoop@kitcoop-PC MINGW64 /c/imsi (hotfix)
$ ls
test.txt  test2.txt
```

-	add output "Tell your world"

-	git merge 병합할 브랜치명

---

### 결론 : 하나이상의 파일을 공유해서 작업 가능

---

##### 두 개의 브랜치에서 동시에 같은 파일의 같은 곳을 수정하고, 그걸 병합하면서 생기는 충돌을 해결

![1](http://i.imgur.com/nEbA0R1.png)

![2](http://i.imgur.com/1QbnlFo.png)

---

정리 ★★★★★
----------

1.git init->로컬 저장소를 생성시키는 명령어(c:\imsi)->master 2.git add 수정한 파일명==>이 파일에 변동사항을 브랜치별로 기록 test.txt 3.git commit->파일을 수정할때 마다 최종적으로 commit메세지저장

4.git branch->현재 브랜치 목록을 확인 5.git branch 생성시킬 브랜치명(hotfix) 6.git checkout 이동할 브랜치명 7.git merge 병합할 브랜치명

```
git merge hotfix

kitcoop@kitcoop-PC MINGW64 /c/imsi (master)
$ git merge hotfix
Updating 4869169..35358e2
Fast-forward
 test.txt | 1 +
 1 file changed, 1 insertion(+)

kitcoop@kitcoop-PC MINGW64 /c/imsi (master)
$ cat test.txt
print("hello world")
print("Tell your World")

kitcoop@kitcoop-PC MINGW64 /c/imsi (master)
```

---

```
## branch의 모델

### 메인 브랜치(Main branch)

- 'master' 브랜치와 'develop' 브랜치, 이 두 종류의 브랜치를 보통 메인 브랜치로 사용합니다.

#### 마스터 브랜치(master)
마스터 브랜치에서는, 배포 가능한 상태만을 관리합니다. 커밋할 때에는 태그를 사용하여 배포 번호를 기록합니다.


#### 디벨롭 브랜치(develop)

디벨롭 브랜치는 앞서 설명한 통합 브랜치의 역할을 하며, 평소에는 이 브랜치를 기반으로 개발을 진행합니다.

#### 피처 브랜치(Feature branch)

피처 브랜치는, 앞서 설명한 토픽 브랜치 역할을 담당합니다.

이 브랜치는 새로운 기능 개발 및 버그 수정이 필요할 때에 'develop' 브랜치로부터 분기합니다. 피처 브랜치에서의 작업은 기본적으로 공유할 필요가 없기 때문에, 원격으로는 관리하지 않습니다. 개발이 완료되면 'develop' 브랜치로 병합하여 다른 사람들과 공유합니다.

#### 릴리즈 브랜치(Release branch)

릴리즈 브랜치에서는 버그를 수정하거나 새로운 기능을 포함한 상태로 모든 기능이 정상적으로 동작하는지 확인합니다. 릴리즈 브랜치의 이름은 관례적으로 브랜치 이름 앞에 'release-' 를 붙입니다. 이 때, 다음 번 릴리즈를 위한 개발 작업은 'develop' 브랜치 에서 계속 진행해 나가면 됩니다.

릴리즈 브랜치에서는 릴리즈를 위한 최종적인 버그 수정 등의 개발을 수행합니다. 모든 준비를 마치고 배포 가능한 상태가 되면 'master' 브랜치로 병합시키고, 병합한 커밋에 릴리즈 번호 태그를 추가합니다.

릴리즈 브랜치에서 기능을 점검하며 발견한 버그 수정 사항은 'develop' 브랜치에도 적용해 주어야 합니다. 그러므로 배포 완료 후 'develop' 브랜치에 대해서도 병합 작업을 수행합니다.

#### 핫픽스 브랜치(Hotfix branch)

배포한 버전에 긴급하게 수정을 해야 할 필요가 있을 경우, 'master' 브랜치에서 분기하는 브랜치입니다. 관례적으로 브랜치 이름 앞에 'hotfix-'를 붙입니다.

예를 들어 'develop' 브랜치에서 개발을 한창 진행하고 있는 도중에 이전에 배포한 소스코드에 아주 큰 버그가 발견되는 경우를 생각해 보세요. 문제가 되는 부분을 빠르게 수정해서 안정적으로 다시 배포해야 하는 상황입니다. 'develop' 브랜치에서 문제가 되는 부분을 수정하여 배포 가능한 버전을 만들기에는 시간도 많이 소요되고 안정성을 보장하기도 어렵습니다. 그렇기 때문에 바로 배포가 가능한 'master' 브랜치에서 직접 브랜치를 만들어 필요한 부분 만을 수정한 후 다시 'master'브랜치에 병합하여 이를 배포하려고 하는 것이죠.

이 때 만든 핫픽스 브랜치에서의 변경 사항은 'develop' 브랜치에도 병합하여 문제가 되는 부분을 처리해 주어야 합니다.
```

---

#### DB

-	test.exerd

##### 테이블 생성

-	3 -> 원하는 위치 클릭
-	ctrl+shift+enter

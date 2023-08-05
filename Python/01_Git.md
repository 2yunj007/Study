# Git

: 분산 버전 관리 시스템



## 버전 관리

- 내가 작업한 행위(변경 사항)를 기록하고 추적
- 전체가 아닌 이전 버전 이후의 변경 사항만 기록
- Git은 효율을 위해 마지막 파일과 이전 변경 사항만 남김



## 중앙 vs. 분산

### 중앙 집중식

- 버전은 중앙 서버에 저장되고 중앙 서버에서 파일을 가져와 다시 중앙에 업로드

### 분산식

- 버전을 여러 개의 복제된 저장소에 저장 및 관리
- 중앙 서버에 의존하지 않고 동시 작업 가능
- 중앙 서버의 장애나 손실에 대비하여 백업 및 복구 용이
- 인터넷에 연결되지 않아도 작업 가능(변경 이력과 코드를 로컬 장소에 저장하고, 나중에 중앙 서버와 동기화)



## Git의 3가지 영역

### Working Directory

: 실제 작업 중인 파일들이 위치하는 영역

### Staging Area

: Working Directory에서 변경된 파일 중, 다음 버전에 포함시킬 파일들을 선택적으로 추가하거나 제외할 수 있는 중간 준비 영역

- 눈에 보이지 않고 명령어로만 확인 가능

### Repository

- 버전 이력과 파일들이 영구적으로 저장되는 영역으로, 모든 버전과 변경 이력이 기록됨
- Working Directory에서 중간 단계 없이 Repository로 넘어가는 것은 불가능
- Commit(버전): 변경된 파일들을 저장하는 행위 / snapshot이라고도 함



## git의 동작

### git init

: 로컬 저장소 설정(초기화)

- git 버전 관리 시작을 의미
- git의 버전 관리를 시작할 디렉토리에서 진행
- git의 관리를 받기 시작한 디렉토리 내에서는 (master) 표시됨

### git init 주의사항

- git 로컬 저장소 내에 또 다른 git 로컬 저장소를 만들지 말 것
- 즉, 이미 git 로컬 저장소인 디렉토리 내부 하단에서 git init 명령어를 다시 입력하지 말 것
- 가장 바깥쪽의  git 저장소가 안쪽의 git 저장소의 변경 사항을 추적할 수 없기 때문

### git add

- 변경 사항이 있는 파일을 staging area에 추가
- 'touch a.txt'를 통해 파일을 생성하면 Staging area를 거치지 않기 때문에 Untracked(추적 불가) 됨
- track 되게 하기 위해 'git add <file>' 입력

### git commit

- Staging area에 있는 파일들을 저장소에 기록
- 해당 시점의 버전을 생성하고 변경 이력을 남기는 것
- git init과 다르게, 최초 1회만 하면 됨

```
SSAFY@DESKTOP-MDJG1NJ MINGW64 ~/Desktop/새 폴더 (master)
$ git commit -m "first commit"
Author identity unknown

*** Please tell me who you are.

Run

  git config --global user.email "you@example.com"
  git config --global user.name "Your Name"
```
(commit을 생성하기 위해서는 작성자 정보 필요)
```
SSAFY@DESKTOP-MDJG1NJ MINGW64 ~/Desktop/새 폴더 (master)
$ touch c.txt d.txt e.txt

SSAFY@DESKTOP-MDJG1NJ MINGW64 ~/Desktop/새 폴더 (master)
$ mkdir sample

SSAFY@DESKTOP-MDJG1NJ MINGW64 ~/Desktop/새 폴더 (master)
$ git status
On branch master
Untracked files:
  (use "git add <file>..." to include in what will be committed)
        c.txt
        d.txt
        e.txt

nothing added to commit but untracked files present (use "git add" to track)
```
(비어 있는 디렉토리는 변경 사항으로 취급하지 않음)
```
SSAFY@DESKTOP-MDJG1NJ MINGW64 ~/Desktop/새 폴더 (master)
$ git add .

SSAFY@DESKTOP-MDJG1NJ MINGW64 ~/Desktop/새 폴더 (master)
$ git status
On branch master
Changes to be committed:
  (use "git restore --staged <file>..." to unstage)
        new file:   c.txt
        new file:   d.txt
        new file:   e.txt
```
(git add 할 때 파일명을 다 입력하지 않고 현재 상태를 나타내는 <.> 입력)

### git remote

- 로컬 저장소에 원격 저장소 주소 추가
- 등록을 하는 원격 저장소 개수는 상관없음
- origin: 추가하는 원격 저장소 별칭

```
$ git remote add origin remote_repo_url
```

### git log

- commit history 보기

### git log --oneline

- commit 목록 한 줄로 보기

### git config --global -l

- git global 설정 정보 보기

### git push

- 원격 저장소에 commit 목록을 업로드
- "git아, push 해 줘. origin이라는 이름의 원격 저장소에 master 브랜치를"
- 원격 저장소가 하나일 경우 두 번째부터는 'git push'만 입력해도 됨
- '-u'는 생략해도 됨

```
$ git push -u origin master
```

### git pull

- 원격 저장소 전체를 복제 (다운로드)
- clone으로 받은 프로젝트는 이미 git init 되어 있음
- 원격 저장소의 변경 사항만을 받아옴 (업데이트) 

```
$ git pull origin master
```

### git 사용 예시

```
SSAFY@DESKTOP-MDJG1NJ MINGW64 ~/Desktop/git-practice (master)
$ git remote set-url origin https://github.com/2yunj007/git-practice

SSAFY@DESKTOP-MDJG1NJ MINGW64 ~/Desktop/git-practice (master)
$ git remote -v
origin  https://github.com/2yunj007/git-practice (fetch)
origin  https://github.com/2yunj007/git-practice (push)

SSAFY@DESKTOP-MDJG1NJ MINGW64 ~/Desktop/git-practice (master)
$ git push origin master
info: please complete authentication in your browser...
Enumerating objects: 3, done.
Counting objects: 100% (3/3), done.
Writing objects: 100% (3/3), 208 bytes | 208.00 KiB/s, done.
Total 3 (delta 0), reused 0 (delta 0), pack-reused 0
To https://github.com/2yunj007/git-practice
 * [new branch]      master -> master

SSAFY@DESKTOP-MDJG1NJ MINGW64 ~/Desktop/git-practice (master)
$ git status
On branch master
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
        modified:   README.md

no changes added to commit (use "git add" and/or "git commit -a")

SSAFY@DESKTOP-MDJG1NJ MINGW64 ~/Desktop/git-practice (master)
$ git add .

SSAFY@DESKTOP-MDJG1NJ MINGW64 ~/Desktop/git-practice (master)
$ git commit -m "first commit"
[master 34cedc3] first commit
 1 file changed, 1 insertion(+)

SSAFY@DESKTOP-MDJG1NJ MINGW64 ~/Desktop/git-practice (master)
$ git push origin master
Enumerating objects: 5, done.
Counting objects: 100% (5/5), done.
Writing objects: 100% (3/3), 261 bytes | 261.00 KiB/s, done.
Total 3 (delta 0), reused 0 (delta 0), pack-reused 0
To https://github.com/2yunj007/git-practice
   0cfc533..34cedc3  master -> master
$git clone remote_repo_url
```



## 로컬

: 현재 사용자가 직접 접속하고 있는 기기 또는 시스템으로, 개인 컴퓨터, 노트북 등 사용자가 직접 조작하는 환경



## 원격 저장소

: 코드와 버전 관리 이력을 온라인 상의 특정 위치에 저장하여 여러 개발자가 협업하고 코드를 공유할 수 있는 저장 공간



# Gitignore

: Git에서 특정 파일이나 디렉토리를 추적하지 않도록 설정하는 데 사용되는 텍스트 파일

- 프로젝트에 따라 공유하지 않아야 하는 것들도 존재하기 때문
- .gitignore 파일 생성 (파일명 앞에 '.' 입력, 확장자 없음)
- README와 같은 최상위에 위치
- 이미 git의 관리를 받은 파일이나 디렉토리는 나중에 gitignore에 작성해도 적용되지 않음
- gitignore.io: 운영체제, 프레임워크, 프로그래밍 언어 등 개발 환경에 다라 gitignore 목록을 만들어 주는 사이트

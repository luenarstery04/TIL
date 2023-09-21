# Git Basic Command

## 1. 초기 설정
git을 사용하기 위해 bash 설정까지 죄다 마쳤다고 가정해보자. bash를 통해 폴더든 파일이든 mkdir, touch로 만들 순 있지만 GUI 환경이 익숙하다면 그냥 GUI로 만들어도 무방하다. ~~솔직히 취향껏 쓰면 된다.~~  

내가 git을 설치할 폴더를 선택하고 콘솔이든 터미널이든 아래와 같이 명령어를 친다.  

```bash
git init
```

이제 git이 내 폴더를 관리하기 시작한다. 보통 처음엔 master branch에서 시작할텐데 요즘은 main이라고들 쓴다고 한다. 초기 설치 단계에서 main으로 바꿔주는 옵션이 있는데, 까먹은 분들은 아래와 같이 하면 된다.

```bash
git config --global init.defaultbranch main

또는

git branch -M main
```

그래도 master라고 쓰실 분들은 그대로 쓰면 될 것이다.

git에 commit으로 기여하면 내 이름과 email이 노출된다. 얘는 어떻게 설정하느냐? 아래와 같다.

```bash
git config --global user.email "사용할 이메일"
git config --global user.name "사용할 이름"
```
이러면 누가 commit했고 email은 뭔지 알 수 있다. 따옴표든 쌍따옴표든 "" 안에 들어가면 아무튼 등록된다.  
코드가 좀 엉망이다 싶으면 연락해서 속을 긁어보자.

## 2. 파일 최초 등록하기

파일을 git이 관리하는 폴더 안에 새로 만들고 수정하고 저장해도 별 일 일어나지 않는다. 아직 **git에 add가 이뤄지지 않아 git의 관심 밖에 있는 것이다.** git에게 *야 내 파일 좀 관리해줘* 라고 하고 싶다면 파일을 추가해줘야 한다.

'a.txt, b.py, c.js, index.html' 파일 4개를 새로 만들었다고 치자. add를 통해 파일을 추가해주려면 아래와 같이 쓰면 된다.

```bash
git add a.txt
```
여러 파일을 추가하고 싶다면 이렇게 쓴다. '스페이스'로 파일끼리 구분한다.  
만약 파일명이 'NewAwesome_homepage_index.html'처럼 너무 길다면 앞쪽 NewAw... 까지만 입력하고 **Tab** 키를 눌러준다. 자동완성된다.
```bash
git add a.txt b.py c.js index.html
```
하나하나 추가하기 귀찮다면 **.** 을 쓰면 된다. 그럼 폴더 내 모든 파일이 다 추가된다.
```bash
git add .
```
파일을 commit 해준다. commit 할 땐 그냥 해서는 안되고 -m '대충 메세지'를 적어넣는다. 한글도 잘 된다.  

그럼 git이 내 파일을 관리하기 시작한다.
```bash
git commit -m '커밋 메세지'
```
## 3. 파일 수정 후 commit하기
코딩을 열심히 했다. 이제 얘를 저장하고 싶다.  
git add를 하기 전에 VScode든 뭐든 일단 **Ctrl + S** 눌러서 저장부터 해주자. 저장하기 전에 add, commit 하면 아무 의미가 없다. ~~내가 그런 실수를 했기 때문이다~~

저장했다면 VScode 기준 파일 색상이 <span style="color:#E2C08D">누르딩딩</span>하게 변했을 것이다.

<span style="color:#E2C08D">누르딩딩</span>한 글자 옆에 나타난 M에 마우스를 올려보면 Modified라고 되어있을 것이다. 이 상태에서 git status를 찍어보자.

```bash
$ git status
On branch master
Your branch is up to date with 'origin/master'.

Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
        modified:   Git/basic-command.md

no changes added to commit (use "git add" and/or "git commit -a")
```
막 줄 <span style="color:#D7594A; font-weight: bold">modified:   Git/basic-command.md</span>이 빨간색으로 되어있다면 아직 stage되지 않은 상태란 뜻이다. stage 여부는 VScode 기준 왼쪽 세 번째 탭인 Source Control에서 볼 수 있다.

파일을 add하면 드디어 글씨가 <span style="color:#0DBC79; font-weight: bold">초록색</span>으로 바뀌었다. commit 준비가 된 것이다. 위에서 배운 commit 명령어로 버전을 만들자.

## 4. 버전 관리하기
commit을 꽤나 많이 했다면 중간중간 로그를 확인하는 습관을 가져보자. git log를 입력하면 이제까지 내가 commit한 히스토리를 쫙 볼 수 있다.

```bash
git log

commit 아이디 어쩌구 (HEAD -> main, origin/main)
Author: 내 닉넴 <내 이메일>
Date: 커밋한 날짜
```
참고로 빠져나가는 키는 'q'키이다. quit의 약자란건 유추했겠지  
보기 너무 길다 싶으면 --oneline으로 함축하는 것도 가능하다.

```bash
git log --oneline
```
이러면 보기 좋게 commit id랑 메세지만 보여준다.

간혹 내가 잘못 commit해버렸는데 commit은 지워버릴 순 없지만 과거 내역으로 돌아갈 수는 있다. git checkout '돌아갈 id'를 입력하면 해당 작업내역으로 돌아갈 수 있다. --oneline을 통해 튀어나온 id를 입력하면 된다.

```bash
git checkout b6577d1
```
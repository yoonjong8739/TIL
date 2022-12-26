# Github

> 원격 저장소
- 여태 까지는 내 컴퓨터라는 한정된 공간에 있는 로컬 저장소에서만 버전 관리를 진행했습니다.
이제는 Github의 원격 저장소를 이용해 내 컴퓨터의 로컬 저장소를 다른 사람과 `공유`해봅시다.
Git의 주요 목적 중 하나인 `협업`을 위해 로컬 저장소와 원격 저장소의 연동 방법을 학습합니다.

1. Github에서 원격 저장소 생성
2. 로컬 저장소와 원격 저장소 연결
    1. 원격 저장소가 잘 생성되었는지 확인 후, 저장소의 주소를 복사합니다.
    2. 기존에 실습 때 만들었던 홈 디렉토리의 TIL 폴더로 가서 vscode를 엽니다.
    3. git init을 통해 TIL 폴더를 로컬 저장소로 만들어줍니다.
        ```bash
        kyle@KYLE MINGW64 ~/TIL
        $ git init
        Initialized empty Git repository in C:/Users/kyle/TIL/.git/
        ```
    4. git remote : 로컬 저장소에 원격 저장소를 등록, 조회, 삭제할 수 있는 명령어
       1. **등록**
          - `git remote add <이름> <주소>` 형식으로 작성합니다.
            ```bash
            $ git remote add origin https://github.com/edukyle/TIL.git

            [풀이]
            git 명령어를 작성할건데, remote(원격 저장소)에 add(추가) 한다.
            origin이라는 이름으로 https://github.com/edukyle/TIL.git라는 주소의 원격 저장소를
            ```
       2. **조회**
          - `git remote -v` 로 작성합니다.
            ```bash
            $ git remote -v
            origin  https://github.com/edukyle/TIL.git (fetch)
            origin  https://github.com/edukyle/TIL.git (push)


            add를 이용해 추가했던 원격 저장소의 이름과 주소가 출력됩니다.
            ```
       3. **삭제**
          - `git remote rm <이름>` 혹은 `git remote remove <이름>` 으로 작성합니다.
    
            > 로컬과 원격 저장소의 연결을 끊는 것이지, 원격 저장소 자체를 삭제하는 게 아닙니다.
            ```bash
            $ git remote rm origin
            $ git remote remove origin


            [풀이]
            git 명령어를 작성할건데, remote(원격 저장소)와의 연결을 rm(remove, 삭제) 한다.
            그 원격 저장소의 이름은 origin이다.
            ```
3. 원격 저장소에 업로드
- 실습 때 작성했던 TIL 파일을 Github 원격 저장소에 업로드 해보겠습니다.
- **정확히 말하면, 파일을 업로드하는 게 아니라 커밋을 업로드 하는 것입니다!**
- 따라서 먼저 로컬 저장소에서 커밋을 생성해야 원격 저장소에 업로드 할 수 있습니다.
    1. 로컬 저장소에서 커밋 생성
        ```bash
        # 현재 상태 확인

        $ git status
        On branch master

        No commits yet

        Untracked files:
        (use "git add <file>..." to include in what will be committed)
                day1.md

        nothing added to commit but untracked files present (use "git add" to track)
        ```
        ```bash
        $ git add day1.md
        ```
        ```bash
        $ git commit -m "Upload TIL Day1"
        [master (root-commit) f3d6d42] Upload TIL Day1
        1 file changed, 0 insertions(+), 0 deletions(-)
        create mode 100644 day1.md
        ```
        ```bash
        # 커밋 확인

        $ git log --oneline
        f3d6d42 (HEAD -> master) Upload TIL Day1
        ```
    2. git push
       - 로컬 저장소의 커밋을 원격 저장소에 업로드하는 명령어
       - `git push <저장소 이름> <브랜치 이름>` 형식으로 작성합니다.
       - `-u` 옵션을 사용하면, 두 번째 커밋부터는 `저장소 이름, 브랜치 이름`을 생략 가능합니다.
        ```bash
        $ git push origin master

        [풀이]
        git 명령어를 사용할건데, origin이라는 이름의 원격 저장소의 master 브랜치에 push 한다.

        ------------------------------------------------

        $ git push -u origin master
        이후에는 $ git push 라고만 작성해도 push가 됩니다.
        ```
    3. vscode 자격 증명 이후 git push 완료
        ```bash
        $ git push -u origin master
        info: please complete authentication in your browser...
        Enumerating objects: 3, done.
        Counting objects: 100% (3/3), done.
        Writing objects: 100% (3/3), 218 bytes | 218.00 KiB/s, done.
        Total 3 (delta 0), reused 0 (delta 0), pack-reused 0
        To https://github.com/edukyle/TIL.git
        * [new branch]      master -> master
        Branch 'master' set up to track remote branch 'master' from 'origin'.
        ```
    4. 원격 저장소에서 정상 업로드 확인
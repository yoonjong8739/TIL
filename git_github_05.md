# clone, pull
## [1] 원격 저장소 가져오기

> 지금까지는 로컬 저장소의 내용을 원격 저장소에 업로드하는 것을 학습했습니다.
이번에는 반대로, 원격 저장소의 내용을 로컬 저장소로 가져오는 것을 학습합니다.
> 

### (1) git clone

- 원격 저장소의 커밋 내역을 모두 가져와서, 로컬 저장소를 생성하는 명령어
- clone은 `"복제"`라는 뜻으로, `git clone` 명령어를 사용하면 원격 저장소를 통째로 복제해서 내 컴퓨터에 옮길 수 있습니다.
- `git clone <원격 저장소 주소>`의 형태로 작성합니다. 다음처럼 작성한 대로 실행하면, `Github의 edukyle이라는 계정의 TIL 원격 저장소를 복제`하여 내 컴퓨터에 TIL이라는 이름의 로컬 저장소를 생성하게 됩니다.

    ```bash
    $ git clone https://github.com/edukyle/TIL.git
    Cloning into 'TIL'...
    remote: Enumerating objects: 3, done.
    remote: Counting objects: 100% (3/3), done.
    remote: Total 3 (delta 0), reused 3 (delta 0), pack-reused 0
    Receiving objects: 100% (3/3), done.
    ```

- git clone을 통해 생성된 로컬 저장소는 `git init`과 `git remote add`가 이미 수행되어 있습니다.

### (2) git pull

- 원격 저장소의 변경 사항을 가져와서, 로컬 저장소를 업데이트하는 명령어
- 로컬 저장소와 원격 저장소의 내용이 완전 일치하면 git pull을 해도 변화가 일어나지 않습니다.
- `git pull <저장소 이름> <브랜치 이름>`의 형태로 작성합니다.
    ```bash
    $ git pull origin master
    From https://github.com/edukyle/git-practice
    * branch            master     -> FETCH_HEAD
    Updating 6570ecb..56809a9
    Fast-forward
    README.md | 1 +
    1 file changed, 1 insertion(+)


    [풀이]
    git 명령어를 사용할건데, origin이라는 원격 저장소의 master 브랜치의 내용을 가져온다(pull).
    ```

## 내 컴퓨터 ↔ Github(원격 저장소) ↔ 강의장 컴퓨터 (예시)
>지금까지는 로컬 저장소의 내용을 원격 저장소에 업로드하는 것을 학습했습니다.
이번에는 반대로, 원격 저장소의 내용을 로컬 저장소로 가져오는 것을 학습합니다.
>

### (1) git clone

- 원격 저장소의 커밋 내역을 모두 가져와서, 로컬 저장소를 생성하는 명령어
- clone은 `"복제"`라는 뜻으로, `git clone` 명령어를 사용하면 원격 저장소를 통째로 복제해서 내 컴퓨터에 옮길 수 있습니다.
- `git clone <원격 저장소 주소>`의 형태로 작성합니다. 다음처럼 작성한 대로 실행하면, Github의 edukyle이라는 계정의 TIL 원격 저장소를 복제하여 내 컴퓨터에 TIL이라는 이름의 로컬 저장소를 생성하게 됩니다.

    ```bash
    $ git clone https://github.com/edukyle/TIL.git
    Cloning into 'TIL'...
    remote: Enumerating objects: 3, done.
    remote: Counting objects: 100% (3/3), done.
    remote: Total 3 (delta 0), reused 3 (delta 0), pack-reused 0
    Receiving objects: 100% (3/3), done.
    ```
- git clone을 통해 생성된 로컬 저장소는 git init과 git remote add가 이미 수행되어 있습니다.

### (2) git pull

- 원격 저장소의 변경 사항을 가져와서, 로컬 저장소를 업데이트하는 명령어
- 로컬 저장소와 원격 저장소의 내용이 완전 일치하면 git pull을 해도 변화가 일어나지 않습니다.
- `git pull <저장소 이름> <브랜치 이름>`의 형태로 작성합니다.
    ```bash
    $ git pull origin master
    From https://github.com/edukyle/git-practice
    * branch            master     -> FETCH_HEAD
    Updating 6570ecb..56809a9
    Fast-forward
    README.md | 1 +
    1 file changed, 1 insertion(+)


    [풀이]
    git 명령어를 사용할건데, origin이라는 원격 저장소의 master 브랜치의 내용을 가져온다(pull).
    ```
## [2] 내 컴퓨터 ↔ Github(원격 저장소) ↔ 강의장 컴퓨터 (예시)
>지금까지는 로컬 저장소의 내용을 원격 저장소에 업로드하는 것을 학습했습니다.
이번에는 반대로, 원격 저장소의 내용을 로컬 저장소로 가져오는 것을 학습합니다.
>

### (1) 규칙

- 수업 때는 두 개의 폴더를 `"내 컴퓨터"`와 `"강의장 컴퓨터"` 라고 가정합니다.
- 내 컴퓨터에 있는 로컬 저장소의 이름은 `TIL-home` 입니다.
- 강의장 컴퓨터에 있는 로컬 저장소의 이름은 `TIL-class` 입니다.
- Github에 있는 원격 저장소의 이름은 `TIL-remote` 입니다.

### (2) 사전 세팅

- 홈 디렉토리 안에 `TIL-home` 폴더를 생성합니다.
- Github에서 `TIL-remote` 라는 이름의 원격 저장소를 생성합니다.
- `TIL-home` 폴더에서 vscode를 엽니다.
- 아래와 같은 절차를 진행합니다.

    ```bash
    # TIL-home

    $ git init
    $ touch day1.md
    $ git add .
    $ git commit -m "집에서 Day1 작성"
    $ git remote add origin https://github.com/edukyle/TIL-remote.git
    $ git push origin master
    ```
    `TIL-home` 로컬 저장소의 내용이 `TIL-remote` 원격 저장소에 그대로 반영되었습니다.
### (3) git clone

> 여러분은 이제 강의장에 왔습니다. 강의장 컴퓨터에는 여러분의 TIL 폴더가 없습니다.
> 
- Github에 있는 `TIL-remote`에서 `git clone`을 통해 내려 받습니다.
    
    ```bash
    # TIL-class

    $ git clone https://github.com/edukyle/TIL-remote.git TIL-class
    ```

    **원격 저장소는 `TIL-remote` 이지만, 위와 같이 작성하면 강의장 컴퓨터에는 `TIL-class`라는 이름으로 로컬 저장소가 생성됩니다. (내부 파일 내용은 똑같습니다. 단지 폴더의 이름만 바뀝니다.)**
### (4) git push

> 강의장 컴퓨터 → 원격 저장소
> 
- 강의장에서 새로운 파일을 만들고 원격 저장소에 업로드 합니다.

    ```bash
    # TIL-class

    $ touch day2.md
    $ git add .
    $ git commit -m "강의장에서 Day2 작성"
    $ git push origin master
    ```
### (5) git pull

> 원격 저장소 → 내 컴퓨터
> 
- 내 컴퓨터에는 day2.md가 없습니다. 왜냐하면 강의장 컴퓨터에서 day2.md를 만들어서 원격 저장소에 push 했기 때문입니다. 따라서 원격 저장소에서 day2.md에 대한 내역을 가져와야 합니다.

    ```bash
    # TIL-home

    $ git pull origin master
    ```
    이제 `내 컴퓨터, Github, 강의장 컴퓨터`의 내용은 동일합니다.
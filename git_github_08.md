# Branch-merge

## [1] Branch Merge

> 지금까지는 브랜치를 통해서 독립된 작업 공간을 만드는 것 까지 진행했습니다.
이제 각 브랜치에서의 작업이 끝나면 어떻게 할까요? 
그 작업 내용을 master에 반영해야 하지 않을까요?
지금부터는 `Merge`라고 하는 `병합`을 학습하면서 브랜치를 합치는 것을 살펴보겠습니다.
> 

### (1) git merge

- 분기된 브랜치들을 하나로 합치는 명령어
- `git merge <합칠 브랜치 이름>`의 형태로 사용합니다.
- **Merge하기 전에 일단 다른 브랜치를 합치려고 하는, 즉 메인 브랜치로 switch 해야합니다.**

    ```bash
    # 1. 현재 branch1과 branch2가 있고, HEAD가 가리키는 곳은 branch1 입니다.
    $ git branch
    * branch1
    branch2

    # 2. branch2를 branch1에 합치려면?
    $ git merge branch2

    # 3. branch1을 branch2에 합치려면?
    $ git switch branch2
    $ git merge branch1
    ```
### (2) Merge의 세 종류

1. **Fast-Forward**
    - 브랜치를 병합할 때 마치 `빨리감기`처럼 브랜치가 가리키는 커밋을 앞으로 이동시키는 것
현재 master는 C2 커밋을, hotfix는 C4 커밋을 가리키고 있습니다.
   1. ![그림](https://hg-edu.notion.site/image/https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2F454c4f14-e3f0-44b7-8395-1bc9d0365dec%2FUntitled.png?id=6b3bcf41-91f4-4352-982a-e231da2e031e&table=block&spaceId=f7ab64f0-6613-4035-b609-06b6865d9b61&width=1530&userId=&cache=v2)
   2. master에 hotfix를 병합하면 어떻게 될까요?

        ```bash
        $ git switch master
        $ git merge hotfix
        Updating s1d5f1s..1325sd4
        Fast-forward
        index.html | 2 ++
        1 file changed, 2 insertions(+)
        ```
   3. hotfix가 가리키는 C4는 C2에 기반한 커밋이므로, master가 C4에 이동하게 됩니다.
    
        이렇게 따로 merge 과정 없이 브랜치의 포인터가 이동하는 것을 `Fast-Forward`라고 합니다.

        ![그림](https://hg-edu.notion.site/image/https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2F8f7f9cc1-e5c6-4bb0-808b-56ab5dd6caff%2FUntitled.png?id=682ba800-750d-4fda-9922-ebddfa47ec0c&table=block&spaceId=f7ab64f0-6613-4035-b609-06b6865d9b61&width=1530&userId=&cache=v2)
   4. 병합이 완료된 hotfix는 더 이상 필요 없으므로 삭제합니다.

        ```bash
        $ git branch -d hotfix
        Deleted branch hotfix (1325sd4).
        ```
2. **3-Way Merge**
    - 브랜치를 병합할 때 `각 브랜치의 커밋 두개와 공통 조상 하나`를 사용하여 병합하는 것
    - 두 브랜치에서 `다른 파일` 혹은 `같은 파일의 다른 부분`을 수정했을 때 가능합니다.
    1. 현재 master는 C4 커밋을, iss53은 C5 커밋을 가리키고 있습니다.
        
        master와 iss53의 공통 조상은 C2 커밋입니다.

        ![그림](https://hg-edu.notion.site/image/https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2F11ffb183-4b65-41c9-8c40-058e681564e6%2FUntitled.png?id=5a90b8d8-94b9-4866-b478-369cd6139bf8&table=block&spaceId=f7ab64f0-6613-4035-b609-06b6865d9b61&width=1530&userId=&cache=v2)

    2. 이 상황에서 master에 iss53을 병합하면 어떻게 될까요?

        ```bash
        $ git switch master
        Switched to branch 'master'
        $ git merge iss53
        Merge made by the 'ort' strategy.
        index.html |    1 +
        1 file changed, 1 insertion(+)
        ```
   3. master와 iss53은 갈래가 나누어져 있기 때문에 Fast-Forward로 합쳐질 수 없습니다.
       
       따라서 공통 조상인 C2와 각자가 가리키는 커밋인 C4, C5를 비교하여 3-way merge를 진행합니다.

       ![그림](https://hg-edu.notion.site/image/https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2Fa76a9563-95f0-4cc8-abb9-ab2b8366682e%2FUntitled.png?id=7453fab3-5410-475c-b6a7-c10c3ecc4a71&table=block&spaceId=f7ab64f0-6613-4035-b609-06b6865d9b61&width=1530&userId=&cache=v2)

   4. 이때 생긴 C6는 master와 iss53이 병합되면서 발생한 Merge Commit입니다.
   5. 병합이 완료된 iss53은 더 이상 필요 없으므로 삭제합니다.

        ```bash
        $ git branch -d iss53
        Deleted branch iss53 (58sdf23).
        ```

3. **Merge Conflict**
    - 병합하는 두 브랜치에서 `같은 파일의 같은 부분`을 수정한 경우, Git이 어느 브랜치의 내용으로 작성해야 하는지 판단하지 못해서 발생하는 충돌(Conflict) 현상
    - 결국은 사용자가 직접 내용을 선택해서 Conflict를 해결해야 합니다.
    1. 현재 master는 C4 커밋을, iss53은 C5 커밋을 가리키고 있습니다.
        
        master와 iss53의 공통 조상은 C2 커밋입니다. `(3-way merge에서 상황과 같습니다)`

        ![그림](https://hg-edu.notion.site/image/https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2F11ffb183-4b65-41c9-8c40-058e681564e6%2FUntitled.png?id=4fb88864-5a83-4b57-a39c-450e58994e38&table=block&spaceId=f7ab64f0-6613-4035-b609-06b6865d9b61&width=1530&userId=&cache=v2)
    
    2. 3-way merge와는 달리, 만약 master와 iss53이 같은 파일의 같은 부분을 수정하고 병합한다면 어떤 일이 발생할까요?

        ```bash
        $ git merge iss53
        Auto-merging index.html
        CONFLICT (content): Merge conflict in index.html
        Automatic merge failed; fix conflicts and then commit the result.
        ```

    3. 충돌이 일어난 파일을 확인하기 위해 git status를 입력합니다.

        ```bash
        $ git status
        On branch master
        You have unmerged paths.
        (fix conflicts and run "git commit")

        Unmerged paths:
        (use "git add <file>..." to mark resolution)

            both modified:      index.html

        no changes added to commit (use "git add" and/or "git commit -a")
        ```
    4. index.html을 열어보면 아래와 같이 충돌 내역이 나옵니다.

        ```bash
        <<<<<<< HEAD:index.html
        <div id="footer">contact : email.support@github.com</div>
        =======
        <div id="footer">
        please contact us at support@github.com
        </div>
        >>>>>>> iss53:index.html
        ```
    5. `=======` 를 기준으로 위는 master의 내용, 아래는 iss53의 내용입니다.
    
        이 중 하나를 선택할 수도 있고, 둘 다 선택할 수도 있고, 아예 새롭게 작성할 수도 있습니다.

        ```bash
        <div id="footer">
        please contact us at email.support@github.com
        </div>
        ```
    6. 이후 git add와 git commit을 통해 병합한 내용을 커밋할 수 있습니다.

        ```bash
        $ git add .
        $ git commit
        ```
    7. Vim 편집기를 이용해서 커밋 내역을 수정할 수 있습니다.

        ```bash
        Merge branch 'iss53'

        Conflicts:
            index.html
        #
        # It looks like you may be committing a merge.
        # If this is not correct, please remove the file
        #	.git/MERGE_HEAD
        # and try again.


        # Please enter the commit message for your changes. Lines starting
        # with '#' will be ignored, and an empty message aborts the commit.
        # On branch master
        # All conflicts fixed but you are still merging.
        #
        # Changes to be committed:
        #	modified:   index.html
        #
        ```
    8. Vim 편집기를 통해 작성한 커밋이 이제 C6 커밋이 됩니다.
        ![그림](https://hg-edu.notion.site/image/https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2F586bcfc3-2aeb-4aed-840c-33b24bf19b8b%2FUntitled.png?id=5e577ad4-81b3-49c8-b953-d4364c6ccdd3&table=block&spaceId=f7ab64f0-6613-4035-b609-06b6865d9b61&width=1530&userId=&cache=v2)
    
    9. 병합이 완료된 iss53은 더 이상 필요 없으므로 삭제합니다.

## [2] Branch-merge Scenario

> 지금까지 학습했던 git merge와 세 가지 상황에 대해 다시 한 번 살펴봅니다.
> 

### (1) 사전 세팅

```bash
$ mkdir git_merge
$ cd git_merge

$ git init
$ touch test.txt

# test.txt 에 master test 1 을 입력 후 저장

$ git add .
$ git commit -m "master test 1"
```

### (2) Fast-Forward
1. `login` 브랜치 생성 및 이동합니다.

```bash
$ git switch -c login
```
2. `login.txt`를 만들고 커밋합니다.

```bash
$ touch login.txt
$ git add .
$ git commit -m "login test 1"
```
3. `master` 브랜치로 이동합니다.

```bash
$ git switch master

$ git log --oneline --all --graph
* df231d0 (login) login test 1
* 1e62b4c (HEAD -> master) master test 1
```
4. `master`에 `login`을 병합합니다.

```bash
$ git merge login
Updating 1e62b4c..df231d0
Fast-forward
 login.txt | 0
 1 file changed, 0 insertions(+), 0 deletions(-)
 create mode 100644 login.txt
```
5. 결과를 확인합니다. (**Fast-forward**, 단순히 HEAD를 앞으로 빨리감기)

```bash
$ git log --oneline --all --graph
* df231d0 (HEAD -> master, login) login test 1
* 1e62b4c master test 1
```
6. `login` 브랜치를 삭제합니다.

```bash
$ git branch -d login
Deleted branch login (was df231d0).

$ git log --oneline --all --graph
* df231d0 (HEAD -> master) login test 1
* 1e62b4c master test 1
```

### (3) 3-way Merge (Merge commit)
1. `signout` 브랜치를 생성 및 이동합니다.

```bash
$ git switch -c signout
```
2. 특정 작업 완료 후 커밋합니다.

```bash
$ touch signout.txt

$ git add .
$ git commit -m "signout test 1"

$ git log --oneline
bcade83 (HEAD -> signout) signout test 1
df231d0 (master) login test 1
1e62b4c master test 1
```
3. `master` 브랜치로 이동합니다.

```bash
$ git switch master
```
4. `master`에 추가 작업 후 커밋합니다. (단 **`signout`** **브랜치와 다른 파일**을 생성 혹은 수정합니다.)

```bash
$ touch master.txt

$ git add .
$ git commit -m "master test 2"

$ git log --all --oneline
48bd5a6 (HEAD -> master) master test 2
bcade83 (signout) signout test 1
df231d0 login test 1
1e62b4c master test 1

```
5. `master`에 `signout`을 병합합니다. (자동 merge commit 발생)

```bash
$ git merge signout
Merge made by the 'ort' strategy.
 signout.txt | 0
 1 file changed, 0 insertions(+), 0 deletions(-)
 create mode 100644 signout.txt
```
6. log 확인

```bash
$ git log --oneline --all --graph
*   ac0e971 (HEAD -> master) Merge branch 'signout'
|\
| * bcade83 (signout) signout test 1
* | 48bd5a6 master test 2
|/
* df231d0 login test 1
* 1e62b4c master test 1
```
7. `signout` 브랜치 삭제

```bash
$ git branch -d signout
Deleted branch signout (was bcade83).
```

### (4) Merge Conflict
1. `hotfix` 브랜치를 생성 및 이동합니다.

```bash
$ git switch -c hotfix
```
2. 특정 작업 완료 후 커밋합니다.

```bash
# test.txt 수정

master test 1
이건 hotfix에서 작성한 문장입니다.
```
```bash
$ git add .
$ git commit -m "hotfix test 1"

$ git log --oneline --graph --all
* ad045fa (HEAD -> hotfix) hotfix test 1
*   ac0e971 (master) Merge branch 'signout'
|\
| * bcade83 signout test 1
* | 48bd5a6 master test 2
|/
* df231d0 login test 1
* 1e62b4c master test 1
```
3. `master` 브랜치로 이동합니다.

```bash
$ git switch master
```
4. 특정 작업(`hotfix` 와 동일 파일의 동일 부분 수정) 완료 후 커밋합니다.

```bash
# text.txt 수정

master test 1
이건 master에서 작성한 문장입니다.
```
```bash
$ git add .
$ git commit -m "master test 3"

$ git log --oneline --all --graph
* 3170247 (HEAD -> master) master test 3
| * ad045fa (hotfix) hotfix test 1
|/
*   ac0e971 Merge branch 'signout'
|\
| * bcade83 signout test 1
* | 48bd5a6 master test 2
|/
* df231d0 login test 1
* 1e62b4c master test 1
```
5. `master`에 `hotfix`를 병합합니다.

```bash
$ git merge hotfix
```
6. 결과 → merge conflict 발생 (**같은 파일의 같은 문장을 수정했기 때문입니다!**)
![그림](https://hg-edu.notion.site/image/https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2F072a8eb0-4fa6-4759-806e-6ae38069eb8e%2FUntitled.png?id=57d06f14-2c2f-491b-abc0-c25ef93e1502&table=block&spaceId=f7ab64f0-6613-4035-b609-06b6865d9b61&width=1500&userId=&cache=v2)

7. 충돌 확인 및 해결
    - Merge Conflict가 일어났을 때 Git이 어떤 파일을 Merge 할 수 없었는지 살펴보려면 `git status` 명령을 이용합니다.
        ```bash
            $ git status
        On branch master
        You have unmerged paths.
        (fix conflicts and run "git commit")
        (use "git merge --abort" to abort the merge)

        Unmerged paths:
        (use "git add <file>..." to mark resolution)
                both modified:   test.txt

        no changes added to commit (use "git add" and/or "git commit -a")
        ```
        ```bash
        master test 1
        <<<<<<< HEAD
        이건 master에서 작성한 문장입니다.
        =======
        이건 hotfix에서 작성한 문장입니다.
        >>>>>>> hotfix
        ```
    - `=======` 위쪽의 내용은 HEAD 버전(merge 명령을 실행할 때 작업하던 `master` 브랜치)의 내용이고 아래쪽은 `hotfix` 브랜치의 내용입니다. 충돌을 해결하려면 위쪽이나 아래쪽 내용 중에서 고르거나 새로 작성하여 Merge 해야 합니다. 
(`<<<<<<<, =======, >>>>>>>` 가 포함된 행은 삭제)
        ```bash
        # test.txt 최종본

        master test 1
        이건 충돌을 해결한 후의 문장입니다.
        ```
8. merge commit 진행합니다.
```bash
$ git add .
$ git commit
```
- vim 편집기 등장
        
```bash
Merge branch 'hotfix'

# Conflicts:
#       test.txt
#
# It looks like you may be committing a merge.
# If this is not correct, please run
#       git update-ref -d MERGE_HEAD
# and try again.


# Please enter the commit message for your changes. Lines starting
# with '#' will be ignored, and an empty message aborts the commit.
#
# On branch master
# All conflicts fixed but you are still merging.
#
``` 
- 작성된 커밋 메세지를 확인하고 `esc` 를 누른후 `:wq` 를 입력하여 저장 & 종료합니다.

```bash
$ git commit
[master 8ef1443] Merge branch 'hotfix'
```
9.  log 확인

```bash
$ git log --oneline --all --graph
*   8ef1443 (HEAD -> master) Merge branch 'hotfix'
|\
| * ad045fa (hotfix) hotfix test 1
* | 3170247 master test 3
|/
*   ac0e971 Merge branch 'signout'
|\
| * bcade83 signout test 1
* | 48bd5a6 master test 2
|/
* df231d0 login test 1
* 1e62b4c master test 1
```
10.  `hotfix` 브랜치를 삭제합니다.
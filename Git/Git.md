# Git

### 원격저장소 조회

```bash
$ git remote -v		# verbose
origin  https://github.com/Hwangmi09/test.git (fetch)
origin  https://github.com/Hwangmi09/test.git (push)
```

### 원격저장소 추가

```bash
$ git remote add <원격저장소이름> <주소>
$ git remote add origin https://github.com/<username>/<저장소이름>.git
```

- 깃아, 원격 저장소(remote)를 추가해줘(add). `origin`이라는 이름으로, `주소`를
- 원격저장소가 이미 설정된 경우 설정이 되지 않는다. (remote origin already exists)

### 원격저장소 삭제

```bash
$ git remote rm <원격저장소이름>
```

### 원격저장소 push

```bash
$ git push <원격저장소이름> <브랜치이름>
$ git push origin master
Enumerating objects: 6, done.
Counting objects: 100% (6/6), done.
Delta compression using up to 4 threads
Compressing objects: 100% (2/2), done.
Writing objects: 100% (6/6), 423 bytes | 38.00 KiB/s, done.
Total 6 (delta 0), reused 0 (delta 0), pack-reused 0
To https://github.com/Hwangmi09/test.git
 * [new branch]      master -> master
```

- 깃, push.origin에 master브랜치를!!

- `-u`옵션 : upstream옵션

  - `git push`라고 명령을 하더라도 설정된 원격저장소에 브랜치를 push

  - `git push -u origin master`

    


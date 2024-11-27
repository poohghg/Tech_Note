
> 출처
>
> https://codingapple.com/course-status/

#### 3-way-merge

브랜치에 각각 신규 commit이 있으면 merge명령을 내리면 두 브랜치의 코드를 합쳐서 새로운 commit을 자동을 생성해준다.

![img](https://codingapple.com/wp-content/uploads/2022/06/merge1.png)

#### fast-forword merge

가끔은 새로운 브랜치에서만 commit이 있고, 기준이 되는 메인 브렌치에는 신규 commit이 없는 경우가 있다.이 경우 merge하게 되면 fast-forword merge처리 된다.

fast-forword merge는 메인 브랜치의 commit이없다면, 신규브랜치를 새롭게 메인브랜치로 설정한다.

![img](https://codingapple.com/wp-content/uploads/2022/06/%EA%B7%B8%EB%A6%BC3-4.png)

- 기준이 되는 브랜치에 신규 commit이 없다면 자동으로 fast-forword merge가 발동된다.
-  **git merge --no-ff 브랜치명** 해서 강제로 3-way merge 할 수도 있다.

#### 브랜치 삭제

merge이후에 브랜치는 자동으로 삭제되진 않는다. 병합이 완료되고 필요없는 브랜치를 삭제할 수 있는데 다음 명령어로 가능하다.

```
git branch -d 브랜치이름 // 병합이 안료된 이후
git branch -D 브랜치이름 // 병합하지 않는 브런치 삭제시
```

#### rebase and merge

rebase는 브랜치의 시작점을 다른 commit으로 옮겨주는 행위이다.

![img](https://codingapple.com/wp-content/uploads/2022/06/merge3.png)

1. rebase를 이용해서 신규브랜치의 시작점을 main브랜치 최근 commit으로 옮긴다.
2. fast-forword merge처리를 한다.

실제 rebase and merge의 순서이다.

1. 새로운 브랜치로 이동한다
2. git rebase main 하면된다.
3. 그럼 브랜치가 main브랜치 끝으로 이동하는데 그걸 fast-forward merge하면 된다.

```
git switch 새로운브랜치
git rebase main

git switch main
git merge 새로운브랜치
```

사실 rebase & merge는 쉽게 애기하면 강제 fast-forward merge이다.

- 단점으로는 브랜치끼리 차이가 너무 많은 경우 rebase하면 충돌이 많이 발생할 수 있다.

#### squash and merge 

모든 브랜치를 3-way merge하면 매우 복잡한 그래프 형태를 가질수 있다.

- 실제 main브랜치 입장에서는 3-way merge처리된 브랜치들의 commit내역도 모두 출력이 되기때문에 더러워지는 현상이 있다.

이러한 log트리의 복잡성을 줄이기 위해서 

1. squash and merge 처리 
2. 또는 rebase 하면 된다.

- 새로운 브랜치에 있던 commit들을 연결해주는게 아니라 커밋 내역들을 main브랜치에 합쳐준다.

![img](https://codingapple.com/wp-content/uploads/2022/06/%EA%B7%B8%EB%A6%BC2.png)

### 코드 복구

git은 버전관리 프로그램이기때문에 이전 commit내역으로 돌아가거나,문제가 되는 commit내역을 취소 할 수 있다. git restore,git revert git reset을 이용해서 각각 파일을 복구, commit복구, 시간 되돌리기가 가능하다.

#### restore

파일 하나가 잘못되었을경우 되돌리기 위해 사용된다.

```
git restore 파일명
```

특정 커밋시점 파일로 복구할 수 도 있다.

```
git restore --source 커밋아이디 파일명
```

#### revert

과거 커밋시점으로 복구도 가능하다

![img](https://codingapple.com/wp-content/uploads/2022/06/%EC%BA%A1%EC%B2%983-1.png)

- 다음과 같은 git log기록이 있고 b파일이 있는 commit을 취소하고 싶다면 다음과 같이 처리하면 된다.

```
git revert 커밋아이디
```

결과적으로 특정 커밋시점의  발생된 커밋만 삭제된다.

- 특정 커밋 시점이 삭제되어도 그 이후 에 생성된 커밋은 영향을 받지 않는다.

![img](https://codingapple.com/wp-content/uploads/2022/06/%EA%B7%B8%EB%A6%BC11-1.png)

- revert시 동시에 여러개 commit id를 입력할 수 있다.
- 최근 했던 commit 1개만 revert하고싶으면 git revert HEAD하면 된다.

#### reset

특정 커밋이 생성되었던 시점으로 되돌아 간다. 또한 현재 작업하고 있는폴더의 파일도 특정시점으로 돌아간다.

```
git reset --hard 커밋아이디
```

![img](https://codingapple.com/wp-content/uploads/2022/06/%EA%B7%B8%EB%A6%BC22.png)

- 이러면 commit2이후의 작업물은 모두 삭제된다.


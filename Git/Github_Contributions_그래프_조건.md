## Github Contributions 그래프가 채워지는 조건

Contribution 그래프가 채워지기 위해서는 다음의 세가지 조건이 모두 충족되어야 한다.

- 커밋할때 사용한 이메일 주소(local repository의 user.email)가 github계정의 이메일 주소와 같아야 한다.
- fork를한 commit은 적용되지 않고 독립적인 repository에서 이루어진 commit이여야 한다.
  - 이 때 fork를 실행한 commit이 그래프에 나타나게 하려면 fork한 repo의 parent repo에 merge될 수 있도록 open pull-request해야한다.
- 커밋은 다음으로 만들어져야 한다.
  - repository의 default branch (보통 main)


추가적으로 다음중에서 최소한 한개 이상은 조건이 맞아야 한다.

- repository의 Collaborator 이거나 해당 repository를 가지고 있는 Organization의 멤버면 된다.
- repository에 star를 주어야 합니다.
- repository의 pull request나 issue를 열어봐야 한다.
- repository를 이미 fork한 상태여야 한다.

대부분 위의 조건중에서 충족시키지 못하는 조건은 이메일 불일치이다.

## user 이메일 변경법

먼저 자신의 이메일 주소를 확인하는법

```null
git config user.email
```



전체 설정을 확인하는법

```
git config --list
```



email 변경 법 (github계정 이메일과 동일하게 바꿔주면 된다.)

```null
git config --global user.email [이메일@주소]
```

바뀐 이메일을 잘 바뀌었는지 확인만 해주면 끝.
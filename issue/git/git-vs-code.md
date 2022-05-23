# git 자동 로그인(VS Code)

## VS Code id/pw 설정을 자동 저장

터미널에서 하단의 명령어 입력.

이후에 id와 pw를 한 번만 입력하면 로그인 정보가 로컬 디스크에 저장되어서 자동으로 인증

```
git config --global credential.helper store
```

## 만약 이미 정보가 있는 경우

이미 있는 정보를 삭제하고 다시 설정

```
git config --unset user.name
git config --unset user.email
```

만약 global로 설정한 경우 --global을 추가

```
git config --unset --global user.name
git config --unset --global user.email
```

확인

```
git config --list
```

삭제를 마친 뒤 다시 등록

```
git config --global user.name "mingeon"
git config --global user.email "mingeon@git.com"
```

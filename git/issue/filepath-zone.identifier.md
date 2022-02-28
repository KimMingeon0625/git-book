# filePath:Zone.Identifier

## &#x20;발생 배경

Git clone을 진행하던 중 복사는 완료되었지만 checkout이 불가능하다는 로그 발견.&#x20;

```
$ git clone ~~~.git
error: invalid path 'src/~~~~~~/exit.svg:Zone.Identifier'
```

{% embed url="https://github.com/git-for-windows/git/issues/2777" %}
동일한 이슈
{% endembed %}

core.protectNTFS : 잠재적인 잘못된 파일 이름으로부터 NTFS 시스템을 보호.

system에 위협이 될만한 path를 check하는 것으로 경로에 제제를 가하게 된다. 위 github의 토론을 보았을 때 core.protect가 종종 문제를 일으키는 것으로 예상된다.



## 해결

1. `git config core.protectNTFS false`
2. 위 방법이 권한에 문제가 발생\
   관리자 권한으로 cmd창을 열어 아래의 command를 입력.\
   `git config --system core.protectNTFS false`

## 주의

clone에서 이슈가 발생했지만 checkout(분기점 이동)에서도 발생하는 것으로 파악된다.

path 관련 오류에 protectNTFS가 보이는 경우 해당 이슈의 대응법을 활용하자.

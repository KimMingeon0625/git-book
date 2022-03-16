# cannot deserialize from object value

## 발생 배경

팀원의 이슈에 대한 응답을 하기 위해 진행.

```
cannot deserialize from object value 
(no delegate- or property-based creator)
```

## 해결

* @NoArgsConstructor 추가 or 빈 생성자 추가

binding을 하는 과정에서 발생하는 오류.

binding의 과정에서 빈 생성자를 활용하는데 없어서 생기는 오류로 파악.



## 주의

평소 jackson의 파싱과정에서 발생하는 이슈.

하지만 이번경우 @RequestBody에서 발생.

오류의 원인을 정확히 파악하여 어느곳에서 발생하던 대응할 수 있는 자세가 필요.

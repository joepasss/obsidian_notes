### 특정 package USE flag 확인
**`equery uses MY_PACKAGE_NAME`**

### emerge된 패키지 리스트 확인
**`qlist -IRv`**


### 패키지 삭제
**`--deselect`**
`emerge --deselect` 또는 `emerge -W`
`@world set` 에서 패키지를 삭제함

**`--depclean`**
`emerge --depclean` 또는 `emerge -c`
현재 다른 패키지의 의존성이 없는 패키지 삭제

**`--unmerge`**
`emerge --unmerge` 또는 `emerge -C`
`--depclean`으로 삭제 불가능한 특정 패키지를 삭제할때 사용 (디펜던시 체크 안함, 경고 안해줌)

```
### 패키지 삭제

# 리스트 확인
qlist -IRv | grep -i "some pacakage"

# deselect
sudo emerge -avqW some package

# depclean
sudo emerge --depclean
```


### 패키지 업데이트

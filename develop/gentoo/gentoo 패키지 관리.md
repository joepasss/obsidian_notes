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


### 패키지 emerge time estimate

``` bash
genlop -ci

# 10초마다 watch
watch -cn 10 genlop -ci
```

### 패키지 dependencies 확인

``` bash
# -r (RDEPEND)
# -d (DEPEND)
# -p (PDEPEND)
qdepends <PACKAGE_NAME>

# to list all of the installed packages that depend on a package
qdepends -Q <PACKAGE_NAME>
```

### ebuilds, eclasses 확인

``` bash
qgrep <PAKCAGE_NAME>

# -J limit the search to installed packages.
# -N will print the atom instead of the filename
qgrep -JN <PACKAGE_NAME>
```

### USE flag 확인
```
quse <USE_FLAG>
```

### searching ebuild repositories
```
qsearch <package_name>
```

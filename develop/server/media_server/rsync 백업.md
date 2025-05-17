[gentoo wiki Rsync](https://wiki.gentoo.org/wiki/Rsync)

**패키지 설치**
`emerge -avq rsync`

**rsyncd 실행**
`rc-update add rsyncd default`
`rc-service rsyncd start`

**backup list 생성 & rsyncd 작성**

```
# /backup.list
/etc/
/etc/conf.d/
/etc/conf.d/hwclock

# /etc/rsyncd.conf
[sync]
	path = /
	include from = /backup.list
	exclude = *
```
:
### rsync 설치 & 실행

``` bash
# rsync 설치
sudo emerge -avq rsync

# 시스템 시작 시 자동 시작
sudo rc-update add rsyncd default

# rsync 시작
sudo rc-service rsyncd start
```

### inotify (linux filesystem notification tool) 설치

``` bash
# inotify 설치
sudo emerge -avq inotify-tools
```

### shell script 작성

``` bash
### rsync_backup.sh
#!/bin/bash

SOURCE="$1"
DEST="/backup$SOURCE"

rsync -av --delete $SOURCE $DEST

### backup.sh
#!/bin/bash

SCRIPT_PATH=$(dirname$0)
WATCH_DIR="$1"
BACKUP_SCRIPT="$SCRIPT_PATH/rsync_backup.sh"

inotifywait -m -r -e modify,create,delete $WATCH_DIR |
while read -r directory events filename; do
	$BACKUP_SCRIPT
done

```



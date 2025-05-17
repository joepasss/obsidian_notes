[software raid](https://wiki.gentoo.org/wiki/User:SwifT/Complete_Handbook/Software_RAID)

**디스크 정보 가져오기**
```bash
lsblk
```

**디스크 정리**
``` bash
sudo wipefs -af /dev/MY_DISK0
sudo wipefs -af /dev/MY_DISK1
sudo wipefs -af /dev/MY_DISK2

sudo mdadm --zero-superblock /dev/MY_DISK0
sudo mdadm --zero-superblock /dev/MY_DISK1
sudo mdadm --zero-superblock /dev/MY_DISK2
```

**mdadm RAID 구성(RAID5)**

``` bash
sudo mdadm --create --verbose /dev/md0 --level=5 --raid-devices=3 /dev/MY_DISK0 /dev/MY_DISK1 /dev/MY_DISK2

## 진행 상태 확인
cat /proc/mdstat
```

```
### cat /proc/mdstat 출력 결과
Personalities : [linear] [raid0] [raid1] [raid10] [raid6] [raid5] [raid4] 
md0 : active raid5 sdd[3] sdc[1] sdb[0]
      7813772288 blocks super 1.2 level 5, 512k chunk, algorithm 2 [3/2] [UU_]
      [>....................]  recovery =  0.8% (32378824/3906886144) finish=1266.4min speed=50989K/sec
      bitmap: 0/30 pages [0KB], 65536KB chunk

unused devices: <none>
```

**파일 시스템 만들기 (ext4)**
``` bash
sudo mkfs.ext4 /dev/md0
```

**마운트**
``` bash
sudo mkdir /mnt/my_mount
sudo mount /dev/md0 /mnt/my_mount

# 부팅시 자동 마운트
### /etc/fstab
/dev/md0 /mnt/my_mount ext4 defaults 0 0
```

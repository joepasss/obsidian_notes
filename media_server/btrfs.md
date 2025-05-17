**패키지 설치**
```
emerge -avq sys-fs/btfsf-progs
```

**기존 디스크 초기화**

``` bash
# lsblk 로 확인
lsblk

NAME   MAJ:MIN RM   SIZE RO TYPE MOUNTPOINTS
sda      8:0    0 238.5G  0 disk 
├─sda1   8:1    0     1G  0 part /efi
├─sda2   8:2    0     8G  0 part [SWAP]
└─sda3   8:3    0 229.5G  0 part /
sdb      8:16   0   3.6T  0 disk 
├─sdb1   8:17   0   200M  0 part 
└─sdb2   8:18   0   3.6T  0 part /mnt/ext_hdd
sdc      8:32   0   3.6T  0 disk 
└─sdc1   8:33   0   3.6T  0 part 
sdd      8:48   0   3.6T  0 disk 
└─sdd1   8:49   0   3.6T  0 part

# das 로 sdc, sdd 디스크 연결했으므로,
sudo wipefs -a /dev/sdc
sudo wipefs -a /dev/sdd

# wipefs 이후에 디스크 상태 확인
lsblk

NAME   MAJ:MIN RM   SIZE RO TYPE MOUNTPOINTS
sda      8:0    0 238.5G  0 disk 
├─sda1   8:1    0     1G  0 part /efi
├─sda2   8:2    0     8G  0 part [SWAP]
└─sda3   8:3    0 229.5G  0 part /
sdb      8:16   0   3.6T  0 disk 
├─sdb1   8:17   0   200M  0 part 
└─sdb2   8:18   0   3.6T  0 part /mnt/ext_hdd
sdc      8:32   0   3.6T  0 disk 
sdd      8:48   0   3.6T  0 disk 
```

**btrfs 로 디스크 포멧**

```
# sdc, sdd 두개 디스크 RAID0으로 묶기, 메타데이터는 raid1
sudo mkfs.btrfs -d raid0 -m raid1 /dev/sdc /dev/sdd

# 이후 blkid로 확인
sudo blkid /dev/sdc
/dev/sdc: UUID="ff99cad6-022d-413d-b098-ff5b6d0c35ad" UUID_SUB="f666dc07-2e1f-4a65-a5ef-e3510e07b585" BLOCK_SIZE="4096" TYPE="btrfs"

sudo blkid /dev/sdd
/dev/sdd: UUID="ff99cad6-022d-413d-b098-ff5b6d0c35ad" UUID_SUB="76c23ebf-c96d-4bea-83f7-1ff0f2609418" BLOCK_SIZE="4096" TYPE="btrfs"
```

 **disk mount**
```bash
sudo mkdir -p /mnt/my_mount
sudo mount /dev/sdc /mnt/my_mount

# 필요한 경우 권한 수정
sudo chown username:usergroup /mnt/my_mount

# 서버 부팅시 자동 mount
# /etc/fstab 수정
UUID=my_sdc_uuid /mnt/my_mount btrfs defaults 0 0

# 확인
df -h

sudo btrfs filesystem df /mnt/my_mount

```

2



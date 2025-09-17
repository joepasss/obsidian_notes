[Samba gentoo wiki](https://wiki.gentoo.org/wiki/Samba)

``` bash
# samba client use flag 활성화
touch /etc/portage/package.use/samba
echo "net-fs/samba client" | sudo tee -a /etc/portage/package.use/samba

# cifs-utils emerge
emerge -avq cifs-utils
```

*fstab 작성*
```bash
# //servername/sharename /path/to/mount cifs credentials=/path/to/crendentials,uid=1000,gid=1000,iocharset=utf8
```

---

local disk uuid 가져오기
[Removable_media/UUIDs_and_labels](https://wiki.gentoo.org/wiki/Removable_media#UUIDs_and_labels)

```bash
lsblk -o +fstype,label,uuid,partuuid
```
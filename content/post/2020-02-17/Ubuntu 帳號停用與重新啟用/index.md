---
title: Ubuntu 帳號停用與重新啟用
date: 2020-02-17
categories:
    - Ubuntu
tags:
---

## 帳號停用
將帳號設定為昨天過期
```bash
sudo usermod -e $(date -d "yesterday" +"%Y-%m-%d") <user>
```

確認帳號期限
```bash
sudo chage -l <user>
```

封存家目錄
```bash
cd /home
sudo tar -I pigz -p -cvf <name of archive>.tar.gz <username>
# 或以下
sudo tar czvfp <name of archive>.tar.gz <username>
```

> In addition, when doing a tar backup, it’s also good to add the following flags : p & (z/j)  
> -p will preserve the original file permissions  
> -z will compress using gzip (medium cpu usage, but less space)  
> -j will compress using bzip2 (lots of cpu, even less space)  
> -v verbose output (optional)

將封存檔移往 NFS 封存區
```bash
sudo rsync -avhP --remove-source-files <name of archive>.tar.gz /nfs/Backup/home_archive
# 或以下
sudo mv <name of archive>.tar.gz /nfs/Backup/home_archive
```

移除家目錄
```bash
sudo rm -rd /home/<user>
```

## 帳號重新啟用
重建家目錄
```bash
cd /home
sudo tar xzvf <name of archive>.tar.gz
```

重新啟用帳號
```bash
sudo usermod -e "" <user>
```

確認帳號期限
```bash
sudo chage -l <user>
```

## 參考資料
- [How to enable or disable a user? – Ask Ubuntu](https://askubuntu.com/questions/282806/how-to-enable-or-disable-a-user)
- [How can I create automatically expiring user accounts? – Unix & Linux Stack Exchange](https://unix.stackexchange.com/questions/80968/how-can-i-create-automatically-expiring-user-accounts)
- [How to Backup and Restore your Home directory | MyLinuxRamblings](https://mylinuxramblings.wordpress.com/2010/01/10/how-to-backup-and-restore-your-home-directory/)

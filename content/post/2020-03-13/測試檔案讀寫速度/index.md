---
title: 測試檔案讀寫速度
date: 2020-03-13
categories:
    - Ubuntu
tags:
---

可用於測試本機速度、NFS 速度等  
請先以 `cd` 指令切換至欲測試的路徑

# 測試檔案寫入速度
```bash
time dd if=/dev/zero of=testfile bs=16k count=128k
```

# 測試檔案讀取速度
```bash
time dd if=testfile of=/dev/null bs=16k
```

# 參考資料
- [performance – Measure & benchmark the speed & latency of file access on a mounted NFS share – Server Fault](https://serverfault.com/questions/324438/measure-benchmark-the-speed-latency-of-file-access-on-a-mounted-nfs-share/324489#324489)

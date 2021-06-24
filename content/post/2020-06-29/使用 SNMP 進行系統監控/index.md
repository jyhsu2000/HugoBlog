---
title: 使用 SNMP 進行系統監控
date: 2020-06-29
categories:
    - Ubuntu
tags:
---

## 監控端
使用 Docker 版，以簡化佈署複雜度  
  
[https://github.com/setiseta/docker-librenms](https://github.com/setiseta/docker-librenms)

1. 調整 docker-compose.yml  
修改 port  
修改時區為 `Asia/Taipei`（兩處）
2. 使用 docker-compose 佈署上述容器
3. 進入修改帳號密碼（預設帳號/密碼：`librenms`）
4. 根據需求於 HTTP server 設定反向代理

## 被監控端
安裝 snmpd 與 snmp（前者為服務本身，後者則包含 snmpwalk 等指令集）
```bash
sudo apt install snmpd snmp
```

從本機測試連線
```bash
snmpwalk -v 2c -c public localhost
```

修改 `/etc/snmp/snmpd.conf`
- 修改 agentAddress，使其能從遠端被訪問（存取控制則由防火牆直接管理）
    ```conf
    # 將
    agentAddress udp:127.0.0.1:161
    
    # 修改為
    agentAddress udp:161
    ```
- 修改資料收集範圍（參考 IDC 設定方式）
    ```conf
    # 將
    view   systemonly  included   .1.3.6.1.2.1.1
    view   systemonly  included   .1.3.6.1.2.1.25.1
    
    # 修改為
    view   systemonly  included   .1.3.6.1.
    ```
- 修改地理位置資訊
    ```conf
    sysLocation    IEE 201
    ```

重新啟動 snmpd
```bash
sudo systemctl restart snmpd.service
```

調整防火牆設定，允許監控端連入  
（若需由本機 docker 連入，則允許 `172.16.0.0/12` 連入 `udp:161`）
```bash
sudo ufw allow from 172.16.0.0/12 to any port 161
```

## 安裝插件
[https://docs.librenms.org/Extensions/Applications/](https://docs.librenms.org/Extensions/Applications/)

根據上述連結中的說明安裝 SNMP 插件，並於 LibreNMS 設定中開啟對應項目

部分插件（如 fail2ban）需要 sudo 權限，則須在 visudo 設定特權，並於 `/etc/snmp/snmpd.conf` 設定使用 sudo 執行
```conf
Debian-snmp ALL=(root) NOPASSWD: /etc/snmp/fail2ban
```
```conf
extend fail2ban /usr/bin/sudo /etc/snmp/fail2ban
```

## 常見問題
若 docker-compose 在 up 之後，持續無法連上資料庫，檢查 `data/config/config.php` 是否確實有資料庫連線設定，若無則補上
```php
<?php
 
## Have a look in misc/config_definitions.json for examples of settings you can set here. DO NOT EDIT misc/config_definitions.json!
 
### Database config
$config['db_host'] = "mysql";
$config['db_user'] = "root";
$config['db_pass'] = "pwd4librenms";
$config['db_name'] = "librenms";
```

若超連結路徑出現各種底線，將 `data/config/config.php` 中以下段落解除註解
```php
$config['base_url']        = "/";
```

若無法採集 fail2ban 的資訊，直接執行出現以下訊息
```
Can't locate JSON.pm in @INC (you may need to install the JSON module) (@INC contains: /etc/perl /usr/local/lib/x86_64-linux-gnu/perl/5.26.1 /usr/local/share/perl/5.26.1 /usr/lib/x86_64-linux-gnu/perl5/5.26 /usr/share/perl5 /usr/lib/x86_64-linux-gnu/perl/5.26 /usr/share/perl/5.26 /usr/local/lib/site_perl /usr/lib/x86_64-linux-gnu/perl-base) at ./fail2ban line 80.
BEGIN failed--compilation aborted at ./fail2ban line 80.
```
需安裝 `libjson-perl` 套件
```bash
sudo apt install libjson-perl
```

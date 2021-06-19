---
title: 使用 Telegram 接收 APC UPS 事件通知
date: 2019-12-12
categories:
    - Ubuntu
tags:
---

# 前置處理
- 已連接 APC UPS，並於機器中安裝 [apcupsd](http://www.apcupsd.org/)
- 向 [@BotFather](https://t.me/BotFather) 申請 Telegram Bot，並取得 token
- 安裝能夠透過 CLI 呼叫 Telegram Bot API 的工具  
（以下以 [TelegramBotCli](https://github.com/jyhsu2000/TelegramBotCli) 為例）

# 設定流程
1. 確認已完成 apcupsd 的設定，並能透過已下指令順利存取 UPS 狀態
    ```
    apcaccess status
    ```
2. 在 Terminal 開著的情況下，測試 UPS 能否順利偵測事件並發送通知  
~~（拔插頭看有沒有看到訊息之類的）~~

3. 進入 `/etc/apcupsd`，可以看到一些檔案  
`apccontrol` 是整個核心部分，有興趣可以看看，但官方警告不要修改這個  
於是我們要修改的是根據不同事件各自獨立的處理流程檔案

4. 開啟事件處理流程檔案，如 `offbattery`、`onbattery`、`commfailure`、`commok` 等我們希望收到通知的事件，預設應該會是類似以下內容  
主要是在事件發生時，先整理需要的訊息，並以管線轉送給設定好的通知程序，進行通知發送的動作  
其中第 16 行管線後的部分，即為我們希望修改的訊息轉送動作
    ```sh {hl_lines=[16]}
    #!/bin/sh
    #
    # This shell script if placed in /etc/apcupsd
    # will be called by /etc/apcupsd/apccontrol when the
    # UPS goes back on to the mains after a power failure.
    # We send an email message to root to notify him.
    #
    
    HOSTNAME=`hostname`
    MSG="$HOSTNAME UPS $1 Power has returned"
    #
    (
        echo "$MSG"
        echo " "
        /sbin/apcaccess status
    ) | $APCUPSD_MAIL -s "$MSG" $SYSADMIN
    exit 0
    ```

5. 我們希望將通知由預設的郵件通知改為 Telegram Bot 通知，因此需要修改管線後的部分
    ```sh {linenostart=16}
    ) | $APCUPSD_MAIL -s "$MSG" $SYSADMIN
    ```
    將其改為發送通知的指令，如：
    ```sh {linenostart=16}
    ) | xargs -0 python3 /home/jyhsu/TelegramBotCli/sendMessage.py
    ```

6. 重新啟動 apcupsd 服務
    ```sh
    sudo systemctl restart apcupsd.service
    ```

7. 再次測試 APC UPS 能否偵測並發送事件  
~~（再拔一次插頭）~~

# 參考資料
- [APCUPSD User Manual](http://www.apcupsd.org/manual/)
- [從零開始的 Telegram Bot | Sean’s Note](https://blog.sean.taipei/2017/05/telegram-bot)

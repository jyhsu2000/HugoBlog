---
title: 讓 LINE 以外部瀏覽器開啟超連結
date: 2020-11-11
categories:
    - LINE
tags:
    - LINE
    - LINE Bot
---

LINE 預設會以 in-app 的瀏覽器開啟超連結，但這種開啟方式卻容易使網頁的功能受限（排版崩壞、JS 無法正常運作等）  
若能以外部瀏覽器開啟超連結，該問題便能被克服。

**註：以下方法不僅限於使用訊息傳送的網址，透過 LINE 掃描的 QR 碼中所帶的網址亦有相同效果**

其實應對方式很簡單
針對我們需要讓使用者使用外部瀏覽器開啟的超連結
加上 `?openExternalBrowser=1` 參數即可

例如原本網址為
```
https://wp.kid7.club/
```

加上參數，變為以下網址
```
https://wp.kid7.club/?openExternalBrowser=1
```

如此便能使在 LINE 中點擊網址的使用者直接以外部瀏覽器開啟超連結

# 參考資料
- [LINE BOT Development Guidelines](https://developers.line.biz/media/partner-docs/LINE_BOT_Development_Guidelines.pdf) （P.52）

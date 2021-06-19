---
title: XAMPP 安裝 PHP_CodeSniffer
date: 2019-04-15
categories: PHP
tags:
---

**2019-10-06 更新：**  
此篇為舊筆記  
現已不建議使用 pear 安裝，建議直接使用 Composer 安裝全域套件

```bash
composer global require "squizlabs/php_codesniffer=*"
```

---

使用 XAMPP 時，  
若需安裝 [PHP_CodeSniffer](https://github.com/squizlabs/PHP_CodeSniffer)，  
一般會使用 PEAR 的安裝方案

```bash
pear install PHP_CodeSniffer
```

但很可能出現以下錯誤

```
Cannot use result of built-in function in write context in D:\xampp\php\pear\Archive\Tar.php on line 639
```

解決方案：
1. 進入檔案，將該行由
    ```php
    $v_att_list = & func_get_args();
    ```
    改為
    ```php
    $v_att_list = func_get_args();
    ```
2. 重新安裝 Archive_Tar
    ```bash
    pear install Archive_Tar
    ```
3. 重新安裝 PHP_CodeSniffer
    ```
    pear install PHP_CodeSniffer
    ```

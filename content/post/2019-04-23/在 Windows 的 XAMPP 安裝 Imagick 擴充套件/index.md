---
title: 在 Windows 的 XAMPP 安裝 Imagick 擴充套件
date: 2019-04-23
categories: PHP
tags:
---

1. 檢視 XAMPP 的 phpinfo()，確認以下項目
    - Architecture：x86 / x64
    - Thread Safety：enabled / disabled
    - Compiler：如 MSVC15 (Visual C++ 2017)
2. 到 [PECL Deps](https://windows.php.net/downloads/pecl/deps/) 查看對應以上 Architecture 與 Compiler 對應的 ImageMagick 版本
    - 如 `ImageMagick-7.0.7-11-vc15-x64.zip` 則為 `7.0.7`
3. 根據以下文章進行安裝，唯各步驟須注意使用上述版本
    - [How to install and enable the Imagick extension in XAMPP for Windows](https://ourcodeworld.com/articles/read/349/how-to-install-and-enable-the-imagick-extension-in-xampp-for-windows)
    - 若第一步驟找不到對應版本的 ImageMagick，可到[這裡](http://ftp.icm.edu.pl/packages/ImageMagick/binaries/)下載
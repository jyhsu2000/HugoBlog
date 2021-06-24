---
title: Anaconda 安裝筆記
date: 2019-10-06
categories:
    - Anaconda
    - Python
tags:
---

## 流程

1. 下載 Anaconda  
[https://www.anaconda.com/distribution/](https://www.anaconda.com/distribution/)
    ```bash
    wget https://repo.anaconda.com/archive/Anaconda3-2019.07-Linux-x86_64.sh
    ```

2. 安裝 Anaconda  
安裝路徑：**/opt/anaconda**  
不使用 Conda init 初始化
    ```bash
    sudo bash ./Anaconda3-2019.07-Linux-x86_64.sh
    ```

3. 建立 Anaconda 使用者
    ```bash
    sudo useradd anaconda
    ```

4. 設定資料夾權限
    ```bash
    sudo chown -R anaconda:anaconda /opt/anaconda
    sudo chmod -R go-w /opt/anaconda
    sudo chmod -R go+rX /opt/anaconda
    ```

5. 設定 PATH  
（給使用者：）在 **/etc/environment** 的 PATH 新增 **/opt/anaconda/bin**  
（給 root：）在 **/etc/profile** 新增 **export PATH=/opt/anaconda/bin:$PATH**

6. 更新 conda 本身  
（設定 PATH 給 sudo 比較麻煩，可使用下方方式呼叫，或 sudo -i 進 root 之後使用 conda）
    ```bash
    sudo $(which conda) update -n base -c defaults conda
    ```

7. 初始化 conda
    ```bash
    # 初始化給 Bash
    conda init
    # 初始化給 Zsh
    conda init zsh
    ```

8. 關閉登入自動 activate
    ```bash
    conda config --set auto_activate_base false
    ```

## 參考資料
- [Installing Anaconda for multiple users – Peter Roche – Medium](https://medium.com/@pjptech/installing-anaconda-for-multiple-users-650b2a6666c6)
- [[Day01]Anaconda環境安裝！ – iT 邦幫忙::一起幫忙解決難題，拯救 IT 人的一天](https://ithelp.ithome.com.tw/articles/10192460)
- [bash – How do I prevent Conda from activating the base environment by default? – Stack Overflow](https://stackoverflow.com/questions/54429210/how-do-i-prevent-conda-from-activating-the-base-environment-by-default)

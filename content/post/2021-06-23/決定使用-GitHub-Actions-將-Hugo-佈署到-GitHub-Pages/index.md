---
title: 決定使用 GitHub Actions 將 Hugo 佈署到 GitHub Pages
description: 學習的過程，總會希望寫些文章，記錄與分享自己所學
date: 2021-06-23
categories:
    - Hugo
tags:
---

## 被放棄的方案
過去，在[無名小站](https://zh.wikipedia.org/wiki/%E7%84%A1%E5%90%8D%E5%B0%8F%E7%AB%99)關閉之後，  
主要是使用 [WordPress](https://tw.wordpress.org/) 將網誌架設於自己租用的 VPS。  
但堂堂 WordPress 只用來寫網誌好像有點太浪費？  
而且 WordPress 的架設、更新、租用 VPS 等也都會是成本。

## 選擇 Hugo 與 GitHub Pages
選擇替代方案時，考量的幾個主要條件：
- 免費
- 持續更新
- 無須準備網站伺服器主機
- 不用花太多時間維護
- 使用 [Markdown](https://zh.wikipedia.org/wiki/Markdown) 撰寫

其中希望使用 Markdown 撰寫的原因，不外乎是平常的習慣，在 GitHub、GitLab、HackMD 等網站都是使用 Markdown。  
比起 WordPress 那些強大的文章編輯工具，Markdown 這類簡單的文字格式反而更適合我們有條理的編輯與呈現內容。

基於上述考量，認為不見得需要將網誌平台「架設」起來，  
而將整個網誌本身轉換為靜態網頁，或許也是個方案。  
並且若是靜態網站，就可以直接發佈到 [GitHub Pages](https://pages.github.com/) 而無須自行架設。

因此，決定往「靜態網站產生器」的方向尋找，  
除了以前就知道但一直沒去接觸的 [Jekyll](https://jekyllrb.com/) 之外，也得知了 [Hexo](https://hexo.io/zh-tw/) 與 [Hugo](https://gohugo.io/) 的存在。  
為了從這些當中選出一款來使用，除了各自官方網站的介紹之外，也稍微看了些比較的文章：
- [靜態網站產生器介紹](https://coreychen71.github.io/posts/2019-05/%E9%9D%9C%E6%85%8B%E7%B6%B2%E7%AB%99%E7%94%A2%E7%94%9F%E5%99%A8%E4%BB%8B%E7%B4%B9/)
- [靜態網站產生器大比拚](https://raychiutw.github.io/2019/Static-Site-Generator-Comparison/)

最終選擇了 Hugo 作為網誌的靜態網站產生器

## 實際佈署流程
網路上的資料超級多了，這邊就不贅述太細節的事情

1. 安裝 [Git](https://git-scm.com/) 與 [Hugo](https://gohugo.io/)  
須對於指令操作與 Git 版本控制略為熟悉。  
由於平常使用 Windows 作業系統，Hugo 的部份是依照[安裝說明](https://gohugo.io/getting-started/installing#binary-cross-platform)直接從 [Hugo 的 GitHub 儲存庫的釋出頁面](https://github.com/gohugoio/hugo/releases)下載  
為了面對未知的未來，選擇安裝了標示了 `extended` 字樣的擴充版本

2. 建立  Hugo 專案，初始化 Git 儲存庫，並新增主題  
Hugo 沒有預設主題，可以直接到 [Hugo Themes](https://themes.gohugo.io/) 尋找
    ```bash
    hugo new site HugoBlog
    cd HugoBlog
    git init
    git submodule add https://github.com/CaiJimmy/hugo-theme-stack/ themes/hugo-theme-stack
    ```

3. 將主題中 `exampleSite` 資料夾的內容，複製到專案資料夾，並嘗試將 Hugo 執行起來
    ```bash
    hugo server -D
    ```

4. 在 `config.yaml` 或 `config.toml` 完成設定，並嘗試新增頁面或文章

5. 為專案建立 `.gitignore` 檔案，避免 Git 追蹤不必要的檔案  
可以直接使用 [gitignore.io](https://gitignore.io/?templates=hugo) 網站生成需要忽略的清單

6. 建立 GitHub 儲存庫，將專案推送至主線

7. 設定 GitHub Actions  
Workflow 使用 [GitHub Actions for Hugo](https://github.com/peaceiris/actions-hugo) 進行專案建置，  
再利用 [GitHub Actions for GitHub Pages](https://github.com/peaceiris/actions-gh-pages) 將建置後的檔案發佈至 GitHub Pages。  
要注意的是，若須自訂域名，須在其中 [actions-gh-pages](https://github.com/peaceiris/actions-gh-pages) 章節的 `with` 指令 `cname` 參數  
具體可參考[本網誌的 Workflow](https://github.com/jyhsu2000/HugoBlog/blob/master/.github/workflows/github-pages.yml)

8. 設定 GitHub Pages  
`Source` 指定為 `gh-pages` 分支  
若須自訂域名，設定的域名須與前一步驟相符

9. 未來若新增或修改文章，推送至主線後，將自動觸發 GitHub Actions 將網站內容發佈

## 結語
將網誌轉為使用 GitHub Actions 自動將 Hugo 佈署到 GitHub Pages 之後，  
可有效省去維護主機與網站的時間成本與金錢成本。

編輯文章方面，我習慣在電腦中使用 [VSCode](https://code.visualstudio.com/) 編輯，並使用 Hugo 預覽，確認沒問題後，再推送至 GitHub 儲存庫。  
但如果真要方便，也可以直接使用 GitHub 網頁版，或使用 [HackMD](https://hackmd.io/)/[HedgeDoc](https://hedgedoc.org/) 等平台連動 GitHub 儲存庫的方式進行編輯，  
如此一來，甚至只須使用瀏覽器即可完成文章編修。

## 參考資料
- [使用 Github Actions 來自動化部署 Hugo 到 Github Pages](https://blog.puckwang.com/post/2020/use-github-actions-deploy-hugo/)
- [Hugo Documentation](https://gohugo.io/documentation/)

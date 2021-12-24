---
title: 自動切換 tqdm 進度條的顯示模式
description: 使用 tqdm？還是 tqdm.notebook？不如讓它自動偵測吧
date: 2021-12-24
categories:
    - Python
tags:
---

## 前言
使用 Python 時，若須顯示進度條，常會使用 [tqdm](https://github.com/tqdm/tqdm) 套件來顯示進度條。

## 介紹
tqdm 是個方便用於顯示進度條的套件，只要簡單的使用 `tqdm()` 將要迭代的變數包起來，即可直接顯示進度條

```python
from tqdm import tqdm
for i in tqdm(range(10000)):
    ...
```

然而若使用 Jupyter Notebook 來執行，顯示上會怪怪的，  
一般會替換使用專為 Notebook 設計的 `tqdm.notebook`

```python
from tqdm.notebook import tqdm
for i in tqdm(range(10000)):
    ...
```

那如果同一份程式碼，要用於指令界面與 Notebook 呢？  
正好套件也有提供能自動偵測環境的 tqdm  
只要改用 `tqdm.auto` 就可以了

```python
from tqdm.auto import tqdm
for i in tqdm(range(10000)):
    ...
```

## 結語

用 `tqdm.auto` 就對了

## 參考資料
- [tqdm - A Fast, Extensible Progress Bar for Python and CLI](https://github.com/tqdm/tqdm)

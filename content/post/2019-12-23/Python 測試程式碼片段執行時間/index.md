---
title: Python 測試程式碼片段執行時間
date: 2019-12-23
categories:
    - Python
tags:
---

測試 Python 的程式碼片段實際執行所需秒數
```python
import time
 
start = time.perf_counter()
# Do something
end = time.perf_counter()
elapsed = end - start
print("elapsed:", elapsed)
```

## 參考資料
- [time — Time access and conversions — Python 3 documentation](https://docs.python.org/3/library/time.html)

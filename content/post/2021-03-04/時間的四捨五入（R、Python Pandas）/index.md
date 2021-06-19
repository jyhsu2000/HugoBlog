---
title: 時間的四捨五入（R、Python Pandas）
date: 2021-03-04
categories:
    - Python
    - R
tags:
    - 資料處理
---

資料處理時，常需要將時間轉換為接近的整點，  
如：9:47 轉換為 10:00；14:03 轉換為 14:00

或是以 15 分鐘為區間，  
如：9:47 轉換為 9:45、5:23 轉換為 5:30

以下分別介紹 R 與 Python 兩種語言的處理方式

# R
使用 [lubridate](https://www.rdocumentation.org/packages/lubridate) 套件中的 `round_date`（四捨五入）、`floor_date`（向下取整）、`ceiling_date`（向上取整）  
三者皆可依照需求指定時間精度，以下範例分別以 5 分鐘、10 分鐘、1 小時取接近值
```R
x <- as.POSIXct("2017-10-11 09:49:03")
 
lubridate::round_date(x, "5 minute")      # 2017-10-11 09:50:00
lubridate::round_date(x, "10 minute")     # 2017-10-11 09:50:00
lubridate::round_date(x, "hour")          # 2017-10-11 10:00:00
 
lubridate::floor_date(x, "5 minute")      # 2017-10-11 09:45:00
lubridate::floor_date(x, "10 minute")     # 2017-10-11 09:40:00
lubridate::floor_date(x, "hour")          # 2017-10-11 10:00:00
 
lubridate::ceiling_date(x, "5 minute")    # 2017-10-11 09:50:00
lubridate::ceiling_date(x, "10 minute")   # 2017-10-11 09:50:00
lubridate::ceiling_date(x, "hour")        # 2017-10-11 10:00:00
```

# Python
Python 本身較難處理這類情況，因此會使用 [Pandas](https://pandas.pydata.org/) 套件中 [pandas.Series.dt](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.Series.dt.html) accessor，使用其 [`round`](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.Series.dt.round.html)（四捨五入）、[`floor`](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.Series.dt.floor.html)（向下取整）、[`ceil`](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.Series.dt.ceil.html)（向上取整）
亦可依照需求指定時間精度，以下範例分別以 5 分鐘、10 分鐘、1 小時取接近值
```python
x = pd.Series([datetime(2017,10,11,9,49,3)])
 
x.dt.round('5 min')    # 2017-10-11 09:50:00
x.dt.round('10 min')   # 2017-10-11 09:50:00
x.dt.round('1 h')      # 2017-10-11 10:00:00
 
x.dt.floor('5 min')    # 2017-10-11 09:45:00
x.dt.floor('10 min')   # 2017-10-11 09:40:00
x.dt.floor('1 h')      # 2017-10-11 09:00:00
 
x.dt.ceil('5 min')     # 2017-10-11 09:50:00
x.dt.ceil('10 min')    # 2017-10-11 09:50:00
x.dt.ceil('1 h')       # 2017-10-11 10:00:00
```

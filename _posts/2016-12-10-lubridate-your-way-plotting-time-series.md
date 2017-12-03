---
layout: post
title: 用 lubridate 處理時間序列(time series)中的日期
categories: [rstats, graphics]
tags: [lubridate, time series, ggplot2]
lang: ch
comments: true
---



這學期讓學生作期末專題時，有幾位學生想處理的資料是內含日期的時間序列(time series)。因為日期讀入 R 以後會儲存成字串(如果有記得`stringsAsFactor = FALSE`)，就算想畫一張時間序列圖，都需要將資料清理一下才行。這時 Hadley Wickham 開發的 [lubridate](https://cran.r-project.org/web/packages/lubridate/vignettes/lubridate.html) 就相當好用。這篇文章用到的包裹如下


```r
library(dplyr)
library(ggplot2)
library(lubridate)
```

## 實作時間序列圖

這次實作的資料是取自 Kaggle Dataset 中的 [Daily Sea Ice Extent Data](https://www.kaggle.com/nsidcorg/daily-sea-ice-extent-data)，但我已經把一些不需要的部分清掉，只留下2009年的部分。


```r
seaice.09 <- read.csv("2016-12-10-seaice_2009.csv", stringsAsFactors = FALSE) %>% select(-Missing)
head(seaice.09,3)
```

```
##   Year Month Day Extent hemisphere
## 1 2009     1   1 13.189      north
## 2 2009     1   2 13.180      north
## 3 2009     1   3 13.267      north
```

這裡的日期被存在三個不同的欄位`Year`、`Month`、`Day`中，所以第一步要先用`paste()`將資料合併成`2009-6-30`樣式的字串，再用lubridate中的`ymd()`函數(意指這裡的字串是以年/月/日的順序讀入)轉換成 R 的標準日期格式(POSIX date format)。



```r
seaice <- seaice.09 %>% 
  mutate(date = (paste(Year, Month, Day, sep="-") %>% ymd()))
head(seaice,3)
```

```
##   Year Month Day Extent hemisphere       date
## 1 2009     1   1 13.189      north 2009-01-01
## 2 2009     1   2 13.180      north 2009-01-02
## 3 2009     1   3 13.267      north 2009-01-03
```
現在`date`就是正常的日期格式，可以丟進`ggplot()`作圖。

```r
class(seaice$date)
```

```
## [1] "Date"
```

```r
seaice %>% ggplot(aes(x=date, y=Extent, color=hemisphere)) + geom_point()
```

![plot of chunk unnamed-chunk-4](/figure/source/2016-12-10-lubridate-your-way-plotting-time-series/unnamed-chunk-4-1.png)

## 轉換日期為十進位數值

ggplot2在作圖時可以直接用日期的資料格式，但如果要進行迴歸分析，就必須把日期轉換成數值格式才行。好在 lubridate 中的 `decimal_date()` 可以幫我們快速轉換。(如果要把數值轉為日期則是`date_decimal()`)


```r
seaice %>% mutate(num.date = decimal_date(date)) %>% head(3)
```

```
##   Year Month Day Extent hemisphere       date num.date
## 1 2009     1   1 13.189      north 2009-01-01 2009.000
## 2 2009     1   2 13.180      north 2009-01-02 2009.003
## 3 2009     1   3 13.267      north 2009-01-03 2009.005
```

## 其他日期排列組合

如果資料的日期不是(年-月-日)而是其他種組合怎麼辦？lubridate 中還有其他函數像是`mdy()`、`dmy()`等等可以利用。舉例來說，我們可以把美國常見(月/日/年)格式的`06/04/2010` 轉換成標準的 R 日期格式 `2010-06-04`.


```r
example.data <- data.frame(
  id = c("A","B","C"),
  before.date = c("06/04/2010","06/11/2010","06/23/2010")
)

example.data %>% mutate(after.date = mdy(before.date))
```

```
##   id before.date after.date
## 1  A  06/04/2010 2010-06-04
## 2  B  06/11/2010 2010-06-11
## 3  C  06/23/2010 2010-06-23
```

lubridate在整理日期時可以處理許多種類型的字串，包括20090614或是2009/06/14都能轉換成POSIX標準的年-月-日格式，相當實用。

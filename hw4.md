Facebook粉絲團分析（分析專頁：柯文哲）
================

分析台北市長柯文哲粉絲專頁之每日發文數、likes數、comments數與shares數，資料分析區間為2016/01/01至2016/04/10

讀取柯文哲粉絲團資料
--------------------

``` r
if (!require('Rfacebook')){
  install.packages("Rfacebook")
  library(Rfacebook)
}
```

``` r
token <- 'CAAXnQ0o1ZBxgBANgnKnnc5z6xIpEmpkwyjYK0Hn3ZAC3aGRf94pc3UkGB63dbmsabWVvVBaaJJjWor4eKA2PmL1bMrSI2nN9uvrENdHe7qXQxEeEhCGuTWTDGNoWRbrqqFeEYHYD0CAqFKC8g0Vcz3huo1mFwjd8cMXZAmtfyZAeSO7v27PX9cOuXOnBrE33dGZBVZCQVHvPLNxWg8jGK13wYV6ZBmnOEgZD'
totalPage<-NULL
lastDate<-Sys.Date()
DateVectorStr<-as.character(seq(as.Date("2016-01-01"),lastDate,by="5 days"))
for(i in 1:(length(DateVectorStr)-1)){
  tempPage<-getPage("DoctorKoWJ",token,
                    since = DateVectorStr[i],until = DateVectorStr[i+1])
  totalPage<-rbind(totalPage,tempPage)
}
```

    ## 2 posts 10 posts 3 posts 2 posts 4 posts 4 posts 3 posts 4 posts 2 posts 1 posts 1 posts 3 posts 3 posts 2 posts 3 posts 2 posts 4 posts 2 posts 1 posts 1 posts

``` r
nrow(totalPage)
```

    ## [1] 57

2016/01/01至2016/04/10 柯文哲粉絲團一共有57篇文章

每日發文數分析
--------------

說明: 分析柯文哲粉絲團每天的發文數，由於日期格式一開始以世界各 時區作為劃分，在換成台灣的日期上可能會有誤差，所以先將其它換為台灣台北時區，再來利用weekdays()讓每個日期都再給予星期幾，再用aggregate來分組計算發文數放入PostCount，最後以發文數高至低顯示出來。

``` r
totalPage$datetime <- as.POSIXct(totalPage$created_time,
                                 format =  "%Y-%m-%dT%H:%M:%S+0000",
                                 tz = "GMT")
totalPage$dateTPE <- format(totalPage$datetime, "%Y-%m-%d", 
                            tz = "Asia/Taipei")
totalPage$weekdays <-weekdays(as.Date(totalPage$dateTPE))
PostCount<-aggregate(id~dateTPE+weekdays,totalPage,length)  
library(knitr)
kable(head(PostCount[order(PostCount$id,decreasing = T),]))
```

|     | dateTPE    | weekdays |   id|
|-----|:-----------|:---------|----:|
| 26  | 2016-01-09 | 星期六   |    4|
| 10  | 2016-03-22 | 星期二   |    2|
| 18  | 2016-01-08 | 星期五   |    2|
| 30  | 2016-02-06 | 星期六   |    2|
| 34  | 2016-01-10 | 星期日   |    2|
| 1   | 2016-01-25 | 星期一   |    1|

討論:2016/01/9（週六）的發文數最多，一共有四篇，其原因是因為這天柯市長挑戰『一日北高，雙城挑戰』，所以以影片和發文來分享他的騎乘過程，再來2016/01/8與/01/10、02/6、03/22這幾天都是兩篇居次，而1/10會有兩篇文章也是雙城挑戰的延續；而02/6這天因為台南發生了大地震，市長為了替南部朋友加油打氣及鼓勵國人捐款而發文。

每日likes數分析
---------------

說明:以aggregate來分組將likes數以每天發文數來做平均計算，再用kable與head來由高至低取前6名排序出來。

``` r
totalPage$weekdays <-weekdays(as.Date(totalPage$dateTPE))
LikesCount<-aggregate(likes_count~dateTPE+weekdays,totalPage,mean)
library(knitr)
kable(head(LikesCount[order(LikesCount$likes_count,decreasing = T),]))
```

|     | dateTPE    | weekdays |  likes\_count|
|-----|:-----------|:---------|-------------:|
| 27  | 2016-01-16 | 星期六   |        329087|
| 38  | 2016-02-28 | 星期日   |        228900|
| 34  | 2016-01-10 | 星期日   |        223666|
| 42  | 2016-01-14 | 星期四   |        187448|
| 32  | 2016-02-27 | 星期六   |        180919|
| 5   | 2016-03-28 | 星期一   |        139065|

討論:結果顯示2016/01/16這天以32萬多的likes數最多，其原因是這天台灣總統大選結果出爐，由蔡英文當選成為台灣第一個女總統，市長發文表示祝福並以容忍、包容、和解、和諧期許台灣的未來；而2016/02/28居次，原因是這天以挑戰一日雙塔來紀念228，以自身為受難家屬用以「寬容如海、成就臺灣」與大家共勉；2016/01/10這天排第三名，主要原因是柯市長挑戰一日雙城完後的發文獲得許多的關注。

每日comments數分析
------------------

說明:以aggregate來分組將comments數以每天發文數來做平均計算，再用kable與head來由高至低取前6名排序出來。

``` r
CommentsCount<-aggregate(comments_count~dateTPE+weekdays,totalPage,mean)
library(knitr)
kable(head(CommentsCount[order(CommentsCount$comments_count,decreasing = T),]))
```

|     | dateTPE    | weekdays |  comments\_count|
|-----|:-----------|:---------|----------------:|
| 34  | 2016-01-10 | 星期日   |          5981.50|
| 26  | 2016-01-09 | 星期六   |          5153.25|
| 5   | 2016-03-28 | 星期一   |          5103.00|
| 38  | 2016-02-28 | 星期日   |          3565.00|
| 32  | 2016-02-27 | 星期六   |          3268.00|
| 42  | 2016-01-14 | 星期四   |          2848.00|

討論:2016/01/10這天comments數5千9百多為最多，原因也是市長再挑戰完雙塔後的發文獲得許多網友的關注；2016/01/09居次也因為是挑戰雙塔當天的發文所帶來的影響;2016/03/28這天發生隨機殺人案震驚全台灣，市長以「愛的相反詞不是恨，而是冷漠」的論點談論此事件並認同當事人母親所說「從根本、從家庭、從教育來讓這樣子的人消失在社會上」獲得許多人認同。

每日shares數分析
----------------

說明:以aggregate來分組將shares數以每天發文數來做平均計算，再用kable與head來由高至低取前6名排序出來。

``` r
SharesCount<-aggregate(shares_count~dateTPE+weekdays,totalPage,mean)
library(knitr)
kable(head(SharesCount[order(SharesCount$shares_count,decreasing = T),]))
```

|     | dateTPE    | weekdays |  shares\_count|
|-----|:-----------|:---------|--------------:|
| 42  | 2016-01-14 | 星期四   |        34775.0|
| 34  | 2016-01-10 | 星期日   |        16407.5|
| 38  | 2016-02-28 | 星期日   |        11235.0|
| 23  | 2016-03-04 | 星期五   |         5334.0|
| 5   | 2016-03-28 | 星期一   |         4967.0|
| 27  | 2016-01-16 | 星期六   |         4897.0|

討論:2016/01/14這天獲得3萬4千多shares數居第一，原因是市長發了一篇關於「夢想」看法的文，以自身為56歲的阿北挑戰雙塔成功來勉勵國人做自己的主人,展現意志力追求夢想獲得許多人的認同與分享;2016/01/10居次，原因是成功完成雙塔後的發文引來許多網友的分享與鼓勵；第三是2016/02/28這天的發文，雖然市長沒有出席重要的紀念會，但市長今天特別的以過人的毅力達成一日雙塔來紀念228，並在發文中表達身為受難家屬的感觸獲得許多網友的分享。

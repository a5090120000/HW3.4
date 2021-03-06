Facebook粉絲專業團分析(分析專業名稱柯文哲)
================

說明:分析柯文哲粉絲專業，資料分析區間為2016/01/01到2016/04/11。

讀取柯文哲粉絲團資料
--------------------

    if (!require('Rfacebook')){
        install.packages("Rfacebook")
        library(Rfacebook)
    }


    totalPage<-NULL
    lastDate<-Sys.Date()
    numberOfPost<-30
    DateVector<-seq(as.Date("2016-01-01"),lastDate,by="5 days")
    DateVectorStr<-as.character(DateVector)


    token<-'EAACEdEose0cBAJzcKuZCUaZB3ZASRv3s5rLxVAeyDaBCCMrwf9MMZAGxg0aVqyUY6pMu3LwoZAZBhEqIzZBaaPdmZCAaCtrdvuyFMs6u9XOvnDuAerjdc0KIBy9pVVczkvaEAC7nqW0Tq4q0gDIUHlYMvpC6XYMzBODR8Jmgi4b13AZDZD'
    for(i in 1:(length(DateVectorStr)-1)){
        tempPage<-getPage("DoctorKoWJ", token,
                          since = DateVectorStr[i],until = DateVectorStr[i+1])
        totalPage<-rbind(totalPage,tempPage)
    }

    ## 2 posts 10 posts 3 posts 2 posts 4 posts 4 posts 3 posts 4 posts 2 posts 1 posts 1 posts 3 posts 3 posts 2 posts 3 posts 2 posts 4 posts 2 posts 1 posts 1 posts 

    nrow(totalPage)

    ##57

討論:2016/01/01至2016/04/11柯文哲粉絲團一共有57篇文章。

每日發文數分析
--------------

說明:分析柯文哲粉絲團每天的發文數，由於日期格式跟台灣不一樣，先將其轉換為台灣時區。

    totalPage$datetime <- as.POSIXct(totalPage$created_time, 
                                     format = "%Y-%m-%dT%H:%M:%S+0000", 
                                     tz = "GMT") #2016-01-16T15:05:36+0000
    totalPage$dateTPE <- format(totalPage$datetime, "%Y-%m-%d", 
                                tz = "Asia/Taipei") #2016-01-16
    totalPage$weekdays <-weekdays(as.Date(totalPage$dateTPE))
    PostCount<-aggregate(id~dateTPE,totalPage,length)
    library(knitr)
    kable(head(PostCount[order(PostCount$id,decreasing = T),]))

|     | dateTPE    |   id|
|:----|:-----------|----:|
| 6   | 2016-01-09 |    4|
| 5   | 2016-01-08 |    2|
| 7   | 2016-01-10 |    2|
| 25  | 2016-02-06 |    2|
| 44  | 2016-03-22 |    2|
| 1   | 2016-01-01 |    1|

討論:由此可看出柯文哲在1月8日到10日的PO文數最密集，其中9日就PO了4篇文章，正值「一日北高，雙城挑戰」的時候，有3篇為直播影片，是這段時間內柯文哲最大的活動之一。

每日Likes數分析
---------------

說明:分析柯文哲粉絲團每天的Likes數，由於日期格式跟台灣不一樣，先將其轉換為台灣時區。

    totalPage$datetime <- as.POSIXct(totalPage$created_time, 
                                     format = "%Y-%m-%dT%H:%M:%S+0000", 
                                     tz = "GMT") #2016-01-16T15:05:36+0000
    totalPage$dateTPE <- format(totalPage$datetime, "%Y-%m-%d", 
                                tz = "Asia/Taipei") #2016-01-16
    totalPage$weekdays <-weekdays(as.Date(totalPage$dateTPE))
    likesCount<-aggregate(likes_count~dateTPE,totalPage,mean)
    library(knitr)
    kable(head(likesCount[order(likesCount$likes_count,decreasing = T),]))

|     | dateTPE    |  likes\_count|
|:----|:-----------|-------------:|
| 11  | 2016-01-16 |        329087|
| 33  | 2016-02-28 |        228900|
| 7   | 2016-01-10 |        223666|
| 9   | 2016-01-14 |        187448|
| 32  | 2016-02-27 |        180919|
| 47  | 2016-03-28 |        139066|

討論:由Likes數可看出1月16日獲得最多的Likes，正是蔡英文總統當選第14任的總統而發表的恭賀文，第二多的則是2.28紀念日感言，以市長以受害者家屬的身分參與二二八紀念儀式，第三多的則是1月10日的「一日北高，雙城挑戰」的挑戰結束與感言。

每日Comments數分析
------------------

說明:分析柯文哲粉絲團每天的Comments數，由於日期格式跟台灣不一樣，先將其轉換為台灣時區。

    totalPage$datetime <- as.POSIXct(totalPage$created_time, 
                                     format = "%Y-%m-%dT%H:%M:%S+0000", 
                                     tz = "GMT") #2016-01-16T15:05:36+0000
    totalPage$dateTPE <- format(totalPage$datetime, "%Y-%m-%d", 
                                tz = "Asia/Taipei") #2016-01-16
    totalPage$weekdays <-weekdays(as.Date(totalPage$dateTPE))
    commentsCount<-aggregate(comments_count~dateTPE,totalPage,mean)
    library(knitr)
    kable(head(commentsCount[order(commentsCount$comments_count,decreasing = T),]))

|     | dateTPE    |  comments\_count|
|:----|:-----------|----------------:|
| 7   | 2016-01-10 |          5981.50|
| 6   | 2016-01-09 |          5153.25|
| 47  | 2016-03-28 |          5103.00|
| 33  | 2016-02-28 |          3565.00|
| 32  | 2016-02-27 |          3268.00|
| 9   | 2016-01-14 |          2848.00|

討論:最多受到討論留言的是1月9日與10日的「一日北高，雙城挑戰」，接著則是3月28日對於內湖發生遇害事件的回應，都超過5000則留言。

每日Shares數分析
----------------

說明:分析柯文哲粉絲團每天的Shares數，由於日期格式跟台灣不一樣，先將其轉換為台灣時區。

    totalPage$datetime <- as.POSIXct(totalPage$created_time, 
                                     format = "%Y-%m-%dT%H:%M:%S+0000", 
                                     tz = "GMT") #2016-01-16T15:05:36+0000
    totalPage$dateTPE <- format(totalPage$datetime, "%Y-%m-%d", 
                                tz = "Asia/Taipei") #2016-01-16
    totalPage$weekdays <-weekdays(as.Date(totalPage$dateTPE))
    sharesCount<-aggregate(shares_count~dateTPE,totalPage,mean)
    library(knitr)
    kable(head(sharesCount[order(sharesCount$shares_count,decreasing = T),]))

|     | dateTPE    |  shares\_count|
|:----|:-----------|--------------:|
| 9   | 2016-01-14 |        34775.0|
| 7   | 2016-01-10 |        16406.5|
| 33  | 2016-02-28 |        11235.0|
| 35  | 2016-03-04 |         5334.0|
| 47  | 2016-03-28 |         4965.0|
| 11  | 2016-01-16 |         4897.0|

討論:最多受到分享的PO文是1月14日對於「一日北高，雙城挑戰」的激勵文，以及1月10日的完成感言。

# Course Project 1
Jieqi Xue

## Loading and preprocessing the data

```r
setwd("~/Documents/R program/reproducible-1")
data <- read.csv('activity.csv',stringsAsFactors = FALSE)
data$date <- as.Date(data$date,format = '%Y-%m-%d')
```
## Mean total number of steps taken per day

```r
dailystep <- tapply(data$steps,data$date,sum,na.rm = TRUE)
hist(dailystep)
```

![plot of chunk unnamed-chunk-2](figure/unnamed-chunk-2-1.png)

```r
mean(dailystep)
```

```
## [1] 9354.23
```

```r
median(dailystep)
```

```
## [1] 10395
```
## Average daily activity pattern

```r
intervalstep <- tapply(data$steps,data$interval,mean,na.rm=TRUE)
intervaldata <- data.frame(interval=as.numeric(names(intervalstep)),intervalstep)
plot(intervaldata$interval,intervaldata$intervalstep,type='l',xlab='interval',ylab='steps')
```

![plot of chunk unnamed-chunk-3](figure/unnamed-chunk-3-1.png)

```r
intervaldata[which.max(intervaldata$intervalstep),]
```

```
##     interval intervalstep
## 835      835     206.1698
```
## Imputing missing values

Total number of missing values

```r
sum(is.na(data$steps))
```

```
## [1] 2304
```
Create a new dataset that is equal to the original dataset but with the missing data filled in

```r
newdata <- data
for (i in 1:nrow(newdata)){
     if (is.na(newdata$steps[i])){
         interval <- newdata$interval[i]
         newdata$steps[i] = intervaldata$intervalstep[intervaldata$interval == interval]
     }
}
```
New steps taken per day

```r
newdailystep <- tapply(newdata$steps,newdata$date,sum)
hist(newdailystep)
```

![plot of chunk unnamed-chunk-6](figure/unnamed-chunk-6-1.png)

```r
mean(newdailystep)
```

```
## [1] 10766.19
```

```r
median(newdailystep)
```

```
## [1] 10766.19
```
As we can see, the mean and the median both increase.

## Differences in activity patterns between weekdays and weekends

```r
library(dplyr)
```

```
## 
## Attaching package: 'dplyr'
```

```
## The following objects are masked from 'package:stats':
## 
##     filter, lag
```

```
## The following objects are masked from 'package:base':
## 
##     intersect, setdiff, setequal, union
```

```r
library(lubridate)
```

```
## 
## Attaching package: 'lubridate'
```

```
## The following object is masked from 'package:base':
## 
##     date
```

```r
library(ggplot2)
data <- mutate(data, day = ifelse(wday(date) %in% c(1,7), "weekend", "weekday"))
data$day <- as.factor(data$day)
weekdaydata <- subset(data,day == 'weekday')
weekenddata <- subset(data,day == 'weekend')
weekdayintervalstep <-tapply(weekdaydata$steps,weekdaydata$interval,mean,na.rm=TRUE)
weekdayintervaldata <-data.frame(interval=as.numeric(names(weekdayintervalstep)),weekdayintervalstep)
par(mfrow=c(2,1))
plot(weekdayintervaldata$interval,weekdayintervaldata$weekdayintervalstep,type='l',xlab='interval',ylab='steps',main ='weekday')
weekendintervalstep <-tapply(weekenddata$steps,weekenddata$interval,mean,na.rm=TRUE)
weekendintervaldata <-data.frame(interval=as.numeric(names(weekendintervalstep)),weekendintervalstep)
plot(weekendintervaldata$interval,weekendintervaldata$weekendintervalstep,type='l',xlab='interval',ylab='steps',main ='weekend')
```

![plot of chunk unnamed-chunk-7](figure/unnamed-chunk-7-1.png)

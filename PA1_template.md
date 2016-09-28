# Course Project 1
Jieqi Xue
## Loading and preprocessing the data

```r
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
plot(intervaldata$interval,intervaldata$intervalstep,type='l')
```

![plot of chunk unnamed-chunk-3](figure/unnamed-chunk-3-1.png)

```r
intervaldata[which.max(intervaldata$intervalstep),]
```

```
##     interval intervalstep
## 835      835     206.1698
```

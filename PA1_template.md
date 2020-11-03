---
title: "Reproducible Research: Peer Assessment 1"
output: 
  html_document:
    keep_md: true
---


## Loading and preprocessing the data

```r
unzip("activity.zip")
activity <- read.csv("activity.csv")
activity$date <- as.Date(activity$date)
activity_date <- aggregate(steps ~ date, data = activity, sum, na.rm = TRUE)
```
## What is mean total number of steps taken per day?

```r
graph_1 <- hist(activity_date$steps, xlab= "Number Of Steps", main = "Total Steps Per Day")
```

![](PA1_template_files/figure-html/unnamed-chunk-2-1.png)<!-- -->

```r
mean(activity_date$steps, na.rm=TRUE)
```

```
## [1] 10766.19
```
## What is the average daily activity pattern?

```r
activity_date_mean <- aggregate(steps ~ interval, data = activity, mean, na.rm = TRUE)
graph_2 <- plot(activity_date_mean$steps ~ activity_date_mean$interval, type = "l", 
                xlab = "Date", ylab = "Steps" , na.rm = TRUE)
```

```
## Warning in plot.window(...): "na.rm" is not a graphical parameter
```

```
## Warning in plot.xy(xy, type, ...): "na.rm" is not a graphical parameter
```

```
## Warning in axis(side = side, at = at, labels = labels, ...): "na.rm" is not a
## graphical parameter

## Warning in axis(side = side, at = at, labels = labels, ...): "na.rm" is not a
## graphical parameter
```

```
## Warning in box(...): "na.rm" is not a graphical parameter
```

```
## Warning in title(...): "na.rm" is not a graphical parameter
```

![](PA1_template_files/figure-html/unnamed-chunk-3-1.png)<!-- -->
## Imputing missing values

```r
sum(is.na(activity))
```

```
## [1] 2304
```
## Are there differences in activity patterns between weekdays and weekends?

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
activity <- activity %>% mutate(dayofweek = weekdays(date))
head(activity)
```

```
##   steps       date interval dayofweek
## 1    NA 2012-10-01        0    Monday
## 2    NA 2012-10-01        5    Monday
## 3    NA 2012-10-01       10    Monday
## 4    NA 2012-10-01       15    Monday
## 5    NA 2012-10-01       20    Monday
## 6    NA 2012-10-01       25    Monday
```

```r
activity$dayofweek[activity$dayofweek %in% c("Saturday","Sunday")] <- "weekend"
activity$dayofweek[activity$dayofweek != "weekend"] <- "weekday"
activity_interval <- aggregate(steps ~ interval + dayofweek, activity, mean)
library(ggplot2)
qplot(interval, steps, data = activity_interval, type = "l", geom=c("line"),
      xlab = "Interval", ylab = "Steps", main = "") +
        facet_wrap(~ dayofweek, ncol = 1)
```

```
## Warning: Ignoring unknown parameters: type
```

![](PA1_template_files/figure-html/unnamed-chunk-5-1.png)<!-- -->
Yes. There are.


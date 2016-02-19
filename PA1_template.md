# Reproducible Research
Artem Larionov  
February 11, 2016  

1. Loading and preprocessing the data

```r
activity <- read.csv('activity.csv', na.strings = 'NA', stringsAsFactors = F)
activity$date <- as.Date(as.character(activity$date), '%Y-%m-%d')
```

2. histogram of the total number of steps taken each day

```r
library(ggplot2)
steps_per_day <- aggregate(steps ~ date, activity, sum)
with(steps_per_day, hist(steps, breaks=30))
```

![](https://github.com/alarionov/RepData_PeerAssessment1/blob/master/figure/plot1-hist-total-steps.png) 

```r
dev.copy(png,'plot1-hist-total-steps.png')
```

```
## quartz_off_screen 
##                 3
```

```r
dev.off()
```

```
## quartz_off_screen 
##                 2
```

3. Mean and median number of steps taken each day

```r
mean(steps_per_day$steps)
```

```
## [1] 10766.19
```

```r
median(steps_per_day$steps)
```

```
## [1] 10765
```

4. Time series plot of the average number of steps taken

```r
steps_by_interval <- aggregate(steps ~ interval, activity, mean)
with(steps_by_interval, plot(interval, steps, type='l'))
```

![](https://github.com/alarionov/RepData_PeerAssessment1/blob/master/figure/plot2-time-series-avg-steps.png) 

```r
dev.copy(png,'plot2-time-series-avg-steps.png')
```

```
## quartz_off_screen 
##                 3
```

```r
dev.off()
```

```
## quartz_off_screen 
##                 2
```

5. Which 5-minute interval, on average across all the days in the dataset, contains the maximum number of steps?

```r
steps_by_interval$interval[which.max(steps_by_interval$steps)]
```

```
## [1] 835
```

6. Code to describe and show a strategy for imputing missing data

```r
sum(is.na(activity$steps))
```

```
## [1] 2304
```

```r
for (i in 1:nrow(activity)) {
  if (is.na(activity[i,1])) {
    activity[i,1] <- steps_by_interval[steps_by_interval$interval == activity[i,3],]$steps
  }
}
```

7. Histogram of the total number of steps taken each day after missing values are imputed

```r
steps_per_day <- aggregate(steps ~ date, activity, sum)
with(steps_per_day, hist(steps, breaks=30))
```

![](https://github.com/alarionov/RepData_PeerAssessment1/blob/master/figure/plot3-hist-total-steps-impute.png) 

```r
dev.copy(png,'plot3-hist-total-steps-impute.png')
```

```
## quartz_off_screen 
##                 3
```

```r
dev.off()
```

```
## quartz_off_screen 
##                 2
```

```r
mean(steps_per_day$steps)
```

```
## [1] 10766.19
```

```r
median(steps_per_day$steps)
```

```
## [1] 10766.19
```

 8. Panel plot comparing the average number of steps taken per 5-minute interval across weekdays and weekends

```r
activity$daytype <- weekdays(activity$date) == "Saturday" | weekdays(activity$date) == "Sunday"
activity$daytype <- factor(activity$daytype, labels = c('weekday','weekend'))
steps_by_interval <- aggregate(steps ~ interval+daytype, activity, mean)
qplot(interval, steps, data=steps_by_interval, geom = 'line', facets = daytype ~ .)
```

![](https://github.com/alarionov/RepData_PeerAssessment1/blob/master/figure/plot4-panel-avg-steps-by-daytype.png) 

```r
dev.copy(png,'plot4-panel-avg-steps-by-daytype.png')
```

```
## quartz_off_screen 
##                 3
```

```r
dev.off()
```

```
## quartz_off_screen 
##                 2
```

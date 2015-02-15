# Reproducible Research: Peer Assessment 1


## Loading and preprocessing the data

```r
data <- read.table(file="e:\\test2\\activity.csv",head=TRUE,sep=",")
```


## What is mean total number of steps taken per day?

```r
aggdata_steps_by_date <-aggregate(data$steps, by=list(data$date), FUN=sum, na.rm=TRUE)
hist(aggdata_steps_by_date$x,main= " Frequency of Total number of steps taken each day", xlab="Total number of steps taken each day")
```

![](./PA1_template_files/figure-html/unnamed-chunk-2-1.png) 

```r
print(mean(aggdata_steps_by_date$x))
```

```
## [1] 9354.23
```

```r
print(median(aggdata_steps_by_date$x))
```

```
## [1] 10395
```

## What is the average daily activity pattern?

```r
aggdata_steps_by_interval <-aggregate(data$steps, by=list(data$interval), FUN=mean, na.rm=TRUE)
plot(aggdata_steps_by_interval$Group.1,aggdata_steps_by_interval$x, type='l',xlab="Interval", ylab="Average across all days")
```

![](./PA1_template_files/figure-html/unnamed-chunk-3-1.png) 

```r
max_five_minute = which.max(aggdata_steps_by_interval$x)
p=aggdata_steps_by_interval[max_five_minute,]
print("max 5-minute interval, on average across all the days having max steps")
```

```
## [1] "max 5-minute interval, on average across all the days having max steps"
```

```r
print(p$Group.1)
```

```
## [1] 835
```


## Imputing missing values

```r
na_data <-subset(data,is.na(data))
print("total number of missing values in the dataset")
```

```
## [1] "total number of missing values in the dataset"
```

```r
print(length(na_data$steps))
```

```
## [1] 2304
```

```r
data[is.na(data),1] <- aggdata_steps_by_date[data[is.na(data),2],2]
aggdata_steps_by_date <-aggregate(data$steps, by=list(data$date), FUN=sum, na.rm=TRUE)
hist(aggdata_steps_by_date$x,main= " Frequency of Total number of steps taken each day", xlab="Total number of steps taken each day")
```

![](./PA1_template_files/figure-html/unnamed-chunk-4-1.png) 

```r
print(mean(aggdata_steps_by_date$x))
```

```
## [1] 9354.23
```

```r
print(median(aggdata_steps_by_date$x))
```

```
## [1] 10395
```

## Are there differences in activity patterns between weekdays and weekends?

```r
data[,"weekday"] <- NA
data$weekday <- weekdays(as.Date(data$date))
```

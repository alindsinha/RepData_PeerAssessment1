# Reproducible Research: Peer Assessment 1


## Loading and preprocessing the data

```r
data = read.csv("activity.csv", sep = ",", header = T)
cdata = na.omit(data)
```

## What is mean total number of steps taken per day?

```r
cavg1 = aggregate(cdata$steps, list(date = cdata$date), sum)

```

Histogram denoting the pattern is

```r
hist(cavg1$x, xlab = "steps", main = paste("Histogram of  steps per day"))
```

![plot of chunk unnamed-chunk-3](figure/unnamed-chunk-3.png) 

the mean and median total number of steps taken per day

```r
cavg1 = aggregate(cdata$steps, list(date = cdata$date), mean)
mean(cavg1$x)
```

```
## [1] 37.38
```

```r
median(cavg1$x)
```

```
## [1] 37.38
```


## What is the average daily activity pattern?
This is a time series plot which demonstarte it.

```r
cavg2 = aggregate(cdata$steps, list(interval = cdata$interval), mean)
plot(cavg2$interval, cavg2$x, type = "l")
```

![plot of chunk unnamed-chunk-5](figure/unnamed-chunk-5.png) 

5 min interval with maximum number of steps is

```r
cavg2[which.max(cavg2$x), 1]
```

```
## [1] 835
```


Total number of misisng values are

```r

sum(is.na(data$steps))
```

```
## [1] 2304
```


## Imputing missing values
inserting medain for the missing values

```r
data$steps[is.na(data$steps)] = median(data$steps, na.rm = TRUE)
```

## What is mean total number of steps taken per day(with simulated data)?

```r
avg1 = aggregate(data$steps, list(date = data$date), sum)

```

Histogram denoting the pattern is

```r
hist(avg1$x, xlab = "steps", main = paste("Histogram of  steps per day"))
```

![plot of chunk unnamed-chunk-10](figure/unnamed-chunk-10.png) 

the mean and median total number of steps taken per day(with simulated data inserted)

```r
avg1 = aggregate(data$steps, list(date = data$date), mean)
mean(avg1$x)
```

```
## [1] 32.48
```

```r
median(avg1$x)
```

```
## [1] 36.09
```


##The values now have changed,mean and median have both gone down because we have reduced the bias by filling in the simulated values
Creating a new factor variable to divide the data into weekdays and weekend

```r
daydata = transform(data, DAY = weekdays(as.Date(data$date)))
daydata = transform(daydata, cat = as.factor(ifelse(daydata$DAY %in% c("Saturday", 
    "Sunday"), "Weekend", "Weekday")))
daydata$DAY <- NULL
```

Plotting the data to identify any differnces

```r
avgf = aggregate(daydata$steps, list(interval = daydata$interval, daydata$cat), 
    mean)
library(lattice)
xyplot(x ~ interval | Group.2, data = avgf, type = "l", layout = c(1, 2))
```

![plot of chunk unnamed-chunk-13](figure/unnamed-chunk-13.png) 

## Are there differences in activity patterns between weekdays and weekends?
There is some differnce betwwen weekend and weekday in the activity pattern

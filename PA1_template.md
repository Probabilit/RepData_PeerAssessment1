---
title: "Reproducible Research: Peer Assessment 1"
output: 
  html_document:
    keep_md: true
---


## Loading and preprocessing the data

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
library(ggplot2)
unzip(zipfile = "activity.zip") 
activity <- read.csv("activity.csv")
activity$date <- as.Date(activity$date)
```
## What is mean total number of steps taken per day?

```r
daily_steps <- activity %>% group_by(date) %>%
    summarise(steps_per_day = sum(steps, na.rm = TRUE))
```

```
## `summarise()` ungrouping output (override with `.groups` argument)
```
1. Histogram: Total number of steps taken each day 

```r
hist(daily_steps$steps_per_day, main= "Histogram of Daily Steps", xlab = "Daily Steps", breaks = 20)
```

![](PA1_template_files/figure-html/unnamed-chunk-3-1.png)<!-- -->

2. **Mean** and **median** total number of steps taken per day

```r
meansteps <- mean(daily_steps$steps_per_day)
```
The mean is 9354.2295082

```r
mediansteps <- median(daily_steps$steps_per_day)
```
The median is 10395

## What is the average daily activity pattern?
1. Average steps in 5-minute interval    

```r
steps_interval <- activity %>%
        group_by(interval) %>%
        summarize(intervalsteps = mean(steps, na.rm = TRUE)) 
```

```
## `summarise()` ungrouping output (override with `.groups` argument)
```

```r
plot(steps_interval$interval, steps_interval$intervalsteps, type = "l", xlab = "5-minute Interval", ylab = "Average steps", main = "Average steps by 5-minute intervals")
```

![](PA1_template_files/figure-html/unnamed-chunk-6-1.png)<!-- -->
2. Maximum average steps in a 5-minute interval

```r
maxsteps <- max(steps_interval$intervalsteps)
```
The maximum average steps in a 5-minute interval are 206.1698113

## Imputing missing values
1. Finding the number of observations with missing values 

```r
missingvalues <- sum(is.na(activity$steps))
```
There are `r missingvalues' in the data set. 
2. Create main dataset imputing mean to NA 

```r
activity1 <- activity
activity1$steps[is.na(activity1$steps)] <- mean(activity1$steps, na.rm=TRUE)
```
3. Total number of steps df on each day after NA = mean

```r
daily_steps1 <- activity1 %>% group_by(date) %>%
    summarise(steps_per_day = sum(steps))
```

```
## `summarise()` ungrouping output (override with `.groups` argument)
```
1. Histogram: Total number of steps taken each day after imputing 

```r
hist(daily_steps1$steps_per_day, main= "Histogram of Daily Steps", xlab = "Daily Steps", breaks = 20)
```

![](PA1_template_files/figure-html/unnamed-chunk-11-1.png)<!-- -->

2. **Mean** and **median** total number of steps taken per day after imputing

```r
meansteps1 <- mean(daily_steps1$steps_per_day)
```
The mean is 1.0766189\times 10^{4}

```r
mediansteps1 <- median(daily_steps1$steps_per_day)
```
The median is 1.0766189\times 10^{4}

## Are there differences in activity patterns between weekdays and weekends?

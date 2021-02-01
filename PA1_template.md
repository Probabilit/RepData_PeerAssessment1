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
1. Make a histogram of the total number of steps taken each day

```r
hist(daily_steps$steps_per_day, main= "Histogram of Daily Steps", xlab = "Daily Steps", breaks = 20)
```

![](PA1_template_files/figure-html/unnamed-chunk-3-1.png)<!-- -->

2. Calculate and report the **mean** and **median** total number of steps taken per day

```r
mean <- mean(daily_steps$steps_per_day)
```
The mean is 9354.2295082

```r
median <- median(daily_steps$steps_per_day)
```
The median is 10395

## What is the average daily activity pattern?
 

## Imputing missing values



## Are there differences in activity patterns between weekdays and weekends?

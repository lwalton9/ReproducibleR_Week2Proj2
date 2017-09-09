------------------------------------------------------------------------

### Coursera Reproducible Research Week 2: Project 1

#### Step \#1 Read in the Data

#### Question \#1: What is the total number of steps taken per day

##### Calculate total number of steps taken per day

##### Make a Histogram of Total number of steps taken per day

##### Calculate & Report mean & median of the total number of steps taken per day

``` r
#Calculate Total Steps
TotSteps <- tapply(ActivityTable$steps, ActivityTable$date, sum)
TotSteps
```

    ## 2012-10-01 2012-10-02 2012-10-03 2012-10-04 2012-10-05 2012-10-06 
    ##         NA        126      11352      12116      13294      15420 
    ## 2012-10-07 2012-10-08 2012-10-09 2012-10-10 2012-10-11 2012-10-12 
    ##      11015         NA      12811       9900      10304      17382 
    ## 2012-10-13 2012-10-14 2012-10-15 2012-10-16 2012-10-17 2012-10-18 
    ##      12426      15098      10139      15084      13452      10056 
    ## 2012-10-19 2012-10-20 2012-10-21 2012-10-22 2012-10-23 2012-10-24 
    ##      11829      10395       8821      13460       8918       8355 
    ## 2012-10-25 2012-10-26 2012-10-27 2012-10-28 2012-10-29 2012-10-30 
    ##       2492       6778      10119      11458       5018       9819 
    ## 2012-10-31 2012-11-01 2012-11-02 2012-11-03 2012-11-04 2012-11-05 
    ##      15414         NA      10600      10571         NA      10439 
    ## 2012-11-06 2012-11-07 2012-11-08 2012-11-09 2012-11-10 2012-11-11 
    ##       8334      12883       3219         NA         NA      12608 
    ## 2012-11-12 2012-11-13 2012-11-14 2012-11-15 2012-11-16 2012-11-17 
    ##      10765       7336         NA         41       5441      14339 
    ## 2012-11-18 2012-11-19 2012-11-20 2012-11-21 2012-11-22 2012-11-23 
    ##      15110       8841       4472      12787      20427      21194 
    ## 2012-11-24 2012-11-25 2012-11-26 2012-11-27 2012-11-28 2012-11-29 
    ##      14478      11834      11162      13646      10183       7047 
    ## 2012-11-30 
    ##         NA

``` r
#Create a Histogram of Total Steps
hist(TotSteps,main="Histogram of Total Number of Steps Taken Each Day", xlab="Total Number of Steps",
col="blue")
```

![](PA1_template_files/figure-markdown_github/unnamed-chunk-2-1.png)

``` r
#Calculate Mean & Median of the total number of steps taken per day
MeanSteps <- mean(TotSteps, na.rm=TRUE)
MeanSteps
```

    ## [1] 10766.19

``` r
MedianSteps <- median(TotSteps, na.rm=TRUE)
MedianSteps
```

    ## [1] 10765

The mean of the total number of steps taken per day is 10766.19. The median of the total number of steps taken per day is 10765.

##### Questions \#2: What is the average daily activity pattern

##### Make a Time Series Plot of the 5 minute internal(x-axis) and avg number of steps taken,avg all days(y-axis)

##### Which 5 minute interval, on avg across all days, contains max number of steps?

The 5-minute interval, on average across all the days in the dataset, which contains the maximum number of steps is Interval 835.

##### Question \#3: Assessing Missing Values

###### Calculate & Report total number of rows with NAs

###### Devise a strategy for filling in all of the missing values in the dataset

###### Create a new dataset with missing data filled in

###### Make a histogram of total number of steps each day; report mean and median of total number of steps per day

###### Do these values different from estimates in Q1. What is impact of imputing missing values

The total number of mising values in the dataset is 2,304.

The mean of the total number of steps taken per day for imputed Table is 10766.19 which is the same as the mean of the Table with NAs present The median of the total number of steps taken per day is 10766.19 which is slighly higher than the median of the Table with the NAs present.

The impact of imputing missing data on the estimates of the total daily number of steps is that overall the data becomes more towards the mean.

##### Question \#4: Are there any differences in activity patterns between weekdays and weekends?

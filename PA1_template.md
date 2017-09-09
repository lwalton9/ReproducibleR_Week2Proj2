------------------------------------------------------------------------

### Coursera Reproducible Research Week 2: Project 1

#### Step \#1 Read in the Data

``` r
getwd()
```

    ## [1] "G:/LORI/2016 Role/Analytics & Data Division/Miscellaneous/Coursera R/Reproducible_Coursera5/Coursera5_wk2_Project1"

``` r
#Read in CSV File
ActivityTable <- read.csv("activity.csv", header = TRUE, na.strings="NA")
head(ActivityTable)
```

    ##   steps       date interval
    ## 1    NA 2012-10-01        0
    ## 2    NA 2012-10-01        5
    ## 3    NA 2012-10-01       10
    ## 4    NA 2012-10-01       15
    ## 5    NA 2012-10-01       20
    ## 6    NA 2012-10-01       25

``` r
str(ActivityTable)
```

    ## 'data.frame':    17568 obs. of  3 variables:
    ##  $ steps   : int  NA NA NA NA NA NA NA NA NA NA ...
    ##  $ date    : Factor w/ 61 levels "2012-10-01","2012-10-02",..: 1 1 1 1 1 1 1 1 1 1 ...
    ##  $ interval: int  0 5 10 15 20 25 30 35 40 45 ...

``` r
#change Date type to date class
ActivityTable$date <- as.Date(as.character(ActivityTable$date))

#NEEDED TO INSTALL THESE PACKAGES FOR RMARKDOWN TO WORK
#install.packages("stringi",type="win.binary")
#install.packages("rmarkdown")
```

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

``` r
#Average number of steps across all days by 5 minute intervals
MeanSteps5 <- aggregate(steps ~ interval,ActivityTable,mean)
#MeanSteps5
#Create Time Series
Steps5TimeSeries <- ts(MeanSteps5$steps)
#Steps5TimeSeries
plot.ts(Steps5TimeSeries,main="Time Series of 5-Minute Intervals and Avg Number of Steps Taken,avg all days")
```

![](PA1_template_files/figure-markdown_github/unnamed-chunk-3-1.png)

``` r
#Which 5 Minute interval contains max # of steps
MaxSteps <- max(MeanSteps5$steps)
#MaxSteps
MeanSteps5[MeanSteps5$steps == MaxSteps, ]
```

    ##     interval    steps
    ## 104      835 206.1698

The 5-minute interval, on average across all the days in the dataset, which contains the maximum number of steps is Interval 835.

##### Question \#3: Assessing Missing Values

###### Calculate & Report total number of rows with NAs

###### Devise a strategy for filling in all of the missing values in the dataset

###### Create a new dataset with missing data filled in

###### Make a histogram of total number of steps each day; report mean and median of total number of steps per day

###### Do these values different from estimates in Q1. What is impact of imputing missing values

``` r
#Number of NAs
StepsNACount <- sum(is.na(ActivityTable$steps))
StepsNACount
```

    ## [1] 2304

The total number of mising values in the dataset is 2,304.

``` r
#Impute Missing Data & Create New Dataset with missing values filled in
ActivityTable_Impute <- ActivityTable
NAdata <- is.na(ActivityTable_Impute$steps)
#NAdata
NotNAdata <- ActivityTable_Impute[!is.na(ActivityTable_Impute$steps),]
#NotNAdata
Activitymeans <- tapply(NotNAdata$steps, NotNAdata$interval, mean, na.rm=TRUE, simplify=TRUE)
#Activitymeans
ActivityTable_Impute$steps[NAdata] <- Activitymeans[as.character(ActivityTable_Impute$interval[NAdata])]
#check that NAs are filled with Data
sum(is.na(ActivityTable_Impute$steps))
```

    ## [1] 0

``` r
#Calculate Total Steps taken each day with new DataFrame
TotSteps_Impute <- tapply(ActivityTable_Impute$steps, ActivityTable_Impute$date, sum)
TotSteps_Impute
```

    ## 2012-10-01 2012-10-02 2012-10-03 2012-10-04 2012-10-05 2012-10-06 
    ##   10766.19     126.00   11352.00   12116.00   13294.00   15420.00 
    ## 2012-10-07 2012-10-08 2012-10-09 2012-10-10 2012-10-11 2012-10-12 
    ##   11015.00   10766.19   12811.00    9900.00   10304.00   17382.00 
    ## 2012-10-13 2012-10-14 2012-10-15 2012-10-16 2012-10-17 2012-10-18 
    ##   12426.00   15098.00   10139.00   15084.00   13452.00   10056.00 
    ## 2012-10-19 2012-10-20 2012-10-21 2012-10-22 2012-10-23 2012-10-24 
    ##   11829.00   10395.00    8821.00   13460.00    8918.00    8355.00 
    ## 2012-10-25 2012-10-26 2012-10-27 2012-10-28 2012-10-29 2012-10-30 
    ##    2492.00    6778.00   10119.00   11458.00    5018.00    9819.00 
    ## 2012-10-31 2012-11-01 2012-11-02 2012-11-03 2012-11-04 2012-11-05 
    ##   15414.00   10766.19   10600.00   10571.00   10766.19   10439.00 
    ## 2012-11-06 2012-11-07 2012-11-08 2012-11-09 2012-11-10 2012-11-11 
    ##    8334.00   12883.00    3219.00   10766.19   10766.19   12608.00 
    ## 2012-11-12 2012-11-13 2012-11-14 2012-11-15 2012-11-16 2012-11-17 
    ##   10765.00    7336.00   10766.19      41.00    5441.00   14339.00 
    ## 2012-11-18 2012-11-19 2012-11-20 2012-11-21 2012-11-22 2012-11-23 
    ##   15110.00    8841.00    4472.00   12787.00   20427.00   21194.00 
    ## 2012-11-24 2012-11-25 2012-11-26 2012-11-27 2012-11-28 2012-11-29 
    ##   14478.00   11834.00   11162.00   13646.00   10183.00    7047.00 
    ## 2012-11-30 
    ##   10766.19

``` r
#Create a Histogram of Total Steps
hist(TotSteps_Impute,main="Histogram of Total Number of Steps Taken Each Day-Imputed DF", xlab="Total Number of Steps",
col="blue")
```

![](PA1_template_files/figure-markdown_github/unnamed-chunk-5-1.png)

``` r
#Calculate Mean & Median of the total number of steps taken per day
MeanSteps_Impute <- mean(TotSteps_Impute, na.rm=TRUE)
MeanSteps_Impute
```

    ## [1] 10766.19

``` r
MedianSteps_Impute <- median(TotSteps_Impute, na.rm=TRUE)
MedianSteps_Impute
```

    ## [1] 10766.19

The mean of the total number of steps taken per day for imputed Table is 10766.19 which is the same as the mean of the Table with NAs present The median of the total number of steps taken per day is 10766.19 which is slighly higher than the median of the Table with the NAs present.

The impact of imputing missing data on the estimates of the total daily number of steps is that overall the data becomes more towards the mean.

##### Question \#4: Are there any differences in activity patterns between weekdays and weekends?

``` r
#Create new factor variable with two levels-weekday and weekend
ActivityTable_Impute$weekday <- weekdays(ActivityTable_Impute$date)
#ActivityTable_Impute$weekday
ActivityTable_Impute$DayWeek <- ifelse (ActivityTable_Impute$weekday == "Saturday" | ActivityTable_Impute$weekday == "Sunday", "Weekend", "Weekday")
ActivityTable_Impute$DayWeek <- as.factor(ActivityTable_Impute$DayWeek)
str(ActivityTable_Impute)
```

    ## 'data.frame':    17568 obs. of  5 variables:
    ##  $ steps   : num  1.717 0.3396 0.1321 0.1509 0.0755 ...
    ##  $ date    : Date, format: "2012-10-01" "2012-10-01" ...
    ##  $ interval: int  0 5 10 15 20 25 30 35 40 45 ...
    ##  $ weekday : chr  "Monday" "Monday" "Monday" "Monday" ...
    ##  $ DayWeek : Factor w/ 2 levels "Weekday","Weekend": 1 1 1 1 1 1 1 1 1 1 ...

``` r
#Make a Panel Plot
#install.packages("lattice")
library(lattice)
PPLOT_weekactivity <- aggregate(steps ~ DayWeek+interval, data=ActivityTable_Impute, FUN=mean)
#PPLOT_weekactivity <- aggregate(ActivityTable_Impute$steps, by=list(ActivityTable_Impute$DayWeek, #ActivityTable_Impute$interval), mean)
#PPLOT_weekactivity
#head(PPLOT_weekactivity)
#str(PPLOT_weekactivity)
xyplot(steps ~ interval | DayWeek,
       layout = c(1, 2),
       xlab="Interval",
       ylab="Number of steps",
       type="l",
       lty=1,
       data=PPLOT_weekactivity, main='Panel Plot containing Times series of Interval & Avg Steps across week')
```

![](PA1_template_files/figure-markdown_github/unnamed-chunk-6-1.png)

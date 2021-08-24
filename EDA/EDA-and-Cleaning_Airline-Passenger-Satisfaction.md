EDA and Cleaning\_Airline Passenger Satisfaction
================
4/25/2021

``` r
library("corrplot")
```

    ## Warning: package 'corrplot' was built under R version 4.0.5

    ## corrplot 0.84 loaded

``` r
library("FactoMineR")
```

    ## Warning: package 'FactoMineR' was built under R version 4.0.4

``` r
library("factoextra")
```

    ## Warning: package 'factoextra' was built under R version 4.0.4

    ## Loading required package: ggplot2

    ## Warning: package 'ggplot2' was built under R version 4.0.4

    ## Welcome! Want to learn more? See two factoextra-related books at https://goo.gl/ve3WBa

``` r
library("DataExplorer")
```

    ## Warning: package 'DataExplorer' was built under R version 4.0.5

``` r
library("compareGroups")
```

# Import Data

``` r
#read test and train
train <- read.csv("train.csv")
test <- read.csv("test.csv")

#random sample train data set, limit to 15,000 rows
set.seed(200)
train <- train[sample(nrow(train), 15000),]

#combine into one DF for non-supervised learning models
df <- rbind(train,test)

#remove id columns as they aren't needed for prediction
train <- train[,-1]
test <- test[,-1]
df <- df[,-1]

#check dimensions
dim(train)
```

    ## [1] 15000    23

``` r
dim(test)
```

    ## [1] 25976    23

``` r
dim(df)
```

    ## [1] 40976    23

# EDA

``` r
str(df)
```

    ## 'data.frame':    40976 obs. of  23 variables:
    ##  $ Gender                           : chr  "Female" "Female" "Female" "Female" ...
    ##  $ Customer.Type                    : chr  "Loyal Customer" "Loyal Customer" "disloyal Customer" "disloyal Customer" ...
    ##  $ Age                              : int  47 45 20 37 26 23 49 57 40 13 ...
    ##  $ Type.of.Travel                   : chr  "Business travel" "Business travel" "Business travel" "Business travel" ...
    ##  $ Class                            : chr  "Business" "Eco" "Eco" "Eco Plus" ...
    ##  $ Flight.Distance                  : int  1551 397 357 372 363 964 3133 374 3590 427 ...
    ##  $ Inflight.wifi.service            : int  1 4 1 2 4 4 2 4 5 4 ...
    ##  $ Departure.Arrival.time.convenient: int  1 4 0 2 0 3 2 4 5 1 ...
    ##  $ Ease.of.Online.booking           : int  1 4 1 2 4 3 2 4 5 4 ...
    ##  $ Gate.location                    : int  1 4 1 4 1 4 2 4 5 1 ...
    ##  $ Food.and.drink                   : int  2 3 5 5 4 3 3 5 5 1 ...
    ##  $ Online.boarding                  : int  5 3 1 2 4 3 5 4 4 4 ...
    ##  $ Seat.comfort                     : int  4 2 5 5 4 3 5 5 5 1 ...
    ##  $ Inflight.entertainment           : int  4 4 5 5 4 3 4 5 4 1 ...
    ##  $ On.board.service                 : int  4 4 2 4 3 1 4 5 4 4 ...
    ##  $ Leg.room.service                 : int  4 4 3 1 5 3 4 5 4 4 ...
    ##  $ Baggage.handling                 : int  4 4 4 4 5 4 4 5 4 1 ...
    ##  $ Checkin.service                  : int  5 2 1 2 5 1 5 3 5 4 ...
    ##  $ Inflight.service                 : int  4 4 1 4 4 4 4 5 4 1 ...
    ##  $ Cleanliness                      : int  3 1 5 5 4 3 5 5 5 1 ...
    ##  $ Departure.Delay.in.Minutes       : int  22 1 0 0 22 0 0 4 0 0 ...
    ##  $ Arrival.Delay.in.Minutes         : int  25 6 0 0 72 0 0 0 0 0 ...
    ##  $ satisfaction                     : chr  "satisfied" "satisfied" "neutral or dissatisfied" "neutral or dissatisfied" ...

``` r
summary(df)
```

    ##     Gender          Customer.Type           Age        Type.of.Travel    
    ##  Length:40976       Length:40976       Min.   : 7.00   Length:40976      
    ##  Class :character   Class :character   1st Qu.:27.00   Class :character  
    ##  Mode  :character   Mode  :character   Median :40.00   Mode  :character  
    ##                                        Mean   :39.67                     
    ##                                        3rd Qu.:51.00                     
    ##                                        Max.   :85.00                     
    ##                                                                          
    ##     Class           Flight.Distance Inflight.wifi.service
    ##  Length:40976       Min.   :  31    Min.   :0.000        
    ##  Class :character   1st Qu.: 413    1st Qu.:2.000        
    ##  Mode  :character   Median : 844    Median :3.000        
    ##                     Mean   :1192    Mean   :2.724        
    ##                     3rd Qu.:1744    3rd Qu.:4.000        
    ##                     Max.   :4983    Max.   :5.000        
    ##                                                          
    ##  Departure.Arrival.time.convenient Ease.of.Online.booking Gate.location  
    ##  Min.   :0.000                     Min.   :0.000          Min.   :1.000  
    ##  1st Qu.:2.000                     1st Qu.:2.000          1st Qu.:2.000  
    ##  Median :3.000                     Median :3.000          Median :3.000  
    ##  Mean   :3.046                     Mean   :2.755          Mean   :2.976  
    ##  3rd Qu.:4.000                     3rd Qu.:4.000          3rd Qu.:4.000  
    ##  Max.   :5.000                     Max.   :5.000          Max.   :5.000  
    ##                                                                          
    ##  Food.and.drink  Online.boarding  Seat.comfort   Inflight.entertainment
    ##  Min.   :0.000   Min.   :0.000   Min.   :1.000   Min.   :0.000         
    ##  1st Qu.:2.000   1st Qu.:2.000   1st Qu.:2.000   1st Qu.:2.000         
    ##  Median :3.000   Median :3.000   Median :4.000   Median :4.000         
    ##  Mean   :3.206   Mean   :3.255   Mean   :3.444   Mean   :3.352         
    ##  3rd Qu.:4.000   3rd Qu.:4.000   3rd Qu.:5.000   3rd Qu.:4.000         
    ##  Max.   :5.000   Max.   :5.000   Max.   :5.000   Max.   :5.000         
    ##                                                                        
    ##  On.board.service Leg.room.service Baggage.handling Checkin.service
    ##  Min.   :0.000    Min.   :0.000    Min.   :1.000    Min.   :1.00   
    ##  1st Qu.:2.000    1st Qu.:2.000    1st Qu.:3.000    1st Qu.:3.00   
    ##  Median :4.000    Median :4.000    Median :4.000    Median :3.00   
    ##  Mean   :3.385    Mean   :3.352    Mean   :3.633    Mean   :3.31   
    ##  3rd Qu.:4.000    3rd Qu.:4.000    3rd Qu.:5.000    3rd Qu.:4.00   
    ##  Max.   :5.000    Max.   :5.000    Max.   :5.000    Max.   :5.00   
    ##                                                                    
    ##  Inflight.service  Cleanliness   Departure.Delay.in.Minutes
    ##  Min.   :0.000    Min.   :0.00   Min.   :   0.00           
    ##  1st Qu.:3.000    1st Qu.:2.00   1st Qu.:   0.00           
    ##  Median :4.000    Median :3.00   Median :   0.00           
    ##  Mean   :3.649    Mean   :3.28   Mean   :  14.55           
    ##  3rd Qu.:5.000    3rd Qu.:4.00   3rd Qu.:  12.00           
    ##  Max.   :5.000    Max.   :5.00   Max.   :1592.00           
    ##                                                            
    ##  Arrival.Delay.in.Minutes satisfaction      
    ##  Min.   :   0             Length:40976      
    ##  1st Qu.:   0             Class :character  
    ##  Median :   0             Mode  :character  
    ##  Mean   :  15                               
    ##  3rd Qu.:  13                               
    ##  Max.   :1584                               
    ##  NA's   :120

``` r
corrplot(cor(df[,c(3,6:22)]), type='lower')
```

![](EDA-and-Cleaning_Airline-Passenger-Satisfaction_files/figure-gfm/unnamed-chunk-4-1.png)<!-- -->

``` r
plot_intro(train, ggtheme=theme_minimal())
```

![](EDA-and-Cleaning_Airline-Passenger-Satisfaction_files/figure-gfm/unnamed-chunk-5-1.png)<!-- -->

``` r
plot_histogram(train, ggtheme=theme_minimal())
```

![](EDA-and-Cleaning_Airline-Passenger-Satisfaction_files/figure-gfm/unnamed-chunk-5-2.png)<!-- -->![](EDA-and-Cleaning_Airline-Passenger-Satisfaction_files/figure-gfm/unnamed-chunk-5-3.png)<!-- -->

``` r
plot_bar(train,ggthem=theme_minimal())
```

![](EDA-and-Cleaning_Airline-Passenger-Satisfaction_files/figure-gfm/unnamed-chunk-5-4.png)<!-- -->
There are some missing values in the Arrival delay column, I will fill
these with the median value, which is 0

``` r
df$Arrival.Delay.in.Minutes[is.na(df$Arrival.Delay.in.Minutes)] <- 0 
test$Arrival.Delay.in.Minutes[is.na(test$Arrival.Delay.in.Minutes)] <- 0 
train$Arrival.Delay.in.Minutes[is.na(train$Arrival.Delay.in.Minutes)] <- 0 
```

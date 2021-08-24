airline\_passenger\_satisfaction\_code\_git
================
Sara Tuncten
4/25/2021

``` r
library("corrplot")
library("FactoMineR")
library("factoextra")
library("DataExplorer")
library("compareGroups")
```

# Import Data

``` r
#read test and train
train <- read.csv("C:/Users/ST034045/OneDrive - Cerner Corporation/Documents/Rockhurst Data Science/Marketing Research & Analysis/Final Project/train.csv")
test <- read.csv("C:/Users/ST034045/OneDrive - Cerner Corporation/Documents/Rockhurst Data Science/Marketing Research & Analysis/Final Project/test.csv")


#random sample train data set, it was too large to run on my computer, i will limit to 50,000 rows
set.seed(200)
train <- train[sample(nrow(train), 15000),]


#combine into one DF for non-supervised learning models
df <- rbind(train,test)

#remove id columns
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

![](airline_passenger_satisfaction_code_git_files/figure-gfm/unnamed-chunk-4-1.png)<!-- -->

``` r
plot_intro(train, ggtheme=theme_minimal())
```

![](airline_passenger_satisfaction_code_git_files/figure-gfm/unnamed-chunk-5-1.png)<!-- -->

``` r
plot_histogram(train, ggtheme=theme_minimal())
```

![](airline_passenger_satisfaction_code_git_files/figure-gfm/unnamed-chunk-5-2.png)<!-- -->![](airline_passenger_satisfaction_code_git_files/figure-gfm/unnamed-chunk-5-3.png)<!-- -->

``` r
plot_bar(train,ggthem=theme_minimal())
```

![](airline_passenger_satisfaction_code_git_files/figure-gfm/unnamed-chunk-5-4.png)<!-- -->
There are some missing values in the Arrival delay column, I will fill
these with the median value, which is 0

``` r
df$Arrival.Delay.in.Minutes[is.na(df$Arrival.Delay.in.Minutes)] <- 0 
test$Arrival.Delay.in.Minutes[is.na(test$Arrival.Delay.in.Minutes)] <- 0 
train$Arrival.Delay.in.Minutes[is.na(train$Arrival.Delay.in.Minutes)] <- 0 
```

# Save Processed files as new files

# Logistic Regression

``` r
logittrain <- train
logittest <- test

#convert characters to factors
logittrain[c(1,2,4,5,23)] <- lapply(logittrain[c(1,2,4,5,23)],factor)
logittest[c(1,2,4,5,23)] <- lapply(logittest[c(1,2,4,5,23)],factor)
lapply(logittrain,levels)
```

    ## $Gender
    ## [1] "Female" "Male"  
    ## 
    ## $Customer.Type
    ## [1] "disloyal Customer" "Loyal Customer"   
    ## 
    ## $Age
    ## NULL
    ## 
    ## $Type.of.Travel
    ## [1] "Business travel" "Personal Travel"
    ## 
    ## $Class
    ## [1] "Business" "Eco"      "Eco Plus"
    ## 
    ## $Flight.Distance
    ## NULL
    ## 
    ## $Inflight.wifi.service
    ## NULL
    ## 
    ## $Departure.Arrival.time.convenient
    ## NULL
    ## 
    ## $Ease.of.Online.booking
    ## NULL
    ## 
    ## $Gate.location
    ## NULL
    ## 
    ## $Food.and.drink
    ## NULL
    ## 
    ## $Online.boarding
    ## NULL
    ## 
    ## $Seat.comfort
    ## NULL
    ## 
    ## $Inflight.entertainment
    ## NULL
    ## 
    ## $On.board.service
    ## NULL
    ## 
    ## $Leg.room.service
    ## NULL
    ## 
    ## $Baggage.handling
    ## NULL
    ## 
    ## $Checkin.service
    ## NULL
    ## 
    ## $Inflight.service
    ## NULL
    ## 
    ## $Cleanliness
    ## NULL
    ## 
    ## $Departure.Delay.in.Minutes
    ## NULL
    ## 
    ## $Arrival.Delay.in.Minutes
    ## NULL
    ## 
    ## $satisfaction
    ## [1] "neutral or dissatisfied" "satisfied"

``` r
logitall <- glm(satisfaction ~ ., data = logittrain, family = "binomial")
summary(logitall)
```

    ## 
    ## Call:
    ## glm(formula = satisfaction ~ ., family = "binomial", data = logittrain)
    ## 
    ## Deviance Residuals: 
    ##     Min       1Q   Median       3Q      Max  
    ## -2.4947  -0.5060  -0.1801   0.3973   3.9174  
    ## 
    ## Coefficients:
    ##                                     Estimate Std. Error z value Pr(>|z|)    
    ## (Intercept)                       -7.995e+00  2.075e-01 -38.535  < 2e-16 ***
    ## GenderMale                         6.517e-02  5.062e-02   1.287 0.197974    
    ## Customer.TypeLoyal Customer        2.161e+00  7.938e-02  27.227  < 2e-16 ***
    ## Age                               -6.732e-03  1.854e-03  -3.632 0.000281 ***
    ## Type.of.TravelPersonal Travel     -2.763e+00  8.288e-02 -33.335  < 2e-16 ***
    ## ClassEco                          -6.425e-01  6.792e-02  -9.459  < 2e-16 ***
    ## ClassEco Plus                     -8.575e-01  1.095e-01  -7.829 4.92e-15 ***
    ## Flight.Distance                   -5.225e-05  2.931e-05  -1.783 0.074647 .  
    ## Inflight.wifi.service              4.213e-01  2.983e-02  14.123  < 2e-16 ***
    ## Departure.Arrival.time.convenient -1.465e-01  2.099e-02  -6.979 2.98e-12 ***
    ## Ease.of.Online.booking            -1.593e-01  2.939e-02  -5.421 5.93e-08 ***
    ## Gate.location                      5.383e-02  2.387e-02   2.255 0.024146 *  
    ## Food.and.drink                     2.188e-02  2.821e-02   0.776 0.437847    
    ## Online.boarding                    5.484e-01  2.657e-02  20.640  < 2e-16 ***
    ## Seat.comfort                       6.117e-02  2.957e-02   2.068 0.038597 *  
    ## Inflight.entertainment             4.215e-03  3.703e-02   0.114 0.909385    
    ## On.board.service                   2.744e-01  2.640e-02  10.393  < 2e-16 ***
    ## Leg.room.service                   2.603e-01  2.209e-02  11.788  < 2e-16 ***
    ## Baggage.handling                   1.357e-01  2.944e-02   4.609 4.04e-06 ***
    ## Checkin.service                    3.511e-01  2.234e-02  15.711  < 2e-16 ***
    ## Inflight.service                   1.705e-01  3.137e-02   5.435 5.48e-08 ***
    ## Cleanliness                        2.413e-01  3.178e-02   7.594 3.09e-14 ***
    ## Departure.Delay.in.Minutes        -3.450e-04  2.293e-03  -0.150 0.880443    
    ## Arrival.Delay.in.Minutes          -5.346e-03  2.276e-03  -2.349 0.018801 *  
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ## 
    ## (Dispersion parameter for binomial family taken to be 1)
    ## 
    ##     Null deviance: 20566  on 14999  degrees of freedom
    ## Residual deviance: 10236  on 14976  degrees of freedom
    ## AIC: 10284
    ## 
    ## Number of Fisher Scoring iterations: 5

Statistically significant variables: Customer Type, Age, Type of Travel,
Class, Inflight WiFi Service, Departure/Arrival Time Convenient, Ease of
Online Booking, Online Boarding, Leg Room Service, Baggage Handling,
Check In Service, Inflight Service, Cleanliness, Arrival Delay.

All else being equal, a one unit increase in age will make a passenger
less likely to be satisfied. Customers who rated online boarding higher
are more likely to be satisfied. Customers who traveled for business
instead of personal reasons are more likely to be satisfied. Customers
who traveled economy or economy plus are less likely to be satisfied
than those who travel business class Loyal customers are more likely to
be satisfied than disloyal customers

``` r
#odds ratios
print("Odds Ratio")
```

    ## [1] "Odds Ratio"

``` r
exp(coef(logitall))
```

    ##                       (Intercept)                        GenderMale 
    ##                       0.000337202                       1.067337025 
    ##       Customer.TypeLoyal Customer                               Age 
    ##                       8.682428492                       0.993290453 
    ##     Type.of.TravelPersonal Travel                          ClassEco 
    ##                       0.063123369                       0.525992249 
    ##                     ClassEco Plus                   Flight.Distance 
    ##                       0.424212338                       0.999947750 
    ##             Inflight.wifi.service Departure.Arrival.time.convenient 
    ##                       1.523907885                       0.863727164 
    ##            Ease.of.Online.booking                     Gate.location 
    ##                       0.852743872                       1.055301018 
    ##                    Food.and.drink                   Online.boarding 
    ##                       1.022124952                       1.730427844 
    ##                      Seat.comfort            Inflight.entertainment 
    ##                       1.063075936                       1.004223552 
    ##                  On.board.service                  Leg.room.service 
    ##                       1.315735735                       1.297379049 
    ##                  Baggage.handling                   Checkin.service 
    ##                       1.145311257                       1.420562102 
    ##                  Inflight.service                       Cleanliness 
    ##                       1.185858975                       1.272919552 
    ##        Departure.Delay.in.Minutes          Arrival.Delay.in.Minutes 
    ##                       0.999655109                       0.994667902

A one unit increase in rating of online boarding will make customer more
likely to be satisfied A male passenger is likely to be satisfied than
female. Odds of a loyal customer being satisfied are more than 8 times
greater than a disloyal customer being satisfied One unit increase in
departure time, ease of online booking actually lowered chances of being
satisfied. One unit increase in online boarding rating and inflight wifi
rating increased chances of satisfaction.

Predict Results with Test Data

``` r
#check accuracy of model
logittest$pred <- predict(logitall,logittest, type="response")


logittest$pred1[logittest$pred <0.5]<-'neutral or dissatisfied'
logittest$pred1[logittest$pred >=0.5]<-'satisfied'

tb1<-table(logittest$pred1, logittest$satisfaction)
tb1
```

    ##                          
    ##                           neutral or dissatisfied satisfied
    ##   neutral or dissatisfied                   13077      1882
    ##   satisfied                                  1496      9521

``` r
mean(logittest$pred1 == logittest$satisfaction)
```

    ## [1] 0.8699569

87% accurate with all variables

# Stepwise Logistic Regression

``` r
library(MASS)
```

    ## Warning: package 'MASS' was built under R version 4.0.5

``` r
step.model <- stepAIC(logitall, trace = FALSE)
summary(step.model)
```

    ## 
    ## Call:
    ## glm(formula = satisfaction ~ Customer.Type + Age + Type.of.Travel + 
    ##     Class + Flight.Distance + Inflight.wifi.service + Departure.Arrival.time.convenient + 
    ##     Ease.of.Online.booking + Gate.location + Online.boarding + 
    ##     Seat.comfort + On.board.service + Leg.room.service + Baggage.handling + 
    ##     Checkin.service + Inflight.service + Cleanliness + Arrival.Delay.in.Minutes, 
    ##     family = "binomial", data = logittrain)
    ## 
    ## Deviance Residuals: 
    ##     Min       1Q   Median       3Q      Max  
    ## -2.4974  -0.5073  -0.1807   0.3973   3.8961  
    ## 
    ## Coefficients:
    ##                                     Estimate Std. Error z value Pr(>|z|)    
    ## (Intercept)                       -7.945e+00  1.958e-01 -40.584  < 2e-16 ***
    ## Customer.TypeLoyal Customer        2.168e+00  7.796e-02  27.802  < 2e-16 ***
    ## Age                               -6.824e-03  1.848e-03  -3.692 0.000222 ***
    ## Type.of.TravelPersonal Travel     -2.767e+00  8.177e-02 -33.837  < 2e-16 ***
    ## ClassEco                          -6.412e-01  6.764e-02  -9.479  < 2e-16 ***
    ## ClassEco Plus                     -8.547e-01  1.093e-01  -7.819 5.33e-15 ***
    ## Flight.Distance                   -5.264e-05  2.930e-05  -1.796 0.072437 .  
    ## Inflight.wifi.service              4.237e-01  2.946e-02  14.379  < 2e-16 ***
    ## Departure.Arrival.time.convenient -1.468e-01  2.096e-02  -7.006 2.45e-12 ***
    ## Ease.of.Online.booking            -1.588e-01  2.933e-02  -5.415 6.11e-08 ***
    ## Gate.location                      5.360e-02  2.384e-02   2.248 0.024556 *  
    ## Online.boarding                    5.456e-01  2.637e-02  20.689  < 2e-16 ***
    ## Seat.comfort                       6.796e-02  2.751e-02   2.471 0.013479 *  
    ## On.board.service                   2.737e-01  2.551e-02  10.727  < 2e-16 ***
    ## Leg.room.service                   2.607e-01  2.191e-02  11.899  < 2e-16 ***
    ## Baggage.handling                   1.362e-01  2.897e-02   4.701 2.59e-06 ***
    ## Checkin.service                    3.498e-01  2.200e-02  15.899  < 2e-16 ***
    ## Inflight.service                   1.720e-01  3.015e-02   5.703 1.18e-08 ***
    ## Cleanliness                        2.548e-01  2.644e-02   9.638  < 2e-16 ***
    ## Arrival.Delay.in.Minutes          -5.706e-03  6.948e-04  -8.213  < 2e-16 ***
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ## 
    ## (Dispersion parameter for binomial family taken to be 1)
    ## 
    ##     Null deviance: 20566  on 14999  degrees of freedom
    ## Residual deviance: 10238  on 14980  degrees of freedom
    ## AIC: 10278
    ## 
    ## Number of Fisher Scoring iterations: 5

``` r
print("Odds Ratio")
```

    ## [1] "Odds Ratio"

``` r
exp(coef(step.model))
```

    ##                       (Intercept)       Customer.TypeLoyal Customer 
    ##                      0.0003544879                      8.7364382492 
    ##                               Age     Type.of.TravelPersonal Travel 
    ##                      0.9931990868                      0.0628564435 
    ##                          ClassEco                     ClassEco Plus 
    ##                      0.5266796548                      0.4254134629 
    ##                   Flight.Distance             Inflight.wifi.service 
    ##                      0.9999473662                      1.5275311651 
    ## Departure.Arrival.time.convenient            Ease.of.Online.booking 
    ##                      0.8634434507                      0.8531299121 
    ##                     Gate.location                   Online.boarding 
    ##                      1.0550644275                      1.7256367035 
    ##                      Seat.comfort                  On.board.service 
    ##                      1.0703264375                      1.3148194103 
    ##                  Leg.room.service                  Baggage.handling 
    ##                      1.2978434252                      1.1459087556 
    ##                   Checkin.service                  Inflight.service 
    ##                      1.4188503724                      1.1876199499 
    ##                       Cleanliness          Arrival.Delay.in.Minutes 
    ##                      1.2902559593                      0.9943099868

One mile increase in flight distance means a passenger will be less
likely to be satisfied. One unit increase in ease of online booking and
the convenient departure/ arrival time rating will make a customer less
likely to be satisfied One year increase in age will make them slightly
less likely to be satisfied.

Highest effect on satisfaction: 1 unit increase in online boarding, in
flight wifi, check in service, cleanliness, leg room, and on board
service

``` r
#multicollinearity
library(car)
```

    ## Warning: package 'car' was built under R version 4.0.4

    ## Loading required package: carData

    ## Warning: package 'carData' was built under R version 4.0.3

``` r
vif(step.model)
```

    ##                                       GVIF Df GVIF^(1/(2*Df))
    ## Customer.Type                     1.609230  1        1.268554
    ## Age                               1.174203  1        1.083606
    ## Type.of.Travel                    1.916854  1        1.384505
    ## Class                             1.740641  2        1.148622
    ## Flight.Distance                   1.371952  1        1.171304
    ## Inflight.wifi.service             2.188711  1        1.479429
    ## Departure.Arrival.time.convenient 1.695058  1        1.301944
    ## Ease.of.Online.booking            2.589555  1        1.609210
    ## Gate.location                     1.534922  1        1.238920
    ## Online.boarding                   1.506951  1        1.227579
    ## Seat.comfort                      1.849025  1        1.359789
    ## On.board.service                  1.524732  1        1.234800
    ## Leg.room.service                  1.180322  1        1.086426
    ## Baggage.handling                  1.756129  1        1.325190
    ## Checkin.service                   1.182303  1        1.087337
    ## Inflight.service                  1.864473  1        1.365457
    ## Cleanliness                       1.752288  1        1.323740
    ## Arrival.Delay.in.Minutes          1.016950  1        1.008439

\#Stepwise Predict

``` r
#clear results from first prediction
logittest <- logittest[,-c(24,25)]

#check accuracy of model
logittest$pred <- predict(step.model,logittest, type="response")


logittest$pred1[logittest$pred <0.5]<-'neutral or dissatisfied'
logittest$pred1[logittest$pred >=0.5]<-'satisfied'

tb1<-table(logittest$pred1, logittest$satisfaction)
tb1
```

    ##                          
    ##                           neutral or dissatisfied satisfied
    ##   neutral or dissatisfied                   13075      1885
    ##   satisfied                                  1498      9518

``` r
mean(logittest$pred1 == logittest$satisfaction)
```

    ## [1] 0.8697644

Almost the same accuracy, fewer variables, easier to interpret with
fewer variables. I Will recommend this model over logistic regression
with all variables.

Now that we have an idea of what is linked to satisfaction, I’ll see how
this compares to a decision tree model. I’ll later try to make clusters
and see how those clusters compare against these more important
variables

# Decision Tree

``` r
dttrain <- train
dttest <- test

#convert characters to factors
dttrain[c(1,2,4,5,23)] <- lapply(dttrain[c(1,2,4,5,23)],factor)
dttest[c(1,2,4,5,23)] <- lapply(dttest[c(1,2,4,5,23)],factor)
str(dttrain)
```

    ## 'data.frame':    15000 obs. of  23 variables:
    ##  $ Gender                           : Factor w/ 2 levels "Female","Male": 1 1 1 1 1 1 1 2 1 1 ...
    ##  $ Customer.Type                    : Factor w/ 2 levels "disloyal Customer",..: 2 2 1 1 1 1 2 2 2 2 ...
    ##  $ Age                              : int  47 45 20 37 26 23 49 57 40 13 ...
    ##  $ Type.of.Travel                   : Factor w/ 2 levels "Business travel",..: 1 1 1 1 1 1 1 2 1 2 ...
    ##  $ Class                            : Factor w/ 3 levels "Business","Eco",..: 1 2 2 3 1 2 1 2 1 2 ...
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
    ##  $ Arrival.Delay.in.Minutes         : num  25 6 0 0 72 0 0 0 0 0 ...
    ##  $ satisfaction                     : Factor w/ 2 levels "neutral or dissatisfied",..: 2 2 1 1 2 1 2 1 2 1 ...

``` r
library(rpart)
satsif.rp <- rpart(satisfaction ~ ., data=dttrain)

printcp(satsif.rp)
```

    ## 
    ## Classification tree:
    ## rpart(formula = satisfaction ~ ., data = dttrain)
    ## 
    ## Variables actually used in tree construction:
    ## [1] Inflight.wifi.service Online.boarding       Type.of.Travel       
    ## 
    ## Root node error: 6575/15000 = 0.43833
    ## 
    ## n= 15000 
    ## 
    ##         CP nsplit rel error  xerror      xstd
    ## 1 0.501141      0   1.00000 1.00000 0.0092425
    ## 2 0.126844      1   0.49886 0.49886 0.0076994
    ## 3 0.040456      2   0.37202 0.37202 0.0068814
    ## 4 0.025399      4   0.29110 0.29110 0.0062149
    ## 5 0.010000      5   0.26570 0.26570 0.0059753

The decision tree model is only using inlfight wifi service, online
boarding, and type of travel. We are starting to see similiarites
between the importance of inflight wifi service and online boarding
ratings in both logistic regression and now the decision tree models.

``` r
plotcp(satsif.rp)
```

![](airline_passenger_satisfaction_code_git_files/figure-gfm/unnamed-chunk-17-1.png)<!-- -->

``` r
library(rpart.plot)
```

    ## Warning: package 'rpart.plot' was built under R version 4.0.5

``` r
rpart.plot(satsif.rp, box.palette = "RdBu",shadow.col="gray", nn=TRUE)
```

![](airline_passenger_satisfaction_code_git_files/figure-gfm/unnamed-chunk-18-1.png)<!-- -->
The first split on the tree is whether or not a passenger’s online
boarding rating was less than or greater than 4 out of 5.

If their rating was greater than 4 and they were not a personal traveler
(so they were a business traveler), the model predicts they will be a
satisfied customer.

If their rating was greater than 4 and they are a personal traveler, and
their inflight wifi service was less than 5, they will be dissatisfied.
But if their inflight wifi service rating was 5, they will be satisfied.

``` r
#First, we can use the predict function to generate a predicted variable (outcome) in the test dataset (we have already created)
dttest$predictions <- predict(satsif.rp, dttest, type="class")

#the newly added variable.
fix(dttest)
#classification table (Confusion Matrix) + accuracy
table(dttest$satisfaction, dttest$predictions)
```

    ##                          
    ##                           neutral or dissatisfied satisfied
    ##   neutral or dissatisfied                   12599      1974
    ##   satisfied                                  1045     10358

``` r
mean(dttest$satisfaction == dttest$predictions)
```

    ## [1] 0.8837773

88.4% accurate, slightly more accurate than logistic regression, uses
fewer variables. Let’s try random forest and see how this affects
accuracy

# Decision Tree - Random Forest

``` r
rftrain <- train
rftest <- test

#convert characters to factors
rftrain[c(1,2,4,5,23)] <- lapply(rftrain[c(1,2,4,5,23)],factor)
rftest[c(1,2,4,5,23)] <- lapply(rftest[c(1,2,4,5,23)],factor)

library(randomForest)
```

    ## Warning: package 'randomForest' was built under R version 4.0.5

    ## randomForest 4.6-14

    ## Type rfNews() to see new features/changes/bug fixes.

    ## 
    ## Attaching package: 'randomForest'

    ## The following object is masked from 'package:ggplot2':
    ## 
    ##     margin

``` r
satis.rf <- randomForest(satisfaction ~ ., data = rftrain, importance = T)
satis.rf
```

    ## 
    ## Call:
    ##  randomForest(formula = satisfaction ~ ., data = rftrain, importance = T) 
    ##                Type of random forest: classification
    ##                      Number of trees: 500
    ## No. of variables tried at each split: 4
    ## 
    ##         OOB estimate of  error rate: 4.87%
    ## Confusion matrix:
    ##                         neutral or dissatisfied satisfied class.error
    ## neutral or dissatisfied                    8160       265  0.03145401
    ## satisfied                                   466      6109  0.07087452

``` r
satis.predction <- predict(satis.rf, rftest)

#Similarly, we can get the accuracy of the prediction
table(satis.predction, rftest$satisfaction)
```

    ##                          
    ## satis.predction           neutral or dissatisfied satisfied
    ##   neutral or dissatisfied                   14134       668
    ##   satisfied                                   439     10735

``` r
mean(rftest$satisfaction == satis.predction)
```

    ## [1] 0.9573837

``` r
plot(satis.rf)
```

![](airline_passenger_satisfaction_code_git_files/figure-gfm/unnamed-chunk-22-1.png)<!-- -->

``` r
#We can also examine the importance of each attribute 
varImpPlot(satis.rf)
```

![](airline_passenger_satisfaction_code_git_files/figure-gfm/unnamed-chunk-22-2.png)<!-- -->
Random forest is much more accurate than decision tree.

# Clustering - PAM with only demographic data

``` r
#clustering with mixed data
pamdem2 <- train[,c(1:6,23)]
pamdem <- train[,c(1:6,23)]
colnames(pamdem)
```

    ## [1] "Gender"          "Customer.Type"   "Age"             "Type.of.Travel" 
    ## [5] "Class"           "Flight.Distance" "satisfaction"

``` r
#convert characters to factors
pamdem[c(1,2,4,5)] <- lapply(pamdem[c(1,2,4,5)],factor)

#remove outcome variable
pamdem2 <- pamdem[,-7]

library(cluster)
#calculate distance for mixed data set with daisy function
disMat = daisy(pamdem2, metric="gower")

#find best number of clusters
sil_width <- c(NA)
for(i in 2:10){  
  pam_fit <- pam(disMat, diss = TRUE, k = i)  
  sil_width[i] <- pam_fit$silinfo$avg.width  
}

plot(1:10, sil_width,
     xlab = "Number of clusters",
     ylab = "Silhouette Width")
lines(1:10, sil_width)
```

![](airline_passenger_satisfaction_code_git_files/figure-gfm/unnamed-chunk-23-1.png)<!-- -->
Run pam with 5 clusters. Silhouette plot suggests 10 clusters, but this
will be too many to work with. I will do 5.

``` r
set.seed(200)
pamFit <- pam(disMat, k=5)
table(pamFit$clustering)
```

    ## 
    ##    1    2    3    4    5 
    ## 3364 2738 2184 2881 3833

``` r
pamdf5<-as.data.frame(cbind(pamdem, pamFit$clustering))
colnames(pamdf5)[8]<-"clusters"

group <- compareGroups(clusters~., data=pamdf5)

clustab <- createTable(group, digits=3, show.p.overall = FALSE)
clustab
```

    ## 
    ## --------Summary descriptives table by 'clusters'---------
    ## 
    ## _________________________________________________________________________________________________________________________ 
    ##                                      1                  2                 3                 4                  5          
    ##                                   N=3364             N=2738            N=2184            N=2881             N=3833        
    ## ¯¯¯¯¯¯¯¯¯¯¯¯¯¯¯¯¯¯¯¯¯¯¯¯¯¯¯¯¯¯¯¯¯¯¯¯¯¯¯¯¯¯¯¯¯¯¯¯¯¯¯¯¯¯¯¯¯¯¯¯¯¯¯¯¯¯¯¯¯¯¯¯¯¯¯¯¯¯¯¯¯¯¯¯¯¯¯¯¯¯¯¯¯¯¯¯¯¯¯¯¯¯¯¯¯¯¯¯¯¯¯¯¯¯¯¯¯¯¯¯¯ 
    ## Gender:                                                                                                                   
    ##     Female                    3364 (100.000%)    2738 (100.000%)   1483 (67.903%)      0 (0.000%)         0 (0.000%)      
    ##     Male                        0 (0.000%)         0 (0.000%)       701 (32.097%)    2881 (100.000%)    3833 (100.000%)   
    ## Customer.Type:                                                                                                            
    ##     disloyal Customer          115 (3.419%)        2 (0.073%)      2072 (94.872%)      12 (0.417%)       507 (13.227%)    
    ##     Loyal Customer            3249 (96.581%)     2736 (99.927%)     112 (5.128%)     2869 (99.583%)     3326 (86.773%)    
    ## Age                           43.650 (12.110)    40.291 (17.576)   28.894 (10.350)   40.059 (17.790)    41.908 (12.620)   
    ## Type.of.Travel:                                                                                                           
    ##     Business travel           3341 (99.316%)      451 (16.472%)    2174 (99.542%)     567 (19.681%)     3800 (99.139%)    
    ##     Personal Travel             23 (0.684%)      2287 (83.528%)      10 (0.458%)     2314 (80.319%)       33 (0.861%)     
    ## Class:                                                                                                                    
    ##     Business                  3088 (91.795%)      117 (4.273%)      427 (19.551%)      97 (3.367%)      3437 (89.669%)    
    ##     Eco                         82 (2.438%)      2374 (86.706%)    1652 (75.641%)    2518 (87.400%)      136 (3.548%)     
    ##     Eco Plus                   194 (5.767%)       247 (9.021%)      105 (4.808%)      266 (9.233%)       260 (6.783%)     
    ## Flight.Distance             1785.114 (1128.038) 735.846 (567.767) 669.326 (438.805) 707.465 (536.669) 1644.064 (1119.086) 
    ## satisfaction:                                                                                                             
    ##     neutral or dissatisfied    947 (28.151%)     2250 (82.177%)    1724 (78.938%)    2345 (81.395%)     1159 (30.237%)    
    ##     satisfied                 2417 (71.849%)      488 (17.823%)     460 (21.062%)     536 (18.605%)     2674 (69.763%)    
    ## ¯¯¯¯¯¯¯¯¯¯¯¯¯¯¯¯¯¯¯¯¯¯¯¯¯¯¯¯¯¯¯¯¯¯¯¯¯¯¯¯¯¯¯¯¯¯¯¯¯¯¯¯¯¯¯¯¯¯¯¯¯¯¯¯¯¯¯¯¯¯¯¯¯¯¯¯¯¯¯¯¯¯¯¯¯¯¯¯¯¯¯¯¯¯¯¯¯¯¯¯¯¯¯¯¯¯¯¯¯¯¯¯¯¯¯¯¯¯¯¯¯

# Plot clusters against variables

``` r
#graph outcomes
#clusters by satisfaction
satistable <- table(pamdf5$satisfaction, pamdf5$clusters)
satistable
```

    ##                          
    ##                              1    2    3    4    5
    ##   neutral or dissatisfied  947 2250 1724 2345 1159
    ##   satisfied               2417  488  460  536 2674

``` r
satisprop <- prop.table(satistable,2)
satisprop
```

    ##                          
    ##                                   1         2         3         4         5
    ##   neutral or dissatisfied 0.2815101 0.8217677 0.7893773 0.8139535 0.3023741
    ##   satisfied               0.7184899 0.1782323 0.2106227 0.1860465 0.6976259

``` r
#We visually show the means of the clusters for numeric variables
barplot(satisprop,beside=TRUE,col=c("red", "#2E9FDF"), 
        main = "Customer Satisfaction by Cluster", legend.text = T, args.legend = list(bty="n"))
```

![](airline_passenger_satisfaction_code_git_files/figure-gfm/unnamed-chunk-25-1.png)<!-- -->

``` r
#add ratings to cluster data set
clustcomp <- as.data.frame(cbind(train, pamdf5[,8]))
colnames(clustcomp)[24]<-"clusters" 

#clusters by in flight wifi ratings
wifitable <- table(clustcomp$Inflight.wifi.service, clustcomp$clusters)
wifitable
```

    ##    
    ##       1   2   3   4   5
    ##   0  96  46  93  58 152
    ##   1 640 487 342 501 672
    ##   2 733 796 576 782 861
    ##   3 763 722 560 782 793
    ##   4 645 505 440 544 761
    ##   5 487 182 173 214 594

``` r
wifiprop <- prop.table(wifitable,2)
wifiprop
```

    ##    
    ##              1          2          3          4          5
    ##   0 0.02853746 0.01680058 0.04258242 0.02013190 0.03965562
    ##   1 0.19024970 0.17786706 0.15659341 0.17389795 0.17531959
    ##   2 0.21789536 0.29072316 0.26373626 0.27143353 0.22462823
    ##   3 0.22681332 0.26369613 0.25641026 0.27143353 0.20688756
    ##   4 0.19173603 0.18444120 0.20146520 0.18882333 0.19853900
    ##   5 0.14476813 0.06647188 0.07921245 0.07427976 0.15497000

``` r
#We visually show the means of the clusters for numeric variables
barplot(wifiprop,beside=TRUE,col=c( "#DCDCDC", "#FF0000", "#CD5C5C", "#FFA07A", "#00BFFF", "#0000FF"), 
        main = "Inflight Wifi Ratings by Cluster", legend.text = T, args.legend = list(bty="n"))
```

![](airline_passenger_satisfaction_code_git_files/figure-gfm/unnamed-chunk-25-2.png)<!-- -->

``` r
#clusters by online boarding ratings
boardingtable <- table(clustcomp$Online.boarding, clustcomp$clusters)
boardingtable
```

    ##    
    ##        1    2    3    4    5
    ##   0   10   68  107  137   49
    ##   1  165  327  344  481  240
    ##   2  328  508  532  726  409
    ##   3  587  671  569  699  582
    ##   4 1288  775  458  564 1453
    ##   5  986  389  174  274 1100

``` r
boardingprop <- prop.table(boardingtable,2)
boardingprop
```

    ##    
    ##               1           2           3           4           5
    ##   0 0.002972652 0.024835646 0.048992674 0.047552933 0.012783720
    ##   1 0.049048751 0.119430241 0.157509158 0.166955918 0.062614140
    ##   2 0.097502973 0.185536888 0.243589744 0.251995835 0.106704931
    ##   3 0.174494649 0.245069394 0.260531136 0.242624089 0.151839290
    ##   4 0.382877527 0.283053324 0.209706960 0.195765359 0.379076441
    ##   5 0.293103448 0.142074507 0.079670330 0.095105866 0.286981477

``` r
#We visually show the means of the clusters for numeric variables
barplot(wifiprop,beside=TRUE,col=c( "#DCDCDC", "#FF0000", "#CD5C5C", "#FFA07A", "#00BFFF", "#0000FF"), 
        main = "Online Boarding Ratings by Cluster", legend.text = T, args.legend = list(bty="n"))
```

![](airline_passenger_satisfaction_code_git_files/figure-gfm/unnamed-chunk-25-3.png)<!-- -->

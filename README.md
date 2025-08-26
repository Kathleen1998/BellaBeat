The business task was to seek information from trends on similar devices, and what can inform us on how to market and influnence non Bellabeat consumer

All data in this project is sourced by [Fitbit Fitness Tracker Data]{https://www.kaggle.com/datasets/arashnic/fitbit} on Kaggle 
        which had been allocated from [Zenodo]{https://zenodo.org/records/53894#.X9oeh3Uzaao}

The source is accurate, complete, and unbiased information that has been vetted and proven fit for use
The source is orginal the data is copied from Zenodo exactly
That data isnt missing important information needed to answer the question or find the solution
The data is not current it is between the months of March through May
The data is well citied linking back to the original source of the data


Cleaning/ manipulation of data  

``` ##Remove duplicate entries// COUNTED THE AMOUNT OF DUPLICATES THEN REMOVED 
df %>%
  distinct(.keep_all = TRUE)

nrow(Activity[duplicated(Activity), ]) ## 0

nrow(HourlyCalories[duplicated(HourlyCalories), ]) ## 175


nrow(HourlyIntensities[duplicated(HourlyIntensities), ]) ## 175
HourlyIntensities %>%
  group_by_all() %>%
  filter(n()>1) %>%
  ungroup()

nrow(Sleep[duplicated(Sleep), ]) ## 4300


nrow(SleepDay[duplicated(SleepDay), ]) ## 3
SleepDay %>%
  group_by_all() %>%
  filter(n()>1) %>%
  ungroup()

SleepDay[!duplicated(SleepDay), ]

SleepDay <- SleepDay[-which(duplicated(SleepDay)), ] ## worked

SleepDay %>%
  distinct(.keep_all = TRUE)

nrow(HourlySteps[duplicated(HourlySteps), ]) ## 175
HourlySteps %>%
  group_by_all() %>%
  filter(n()>1) %>%
  ungroup()

nrow(MinuteIntensities[duplicated(MinuteIntensities), ]) ## 1050

##Different amount of users
userAmount <- n_distinct(Activity$Id)

userAmount2 <- n_distinct(HourlyCalories$Id)

userAmount3 <- n_distinct(HourlyIntensities$Id)

userAmount4 <- n_distinct(HourlySteps$Id)

userAmount5 <- n_distinct(Sleep$Id)### 5 has 25 users

userAmount6 <- n_distinct(SleepDay$Id)### 6 has 24 users

userAmount7 <- n_distinct(MinuteIntensities$Id)


##CHECKING THE INTERVAL OF TIME NO DATE SHOULD BE BEFORE 3/12 OR AFTER 5/12; IF SO REMOVED

startinterval <- '03/12/2016 00:00:00'
endinterval <- '05/12/2016 23:59:59'


startinterval1 <- mdy_hms(startinterval)
endinterval2 <- mdy_hms(endinterval)

as.POSIXct(endinterval,format="%m/%d/%Y %H:%M:%S",tz=Sys.timezone())
```

The datas statment piece mentions the data gathered os only from 03.12.2016-05.12.2016; 2 months of data gathered and only in the spring time.
This data does have some limited resourse in only providing information for 2 months and only in the beginig of the second quarter of the year; spring.

The data is oraganized by user and order by date. It is in a long format; Long data is data where each row contains a single data point for a particular item

The only bias is that the data is from March to May which is when the weather becomes warmer and people start to becomemore active than when the months were November through February


 Analyzing

 ```
## Step v. Calories
ggplot(Activity, aes(x = TotalSteps, y = Calories)) +
  geom_point() +
  geom_smooth(method = "lm", linetype = "solid", color = "pink")
```
<img width="1151" height="687" alt="image" src="https://github.com/user-attachments/assets/5aea0aab-447a-4c8b-8092-6171713f9627" />



























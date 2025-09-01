The business task was to seek information from trends on similar devices, and what can inform us on how to market and influnence non Bellabeat consumer

All data in this project is sourced by [Fitbit Fitness Tracker Data]{https://www.kaggle.com/datasets/arashnic/fitbit} on Kaggle which had been allocated from [Zenodo]{https://zenodo.org/records/53894#.X9oeh3Uzaao}

The source is accurate, complete, and unbiased information that has been vetted and proven fit for use The source is original the data is copied from Zenodo exactly That data isn't missing important information needed to answer the question or find the solution The data is not current it is between the months of March through May The data is well cited linking back to the original source of the data



```
install.packages("tidyverse")
install.packages("ggplot2")
install.packages("dplyr")
install.packages("tidyr")
install.packages("rgl")
install.packages("readr")
install.packages("here")
install.packages("skimr")
install.packages("Janitor")
install.packages("lubridate")
install.packages("chron")
install.packages('magrittr')
install.packages('patchwork')
install.packages('hms')
install.packages('corrplot')
library(corrplot)
library(lubridate)
library(tidyverse)
library(ggplot2)
library(dplyr)
library(tidyr)
library(rgl)
library(readr)
library(here)
library(skimr)
library(janitor)
library(chron)
library(patchwork)
library(hms)

```


Creating tables 
```
##March to April STEP 1 reading the data and put it in an object
activityMarch <-  read_csv("mturkfitbit_export_3.12.16-4.11.16/Fitabase Data 3.12.16-4.11.16/dailyActivity_merged.csv")
View(activityMarch)

caloriesHourlyMarch <-  read_csv("mturkfitbit_export_3.12.16-4.11.16/Fitabase Data 3.12.16-4.11.16/hourlyCalories_merged.csv")
View(caloriesHourlyMarch)

intensityHourlyMarch <-  read_csv("mturkfitbit_export_3.12.16-4.11.16/Fitabase Data 3.12.16-4.11.16/hourlyIntensities_merged.csv")
View(intensityHourlyMarch)

stepshourlyMarch <-  read_csv("mturkfitbit_export_3.12.16-4.11.16/Fitabase Data 3.12.16-4.11.16/hourlySteps_merged.csv")
View(stepshourlyMarch)

intensitiesMinuteMarch <-  read_csv("mturkfitbit_export_3.12.16-4.11.16/Fitabase Data 3.12.16-4.11.16/minuteIntensitiesNarrow_merged.csv")
View(intensitiesMinuteMarch)

sleepMarch <-  read_csv("mturkfitbit_export_3.12.16-4.11.16/Fitabase Data 3.12.16-4.11.16/minuteSleep_merged.csv")
View(sleepMarch)

##April to May STEP 2

activityMay <-  read_csv("mturkfitbit_export_4.12.16-5.12.16/Fitabase Data 4.12.16-5.12.16/dailyActivityMAY.csv")
View(activityMay)

caloriesHourlyMay <-  read_csv("mturkfitbit_export_4.12.16-5.12.16/Fitabase Data 4.12.16-5.12.16/hourlyCaloriesMAY.csv")
View(caloriesHourlyMay)

intensityHourlyMay <-  read_csv("mturkfitbit_export_4.12.16-5.12.16/Fitabase Data 4.12.16-5.12.16/hourlyIntensitiesMAY.csv")
View(intensityHourlyMay)

stepshourlyMay <-  read_csv("mturkfitbit_export_4.12.16-5.12.16/Fitabase Data 4.12.16-5.12.16/hourlyStepsMAY.csv")
View(stepshourlyMay)

intensitiesMinuteMay <-  read_csv("mturkfitbit_export_4.12.16-5.12.16/Fitabase Data 4.12.16-5.12.16/minuteIntensitiesNarrowMAY.csv")
View(intensitiesMinuteMay)

sleepMay <-  read_csv("mturkfitbit_export_4.12.16-5.12.16/Fitabase Data 4.12.16-5.12.16/minuteSleepMAY.csv")
View(sleepMay)

SleepDay <-  read_csv("mturkfitbit_export_4.12.16-5.12.16/Fitabase Data 4.12.16-5.12.16/sleepDayMAY.csv")
View(SleepDay)


##Merging Data STEP3

Activity <- rbind(activityMarch, activityMay)
View(Activity)

HourlyCalories <-rbind(caloriesHourlyMarch, caloriesHourlyMay)
View(HourlyCalories)

HourlyIntensities <-rbind(intensityHourlyMarch, intensityHourlyMay)
View(HourlyIntensities)

HourlySteps <-rbind(stepshourlyMarch, stepshourlyMay)
View(HourlySteps)

MinuteIntensities <-rbind(intensitiesMinuteMarch, intensitiesMinuteMay)
View(MinuteIntensities)

Sleep <-rbind(sleepMarch, sleepMay)
View(Sleep)
```

Cleaning/ manipulation of data

```
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

##FINDINGS
```
## Step v. Calories
ggplot(Activity, aes(x = TotalSteps, y = Calories)) +
  geom_point() +
  geom_smooth(method = "lm", linetype = "solid", color = "pink")

```
<img width="751" height="447" alt="image" src="https://github.com/user-attachments/assets/44e34ec4-d1d1-4a44-8235-fda0765a73e5" />

Now I wanted to find out calories burned and active distance 

```
  
Vactive <-  ggplot(data= Activity2) +
    geom_point(mapping = aes(x=VeryActiveDistance, y=Calories))

Mactive <-   ggplot(data= Activity2) +
      geom_point(mapping = aes(x=ModeratelyActiveDistance, y=Calories))
  
Sactive <-  ggplot(data= Activity2) +
    geom_point(mapping = aes(x=SedentaryActiveDistance, y=Calories))
  
Lactive <- ggplot(data= Activity2) +
  geom_point(mapping = aes(x=LightActiveDistance, y=Calories))
```
<img width="925" height="617" alt="image" src="https://github.com/user-attachments/assets/7b4b62ee-e349-431e-bc10-6062436a640d" />


This graph show us the average calories Burned by time of day 

```
df_summary <- ZhourlyCal %>%
  group_by(Time) %>%
  summarise(Average_Calories = mean(Calories, na.rm = TRUE))



ggplot(df_summary, aes(x = Time, y = Average_Calories)) +
  geom_col(fill = "#69b3a2") +
  labs(
    title = "Average Calories Burned by Time of Day",
    x = "Time of Day",
    y = "Average Calories"
  ) +
  theme_minimal()
```

<img width="928" height="616" alt="image" src="https://github.com/user-attachments/assets/048ad4af-d87d-48d3-bfdc-66bb8acdcbd0" />


Here is the participants sleep quality

```
df_quality <- SleepDay %>%
  group_by(TotalSleepRecords) %>%
  summarise(Average_Time_Asleep = mean(TotalTimeInBed, na.rm = TRUE))


df_quality2 <- SleepDay  %>% select(-Id, -SleepDay)

df_quality2 %>%
  melt(value.name = 'TimeAsleep',id.vars = 'TotalSleepRecords',variable.name = 'BedTime')  %>%
  ggplot(aes(x=TotalSleepRecords,y=TimeAsleep,fill=BedTime))+
  labs(title = "Sleep Qulaity",caption = "* 8 hours of sleep is 480 minutes")+
  geom_col(position = position_jitterdodge(dodge.width = 0.5,jitter.height = 0,jitter.width = 0,seed = 25)) -> gg_output


plotly_object <- plotly::ggplotly(gg_output)

plotly_object
```

<img width="1277" height="732" alt="Sleep Quality" src="https://github.com/user-attachments/assets/87045fc8-6992-4494-872b-41cc0820c337" />



Average steps taken by time of the day 
```
# Bar chart
ggplot(step_summary, aes(x = Time, y = Average_Steps)) +
  geom_col(fill = "#69b3a2") +
  labs(
    title = "Average Steps Taken by Time of Day",
    x = "Time of Day",
    y = "Average Steps"
  ) +
  theme_minimal()


```

<img width="753" height="732" alt="image" src="https://github.com/user-attachments/assets/bd231f61-5dbb-4126-84f6-c17ef2f5e840" />


Average intensity per hour 

```
ggplot(int_summary, aes(x = Time, y = Average_Intenities)) +
  geom_col(fill = "#69b3a2") +
  labs(
    title = "Average Intensity Per Hour",
    x = "Time of Day",
    y = "Average Intensity"
  ) +
  theme_minimal()

```

<img width="1181" height="802" alt="intensity" src="https://github.com/user-attachments/assets/12049952-8912-4275-a709-8b81c5854868" />


Average intensity by day 

```
week <- ZhourlyInt %>%
  mutate (ActivityDate = weekdays(Date)) %>%
  group_by(ActivityDate) %>%
  summarize(AverageIntensity = mean(TotalIntensity)) %>%
  mutate(ActivityDate = factor(ActivityDate, levels = c("Monday", "Tuesday", "Wednesday", "Thursday", "Friday", "Saturday", "Sunday")))


ggplot(week, aes(x = ActivityDate, y = AverageIntensity)) +
  geom_col(fill = "#69b3a2") +
  labs(
    title = "Average Intensity By Day",
    x = "Day of the Week",
    y = "Average Intensity"
  ) +
  theme_minimal()

 
```













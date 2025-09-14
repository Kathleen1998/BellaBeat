The business task was to seek information from trends on similar devices, and what can inform us on how to market and influnence non Bellabeat consumer

All data in this project is sourced by [Fitbit Fitness Tracker Data] {https://www.kaggle.com/datasets/arashnic/fitbit} on Kaggle which had been allocated from [Zenodo] {https://zenodo.org/records/53894#.X9oeh3Uzaao}

The source is accurate, complete, and unbiased information that has been vetted and proven fit for use The source is original the data is copied from Zenodo exactly That data isn't missing important information needed to answer the question or find the solution The data is not current it is between the months of March through May The data is well cited linking back to the original source of the data


##FINDINGS
```
## Step v. Calories
ggplot(Activity, aes(x = TotalSteps, y = Calories)) +
  geom_point() +
  geom_smooth(method = "lm", linetype = "solid", color = "pink")

##creating a satter plot with the x axis as the number of steps and the y being calories 

```
<img width="751" height="447" alt="image" src="https://github.com/user-attachments/assets/44e34ec4-d1d1-4a44-8235-fda0765a73e5" />

Now I wanted to find out calories burned and active distance 
There's a strong correlation between the number of steps taken and the calories burned. To encourage users, the Bellabeat app can send notifications that highlight this relationship. These messages can remind users that the more they move, the more calories they burn, helping them stay motivated and reach their goals.
```
  
Vactive <-  ggplot(data= Activity2) +
    geom_point(mapping = aes(x=VeryActiveDistance, y=Calories))

Mactive <-   ggplot(data= Activity2) +
      geom_point(mapping = aes(x=ModeratelyActiveDistance, y=Calories))
  
Sactive <-  ggplot(data= Activity2) +
    geom_point(mapping = aes(x=SedentaryActiveDistance, y=Calories))
  
Lactive <- ggplot(data= Activity2) +
  geom_point(mapping = aes(x=LightActiveDistance, y=Calories))

## Creating 4 differnt plots and adding them all together for simplicity
## First scatter plot being VeryActiveDistance as the X aes and Y being the calories, then moderat active and calories, light activity and lastly sedentary
```
<img width="925" height="617" alt="image" src="https://github.com/user-attachments/assets/7b4b62ee-e349-431e-bc10-6062436a640d" />

This graph show us the average calories Burned by time of day 

This data shows that users tend to be most active around noon and in the early evening from 5 p.m. to 7 p.m. This pattern is consistent across several metrics, including **average steps taken**, **intensity per hour**, and **calories burned**. There's a notable dip in activity and calories burned around 3 p.m.
The data also shows that users are most active on Mondays, Fridays, and Sundays. Sunday is often a rest or lighter activity day for many people, while Mondays and Fridays tend to be lower intensity days, possibly due to a lack of motivation.
To address these trends, Bellabeat could implement a gamified system with achievements and a point system. Users could compete with friends and family, which could provide extra motivation, especially on days when activity levels tend to be lower.

```
HourlyCal %>%
  group_by(Time) %>%
  summarise(Average_Calories = mean(Calories, na.rm = TRUE))
# extracting data from HourlyCal to put group time and calculate the average calories 

ggplot(df_summary, aes(x = Time, y = Average_Calories)) +
  geom_col(fill = "#69b3a2") +
  labs(
    title = "Average Calories Burned by Time of Day",
    x = "Time of Day",
    y = "Average Calories"
  ) +
  theme_minimal()
#Creating a bar chart with x being time and Y being Average calories to find avg calories burned by the hour

```

<img width="928" height="616" alt="image" src="https://github.com/user-attachments/assets/048ad4af-d87d-48d3-bfdc-66bb8acdcbd0" />


Here is the participants sleep quality
Users who sleep through the night tend to sleep for longer periods. The data also reveals a group of users who wake up multiple times during the night. For these users, the app could offer a few suggestions:
Suggest a consultation with a healthcare provider- if they frequently wake up during the night.
Recommend incorporating more physical activity-into their daily routine to help them feel more tired at bedtime.

```
df_quality <- SleepDay %>%
  group_by(TotalSleepRecords) %>%
  summarise(Average_Time_Asleep = mean(TotalTimeInBed, na.rm = TRUE))
#extracts, counts and averages the total sleep record to find how many people wake up in the night

df_quality2 <- SleepDay  %>% select(-Id, -SleepDay)
#extracting colums not need for the plot


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
<img width="1181" height="802" alt="day" src="https://github.com/user-attachments/assets/6dd7f6f8-6c17-4d8c-832d-310b2a165ca6" />









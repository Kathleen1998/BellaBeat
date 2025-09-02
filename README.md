The business task was to seek information from trends on similar devices, and what can inform us on how to market and influnence non Bellabeat consumer

All data in this project is sourced by [Fitbit Fitness Tracker Data] {https://www.kaggle.com/datasets/arashnic/fitbit} on Kaggle which had been allocated from [Zenodo] {https://zenodo.org/records/53894#.X9oeh3Uzaao}

The source is accurate, complete, and unbiased information that has been vetted and proven fit for use The source is original the data is copied from Zenodo exactly That data isn't missing important information needed to answer the question or find the solution The data is not current it is between the months of March through May The data is well cited linking back to the original source of the data


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
<img width="1181" height="802" alt="day" src="https://github.com/user-attachments/assets/6dd7f6f8-6c17-4d8c-832d-310b2a165ca6" />


##Delay per airline percentage--------------------

```
Aline <- (Total_df$Var1)

Atotal <- (Total_df$Freq)

Adelay <- (Delays$Total)

overLap <- data.frame(Aline, Atotal, Adelay)

overLap %>%
  melt(value.name = 'DelaysVsFlights',id.vars = 'Aline',variable.name = 'Airlines')  %>%
  ggplot(aes(x=Aline,y=DelaysVsFlights,fill=Airlines))+
  geom_col(position = position_jitterdodge(dodge.width = 0.5,jitter.height = 0,jitter.width = 0,seed = 25)) -> gg_output


plotly_object <- plotly::ggplotly(gg_output)

plotly_object
```

<img width="910" height="732" alt="delaysnondelays" src="https://github.com/user-attachments/assets/b41a3db8-9844-4499-83fc-983b03691c19" />








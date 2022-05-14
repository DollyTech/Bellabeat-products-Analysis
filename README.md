# Bellabeat-products-Analysis
A analysis that gives insights to tech base app and the trends of the users
---
title: "Bellabeat product analysis"
author: "Dolapo Oyewumi"
date: "`r Sys.Date()`"
output: html_document
---

```{r}
install.packages("tidyverse")
library(tidyverse)

```


comments: for this project, i will be using FitBit Fitness Tracker Data
 uploading the data



```{r}
daily_activity <- read.csv("dailyActivity_merged.csv")
daily_calories <- read.csv("dailyCalories_merged.csv")
daily_intensities <- read.csv("dailyIntensities_merged.csv")
daily_steps <- read.csv("dailySteps_merged.csv")
daily_sleep <- read.csv("sleepDay_merged.csv")
weight <- read.csv("weightLogInfo_merged.csv")

```


comments:identify all columns in the data

```{r}
head(daily_activity)
colnames(daily_activity)
colnames(daily_calories)
colnames(daily_intensities)
colnames(daily_steps)
colnames(daily_sleep)
colnames(weight)
```


comments:checking the numbers of users in the 
```{r}
colnames(weight)
n_distinct(daily_activity$Id)
n_distinct(daily_calories$Id)
n_distinct(daily_intensities$Id)
n_distinct(daily_steps$Id)
n_distinct(daily_sleep$Id)
n_distinct(weight$Id)
```


comments: Observations: majority of the users don't log in their weight. We only have 8 people logged in so far and the number is too small to make a reasonable conclusion.
Users should be encouraged to log in their weight. Also, not all consumers remember to log in their sleeptime

## Analyzing
```{r}
daily_activity %>%  
  select(TotalSteps,
         TotalDistance,
         SedentaryMinutes) %>%
  summary()
```
## analyse the daily_activity by categories
```{r}
daily_activity %>%
  select(VeryActiveMinutes, FairlyActiveMinutes, LightlyActiveMinutes) %>%
  summary()
```
comments:majority of the users are lightly active.
the total step per day needs to be increased. (7638 total steps per day) which is bad for the health. According to mayo clinic, Active is more than 10,000 steps per day.
Users should be encouraged to be more active daily to improve good health.

```{r}
daily_calories %>%  
  select(Calories) %>%
  summary()

```


```{r}
daily_sleep %>%
  select(TotalSleepRecords, TotalMinutesAsleep, TotalTimeInBed) %>%
  summary()
```


comments: The average time sleep time is quite inadequate. The average sleep time for an adults is 7- 9 hours. Users should be encouraged to sleep more

```{r}
weight %>%
  select(WeightKg, BMI) %>%
  summary()
```

comments: users average weight is 72.04kg. Users should be encouraged to watch their weight

## merging data

```{r}
step_calories <- merge(daily_calories, daily_steps, by = "Id")
head(step_calories)
```


##visualization

comments: want to check the correlation between Calories and Total Steps

```{r}
ggplot(data=daily_activity, aes(x=TotalSteps, y=Calories)) +
geom_point() + geom_smooth() + labs(title="Total Steps vs. Calories")

```


comments: there's a positive correlation between total steps and calories. The more active the users, the more calories burnt

```{r}
ggplot(data=daily_activity, aes(x=VeryActiveDistance, y=Calories)) +
geom_point() + geom_smooth() + labs(title="VeryActiveDistance vs. Calories")

```

comments: this shows that very active distance burns more calories than Total steps. users who are looking forward to burn calories should be encouraged to be more active
```{r}
weight_calories <- merge(daily_calories, weight, by = "Id")
head(weight_calories)
ggplot(data=weight_calories, aes(x=WeightKg, y=Calories)) +
geom_histogram(stat = "identity", fill='blue') +
labs(title="Weight vs. Calories")

```

comments: There's a correlation with weight and calories. Users who are concerned about their weight,should watch their calories intake and are encouraged to be more active.

### RECOMMEDATION
 *	Total steps per day needs to be increased. The average Total steps is 7638as against the minimum of 10,000 steps recommended by mayo clinic. Users are encouraged to be more active to improve good health.
*	Users should be encouraged to increase the hours for sleeping. The average sleep time is inadequate. The ideal sleep time is 7- 9 hours daily
*	There’s a correlation between calories consumed and weight. Users who intends to reduce in weight should reduce calories intake. The company can suggest foods low in calories to them.
*	Analyses showed that the more the steps or active a user is, the higher the calories burnt. Users who looked forward to burning calories should be more active in their daily lives.


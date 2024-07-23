# Cyclistic: Converting casual riders into annual subscribers

## Table of Contents

1. Description and deliverables 
2. Project Overview 
3. Tools Used 
4. Asking the right questions 
5. Getting to know and preparing the data 
6. Processing 
7. Analyzing with R 
8. Visualizations 
9. Conclusions and Recommendations 

## 1. Description and deliverables

This case study was my capstone project for Google’s Data Analytics 
professional certificate. In the scenario provided, my task was to 
understand and describe how casual riders and annual members use 
Cyclistic bikes differently by analyzing the company’s historical trip 
data. From these insights, the (fictional) team would design a new 
marketing strategy to convert casual riders into annual members. To get 
my recommendations approved, they would have to be backed up by 
compelling data insights and data visualizations.

Therefore, the deliverables for this project were:

1. A clear statement of the business task
2. A description of all data sources used
3. Documentation of any cleaning or manipulation of data
4. A summary of the analysis
5. Supporting visualizations and key findings
6. The top three recommendations based on the analysis

## 2. Project Overview

Cyclistic’s
 finance analysts have concluded that annual members are much more 
profitable than casual riders. Although the pricing flexibility helps 
Cyclistic attract more customers, the marketing director believes that 
maximizing the number of annual members will be key to future growth. 
Rather than creating a marketing campaign that targets all-new 
customers, the marketing director believes there is a solid opportunity 
to convert casual riders into members. She notes that casual riders are 
already aware of the Cyclistic program and have chosen Cyclistic for 
their mobility needs. The marketing director has set a clear goal: 
Design marketing strategies aimed at converting casual riders into 
annual members. In order to do that, however, the team needs to better 
understand how annual members and casual riders differ, why casual 
riders would buy a membership, and how digital media could affect their 
marketing tactics. The marketing director and her team are interested in
 analyzing the Cyclistic historical bike trip data to identify trends.

## 3. Tools Used

- Windows Command Prompt (to merge the .csv files)
- Rstudio (for data cleaning and EDA)
- Tableau Public (to create the visualizations)
- Microsoft PowerPoint (to create the [Executive Summary](https://github.com/franciscorcastro/Projects/blob/main/Cyclistic/Cyclistic_Executive_Summary.pptx))

## 4. Asking the right questions

Before
 any analysis starts, we should consider what is the actual problem 
we’re trying to solve and how can the resulting insights from our 
analysis can drive business decisions. In this case, our analysis of the
 historical trip data from Cyclistic users will inform a new marketing 
strategy designed to convert casual riders into annual subscribers or 
members. This brings us to our first deliverable, which was a clear 
statement of the business task:

1. Analyze historical usage data to describe how annual members and casual users use Cyclistic bikes differently

## 5. Getting to know and preparing the data

As
 mentioned above, data is located and published in Cyclistic’s own 
website through monthly CSV files. There’s a distinct .csv dataset for 
each of the last 12 months. Inside every .csv file, there are thousands 
of rows, each containing information about 1 distinct trip. Each trip 
contains information logged through multiple columns regarding its 
duration, date, start and end station as well as some other crucial 
information such as membership status.

Since
 the data is published by the bike-sharing service as open data, there 
should be no bias or credibility issues. At first glance, the data seems
 to be reliable, original, comprehensive, and current. Privacy, security
 and accessibility concerns are handled by the publisher, in this case, 
the bike sharing service Divvy. All usage of this data throughout this 
project adheres to the [Data License Agreement](https://divvybikes.com/data-license-agreement)
 of the publisher. To use the data provided, I downloaded the official 
files from the publisher’s website on 14/07/2024 and compared them with 
each other to verify integrity.

Referring
 back to the business task, which was how casual riders and annual pass 
riders use the service differently, this dataset provides us with enough
 information to answer that given that it contains information about all
 trips that were made in the past 12 months using the service. From 
there we can identify trends and draw a comparison between the two 
groups. It is, however, still early to identify all the problems in the 
dataset, but a quick look through it already showed that there are a lot
 of blanks and null values across multiple columns. After this, we’re 
able to present the second deliverable, a description of all data 
sources used:

2.
 The data for this activity is from Divvy Bikes, the city of Chicago’s 
bicycle sharing service. The dataset that was used for this project was 
the monthly data from June 2023 to June 2024, which was then combined 
into a single dataset by merging the 13 .csv files using Windows’ 
Command Prompt, as described in the next section. The combined data set 
contained 6454011 rows and 13 columns for the variables listed below.

![https://github.com/franciscorcastro/Projects/blob/main/Cyclistic/img/variable_description.png](https://github.com/franciscorcastro/Projects/blob/main/Cyclistic/img/variable_description.png)

Variable and Description

![https://github.com/franciscorcastro/Projects/blob/main/Cyclistic/img/column_datatype.png](https://github.com/franciscorcastro/Projects/blob/main/Cyclistic/img/column_datatype.png)

Column and Data Type (in R)

**Note:** More information about the data can be found [here](https://divvybikes.com/). Data can be downloaded through [here](https://divvy-tripdata.s3.amazonaws.com/index.html). License can be accessed through [here](https://divvybikes.com/data-license-agreement).

## 6. Processing

As
 hinted above, the first obstacle I went through was that, even though 
there was no mention of it in the case study guide, all data relative to
 the years of 2020 and later was organized in monthly .csv files instead
 of quarterly or yearly as stated in the guide. A quick look through 
some of the monthly files also showed that merging 3 or 4 months worth 
of data would exceed Microsoft Excel’s row limit. From that moment on, 
it became clear I would have to use a tool designed for larger datasets,
 such as R (through Rstudio).

Before
 I could start cleaning the data using Rstudio, however, I had to tackle
 the first issue, which I did using Windows Command Prompt to merge all 
the monthly .csv files from the last year into one single file which 
could then be imported to Rstudio. This was done through:

```
cd "YourWorkingDirectory" #direct your terminal to a specific folder containing only your downloaded csv files
copy *.csv merged.csv #copy all of the rows from all of the .csv files into a single file called merged.csv
```

**Note:** The text after # is explaining the command on that line, you do not need to copy it.

Now
 that I had a single .csv file to work with, I could open Rstudio and 
start cleaning the data. I did so by using the following code:

> Installing and importing necessary packages
> 

```
install.packages("tidyverse", repos="http://cran.us.r-project.org")
install.packages("janitor", repos="http://cran.us.r-project.org")
install.packages("here",repos="http://cran.us.r-project.org")
install.packages("skimr", repos="http://cran.us.r-project.org")
install.packages("ggplot2", repos="http://cran.us.r-project.org")
install.packages("lubridate", repos="http://cran.us.r-project.org")
install.packages("scales", repos="http://cran.us.r-project.org")
library(tidyverse)
library(janitor)
library(here)
library(skimr)
library(ggplot2)
library(lubridate)
library(scales)
```

> Importing merged csv file
> 

```
twelve_months_data <- read_csv("path/merged.csv")
```

> Checking data integrity and structure
> 

```
colnames(twelve_months_data)
str(twelve_months_data)
View(twelve_months_data)
```

> Removing entries with missing values duplicates
> 

```
clean_twelve_months_data <- na.omit(twelve_months_data) %>% distinct(ride_id, .keep_all = TRUE)
```

> Creating new columns for further analysis
> 

```
clean_data_with_new_columns <- clean_twelve_months_data %>% mutate(ride_length = ended_at - started_at, day_of_week = weekdays(started_at)) %>% filter(ride_length > 0) %>% clean_names()
```

> Exporting a clean version of the original dataset for analysis
> 

```
write_csv(clean_data_with_new_columns, "cleandata.csv")
```

As
 a recap, I ensured data integrity by keeping and comparing the original
 datasets to the merged .csv file and imported it directly to Rstudio. I
 also compared the final cleaned data to the original dataset. I removed
 duplicates, removed rows with missing-values, checked for data-range 
and data-type issues. Finally, I documented the whole cleaning process 
in an R Markdown document available [here](https://github.com/franciscorcastro/Projects/blob/main/Cyclistic/CleaningReport.Rmd), which is the third deliverable for the project.

3. [Cleaning Report](https://github.com/franciscorcastro/Projects/blob/main/Cyclistic/CleaningReport.Rmd)

## 7. Analyzing with R

After
 I had cleaned the data, I wanted to continue analyzing it using Rstudio
 given its size. So, I imported it back to Rstudio and ran some summary 
analysis that I thought could be useful to answer the business task:

> Importing back .csv file for analysis
> 

```
cleandata <- read_csv("cleandata.csv")
```

> Creating
 a Mode function in R (R does not have a native function for this and it
 would come in handy at a later moment so I added it here afterwards for
 clarity)
> 

```
Mode <- function(x, na.rm = FALSE) {
  if(na.rm){
    x = x[!is.na(x)]
  }

  ux <- unique(x)
  return(ux[which.max(tabulate(match(x, ux)))])
}
```

> Performing summary analysis (explanation preceded by #)
> 

```
Mode(cleandata$day_of_week) #shows the day of the week with the most trips
seconds_to_period(mean(cleandata$ride_length)) #shows the average trip duration
seconds_to_period(max(cleandata$ride_length)) #shows the maximum trip duration
member_status_summary <- #creates a summary table with information for each user type
    cleandata %>%
    group_by(member_casual) %>%
    filter(ride_length > 0) %>%
    summarize(average_ride_length=seconds_to_period(round(mean(ride_length))),
              min_ride_length=seconds_to_period(round(min(ride_length), 2)),
              max_ride_length=seconds_to_period(round(max(ride_length))),
              most_common_day=Mode(day_of_week),
              most_common_start_station=Mode(start_station_name),
              most_common_end_station=Mode(end_station_name),
              ride_count = n()) %>%
    ungroup() %>%
    mutate(percent = label_percent()(ride_count/sum(ride_count)))
  day_of_week_summary <- #creates a summary table with information for each day of the week
      cleandata %>%
      group_by(day_of_week) %>%
      filter(ride_length > 0) %>%
      summarize(average_ride_length=seconds_to_period(round(mean(ride_length),1)),
                num_of_rides = n()) %>%
      arrange(desc(num_of_rides))
```

> Exporting summary .csv files for further analysis
> 

```
write_csv(day_of_week_summary, "dayofweeksumm.csv")
write_csv(member_status_summary, "memberstatus.csv")
```

Following
 these steps, I have created specific subsets and columns to better 
answer the business question. On the other hand, I have also got 
unusable results, such as the maximum trip duration being over a week. 
However, most of the insights that resulted from this analysis were 
useful and allowed me to present the fourth deliverable and to conclude 
that:

4.

- The average trip ****of a **casual rider** is **longer** than that of a **user with an annual pass.**
- The average trip of a **casual rider** is **significantly longer** on the weekend (Saturday and Sunday), while the average trip duration of a **member** is **similar** over the seven days of the week, although it is still slightly longer on the weekend.
- For **members**, the day with the **highest** number of trips is Thursday and the day with the lowest number is
Sunday. There are more trips during the week than on the weekend.
- For **casual riders**, the day with the highest number of journeys is Saturday and the day
with the lowest number is Tuesday. Contrary to members, there are more
trips on the weekend than during the week.
- 64% of trips were made by users with a pass, while 36% were made by a casual rider.
- For both types of users, trip duration is higher during the weekend than during the week.

## 8. Visualizations

In
 order to support my key findings, one of the goals of this project was 
to produce beautiful visualizations to present to stakeholders. Using 
Tableau Public, I created some graphs and charts that can help 
understand the trends and patterns of the data in a more digestible way.
 But we get to those, visualizations are sometimes also useful to 
recognize those trends in the first place. So, during the last part of 
the analysis process, I created some visualizations in Rstudio to better
 understand the data myself before I could share it with anyone else. 
This was done using the ggplot2 library.

> Creating a simple visualization plot for average trip duration
> 

```
cleandata %>%
  mutate(weekday = wday(started_at, label = TRUE)) %>%
  group_by(member_casual, weekday) %>%
  summarise(number_of_rides = n(),
            average_duration = mean(ride_length, na.rm = TRUE)) %>%
  arrange(member_casual, weekday) %>%
  ggplot(aes(x = weekday, y = average_duration, fill = member_casual)) +
  geom_col(position = "dodge")
```

![https://github.com/franciscorcastro/Projects/blob/main/Cyclistic/img/avg_duration_plot.png](https://github.com/franciscorcastro/Projects/blob/main/Cyclistic/img/avg_duration_plot.png?)

Average Trip Duration per weekday

> Creating a simple visualization plot for number of trips per weekday
> 

```r
cleandata %>%
  mutate(weekday = wday(started_at, label = TRUE)) %>%
  group_by(member_casual, weekday) %>%
  summarise(number_of_rides = n()
    ,average_duration = mean(ride_length)) %>%
  arrange(member_casual, weekday) %>%
  ggplot(aes(x = weekday, y = number_of_trips, fill = member_casual)) +
  geom_col(position = "dodge")
```

![https://github.com/franciscorcastro/Projects/blob/main/Cyclistic/img/number_of_trips_plot.png](https://github.com/franciscorcastro/Projects/blob/main/Cyclistic/img/number_of_trips_plot.png)

Number of Trips per day of the week

This
 helps set the stage for more complex visualizations using Tableau 
Public, a free platform which I used to create the following 
visualizations (then added to the [Executive Summary](https://github.com/franciscorcastro/Projects/blob/main/Cyclistic/Cyclistic_Executive_Summary.pptx)).

![https://github.com/franciscorcastro/Projects/blob/main/Cyclistic/img/tableau_tripdistribution.png](https://github.com/franciscorcastro/Projects/blob/main/Cyclistic/img/tableau_tripdistribution.png)

Trip Distribution per User Type

![https://github.com/franciscorcastro/Projects/blob/main/Cyclistic/img/tableau_avgduration_usertype.png](https://github.com/franciscorcastro/Projects/blob/main/Cyclistic/img/tableau_avgduration_usertype.png)

Average Trip Duration per User type

![https://github.com/franciscorcastro/Projects/blob/main/Cyclistic/img/tableau_avgduration_dayofweek.png](https://github.com/franciscorcastro/Projects/blob/main/Cyclistic/img/tableau_avgduration_dayofweek.png)

Average Trip Duration per day of the week

![https://github.com/franciscorcastro/Projects/blob/main/Cyclistic/img/tableau_numoftrips_dayofweek.png](https://github.com/franciscorcastro/Projects/blob/main/Cyclistic/img/tableau_numoftrips_dayofweek.png)

Number of Trips per day of the week

![https://github.com/franciscorcastro/Projects/blob/main/Cyclistic/img/tableau_numoftrips_dayofweek_member.png](https://github.com/franciscorcastro/Projects/blob/main/Cyclistic/img/tableau_numoftrips_dayofweek_member.png)

Number of Trips by members per day of the week

![https://github.com/franciscorcastro/Projects/blob/main/Cyclistic/img/tableau_numoftrips_dayofweek_casual.png](https://github.com/franciscorcastro/Projects/blob/main/Cyclistic/img/tableau_numoftrips_dayofweek_casual.png)

Number of Trips by casual riders per day of the week

![https://github.com/franciscorcastro/Projects/blob/main/Cyclistic/img/tableau_numoftrips_dayofweek_both.png](https://github.com/franciscorcastro/Projects/blob/main/Cyclistic/img/tableau_numoftrips_dayofweek_both.png)

Number of trips by both types of users per day of the week

These
 visualizations provide visual context for the findings shared above and
 help reach a larger audience by focusing on non-technical aspects of 
the data. They were the fifth deliverable of this project.

## 9. Conclusion and Recommendations

Finally,
 we reach the end of this project. The sixth and final deliverable for 
the case study were my top three recommendations for the marketing 
strategy to convert casual riders into annual members. But first we must
 state what were the biggest findings of the analysis. First, it became 
fairly obvious that annual members and casual riders use the bikes 
differently, and data suggests they use them for different purposes. 
Second, their usage varies in more than one aspect, but the most 
important ones might just be the average trip duration and the number of
 total trips as described above.

Taking that into consideration, my top three recommendations for the marketing strategy would be to:

1. Offer a discounted weekend-only annual subscription for users who do not need Cyclistic bikes during the week (given that most current casual riders
use the service during the weekend)
2. Focus the new marketing campaign on the benefits of using Cyclistic bikes to
commute (in order to change their usage from the weekend to weekdays as
well)
3. Focus the new marketing campaign on the financial benefits of buying the
subscription instead of paying for each trip, especially for longer
trips (most casual riders use the bikes for longer than annual members)

These recommendations are also stated in the [Executive Summary](https://github.com/franciscorcastro/Projects/blob/main/Cyclistic/Cyclistic_Executive_Summary.pptx/), along with supporting visualizations.

There
 are, of course some limitations to these recommendations as they are 
based on the correlations described by the data, not on more accurate 
causal relations. A survey on casual riders would be an interesting way 
to get more insight on why they’re not using an annual subscription.

As
 a final remark, I would just like to say that this project was a lot of
 fun and a great way to practice all the skills learned during the 
Google Data Analytics Professional Certificate courses. I will be 
posting another project in the next few weeks.

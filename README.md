# Bellabeat - How can a wellness company play it smart?

&nbsp;&nbsp;&nbsp;&nbsp;Bellabeat, a high-tech company that manufactures health-focused smart products.Sršen used her background as an artist to
develop beautifully designed technology that informs and inspires women around the world. Collecting data on activity, sleep,
stress, and reproductive health has allowed Bellabeat to empower women with knowledge about their own health and habits.
Since it was founded in 2013, Bellabeat has grown rapidly and quickly positioned itself as a tech-driven wellness company for women.

## Scenario

&nbsp;&nbsp;&nbsp;&nbsp;Bellabeat is a successful small company, but it has the potential to become a larger player in the
global smart device market.Urška Sršen, cofounder and Chief Creative Officer of Bellabeat, believes that analyzing smart
device fitness data could help unlock new growth opportunities for the company.As a Bellabeat marketing analytics team we have been
asked to focus on analyze smart device data to gain insight into how consumers are using their smart devices.The insights we discover
will then help guide marketing strategy for the company.

### ***This Business Task Have been go through six phase of data analysis process:***

### \-[Ask](#ASK-PHASE:-Identifying-the-Business-Task)
### \-[Prepare](#Prepare)
### \-[Process](#Process)
### \-[Analyze](#Analyze)
### \-[Share](#Share)
### \-[Act](#Act)

#### ***The Key Stakeholders :***

\- Urška Sršen-Chief Creative Officer and Bellabeat’s Co-founder

\-Sando Mur-Mathematician and Bellabeat’s Co-founder

\-Bellabeat’s marketing analytics team.

## ASK PHASE: Identifying the Business Task
Bellabeat aims to leverage smart device usage data to gain insights into customer behavior and improve its marketing strategy. The analysis will answer:
1. What are some trends in smart device usage?
2. How could these trends apply to Bellabeat customers?
3. How could these trends help influence Bellabeat’s marketing strategy?

## PREPARE PHASE: Gathering and Understanding Data Sources
- **Data Source**: Fitbit Fitness Tracker Data (Public Dataset from Kaggle)
- **Data Overview**: Includes physical activity, heart rate, and sleep monitoring from 30 users.
- **Limitations**: Small sample size, possible bias, and lack of diversity.
- **Action Taken**: Additional data sources considered for validation.

## PROCESS PHASE: Cleaning and Transforming Data
### SQL Data Cleaning
```sql
-- Checking for missing values
SELECT column_name, COUNT(*) AS missing_values
FROM FitBit_Data
WHERE column_name IS NULL
GROUP BY column_name;

-- Removing duplicate records
DELETE FROM FitBit_Data
WHERE rowid NOT IN (
    SELECT MIN(rowid)
    FROM FitBit_Data
    GROUP BY user_id, activity_date
);
```

### R Data Cleaning
```r
# Load necessary libraries
library(dplyr)
library(ggplot2)

# Load dataset
data <- read.csv("fitbit_data.csv")

# Checking for missing values
colSums(is.na(data))

# Removing duplicates
data_clean <- data %>% distinct()
```

## ANALYZE PHASE: Performing Calculations and Identifying Trends
### SQL Analysis
```sql
-- Aggregating daily activity data
SELECT user_id, activity_date, SUM(total_steps) AS daily_steps, SUM(total_calories) AS daily_calories
FROM FitBit_Data
GROUP BY user_id, activity_date;

-- Identifying user activity levels
SELECT user_id,
       AVG(total_steps) AS avg_daily_steps,
       CASE 
           WHEN AVG(total_steps) >= 10000 THEN 'Highly Active'
           WHEN AVG(total_steps) BETWEEN 5000 AND 9999 THEN 'Moderately Active'
           ELSE 'Sedentary'
       END AS activity_level
FROM FitBit_Data
GROUP BY user_id;
```

### R Analysis
```r
# Average steps per user
avg_steps <- data_clean %>%
  group_by(user_id) %>%
  summarise(avg_steps = mean(total_steps))

# Activity level classification
data_clean <- data_clean %>%
  mutate(activity_level = case_when(
    total_steps >= 10000 ~ "Highly Active",
    total_steps >= 5000 ~ "Moderately Active",
    TRUE ~ "Sedentary"
  ))
```

## SHARE PHASE: Creating Visuals and Insights
### Tableau Dashboard
- **Step Trends vs. Calories Burned**
- **Activity Levels Across Users**
- **Sleep Patterns and Device Usage**

### R Data Visualization
```r
# Steps vs. Calories Burned
ggplot(data_clean, aes(x = total_steps, y = total_calories)) +
  geom_point(color = "blue") +
  labs(title = "Steps vs Calories Burned")
```

## ACT PHASE: Final Recommendations
1. **Target Audience Identification**: Focus marketing efforts on active users who track steps and calories frequently.
2. **Product Enhancement**: Improve sleep-tracking features based on observed sleep patterns.
3. **Strategic Marketing**: Personalize app recommendations based on user activity levels.

### GitHub Repository Structure
- `README.md`: Overview of the business task, approach, and findings.
- `SQL_Analysis.sql`: SQL queries used for data processing and analysis.
- `R_Analysis.R`: R script for data cleaning, transformation, and visualization.
- `Tableau_Dashboard.twb`: Interactive visualizations.

This structured approach ensures transparency and clarity in presenting Bellabeat’s smart device usage insights.


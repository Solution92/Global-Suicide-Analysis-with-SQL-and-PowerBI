
# Global Suicide Analysis with SQL and PowerBI

## Table of Content

- [Project Objective](#project-objective)
- [Data Source](#data-source)
- [Tools Used](#tools-used)
- [Data Cleaning and Preparation](#data-cleaning-and-preparation)
- [Exploratory Data Analysis (EDA)](#exploratory-data-analysis-eda)
- [Data Analysis](#data-analysis)
- [Result and Findings](#result-and-findings)
- [The Dashboard](#the-dashboard)
- [Recommendation](#recommendation)
- [Limitations](#limitations)
- [Reference](#reference)


### Project Objective

To provide answers on the highest and lowest suicide rate by country concerning year, sex, age, and percentage (age) distribution in the basic aggregate numbers covering 1979 to 2016. Also, to query and find out the total population and total suicide within the given year.

### Data Source

The dataset was provided in a CSV file format by the client which I am not permitted to disclose.

### Tools Used

- PostgreSQL — Data Cleaning, Transformation, and Analysis
  
- Power BI — Used For Creating Reports.

### Data Cleaning and Preparation

Dataset understanding is a crucial stage for any analyst. So, after importing the dataset to PostgreSQL, I needed to understand it so I did a simple query (SELECT * FROM SUICIDE) to call up the entire table for inspection. Note please, SUICIDE here, is the table name in PostgreSQL. The dataset has 43,776 rows and 6 columns comprising Country, Year, Sex, Age, Suicide_no, and Population as the case may be.
I also discovered that some fields in the Suicide_no and Population columns have null values. However, to remove the null values I used the UPDATE statement with a SET clause (see data analysis section).

### Exploratory Data Analysis (EDA)

With the help of statistical graphics and other data visualization methods, I have generated the following key Performance Indicators (KPIs) and insights:
- Total Country = 141

- Total Population Represented = 63,761,315,943

- Total Male =6,104,173

- Total Female = 1,894,294

- Total Percentage of Suicide = 0.012544

- Total Suicide by Year

- Sum of Year and Total Suicide by Sex

- Total Suicide by Age

- Total Suicide by Age (Percentage Distribution)

- Countries with the Highest and Lowest Suicide Rates.

### Data Analysis

This is the query process taken to analyze the dataset on PostgreSQL.


--To call up the entire table for inspection.
SELECT * FROM SUICIDE;

--To remove the null values I used the UPDATE statement with a SET clause.
UPDATE SUICIDE
SET suicides_no = 0
WHERE suicides_no IS NULL;

UPDATE SUICIDE
SET population = 0,
    suicides_no = 0
WHERE population IS NULL OR suicides_no IS NULL;

--KEY PERFORMANCE INDICATORS
--Total Country
SELECT COUNT(DISTINCT country) AS total_unique_countries
FROM SUICIDE; ==141

--Total Population Represented
SELECT SUM(population) AS total_population
FROM SUICIDE; ==63,761,315,943

--Total Male
SELECT SUM(suicides_no) AS total_male_suicides
FROM SUICIDE
WHERE sex = 'male'; ==6,104,173

--Total Female
SELECT SUM(suicides_no) AS total_female_suicides
FROM SUICIDE
WHERE sex = 'female'; ==1,894,294

--Total Percentage of Suicide
SELECT (SUM(suicides_no) * 100.0) / SUM(population) AS total_percentage_suicides
FROM SUICIDE; ==0.012544

--SUICIDE TRENDS OVER THE YEARS:
-- Total Number of Suicides Over the Years
SELECT year,
SUM(suicides_no) AS total_suicides
FROM SUICIDE
GROUP BY year
ORDER BY year;

-- Suicide Rate Per Year
SELECT year,
SUM(suicides_no) AS total_suicides,
ROUND(SUM(suicides_no) * 1.0 / SUM(population),4) AS suicide_rate
FROM SUICIDE
GROUP BY year
ORDER BY year;

--AGE GROUP ANALYSIS:
--Suicide Counts Across Different Age Groups:
SELECT age,
SUM(suicides_no) AS total_suicides
FROM SUICIDE
GROUP BY age
ORDER BY age; 

--Percentage Distribution of Suicides in Each Age Group:
SELECT age,
SUM(suicides_no) AS total_suicides,
(SUM(suicides_no) * 100.0) / SUM(SUM(suicides_no)) OVER () AS percentage_distribution
FROM SUICIDE
GROUP BY age
ORDER BY age;


--Gender-Based Analysis:
--Number of Suicides Between Males and Females
SELECT year, sex,
SUM(suicides_no) AS total_suicides
FROM SUICIDE
GROUP BY year, sex
ORDER BY year, sex;

### Result and Findings

The Global Suicide Analysis is summarized below

Image

-  From the visual above — Total Suicide by Year, At 260,429, 2003 had the highest Total Suicide and was 9.79% higher than 2006, which had the lowest Total Suicide at 237,200.
-  2003 accounted for 10.56% of Total Suicides. Across all 10 years, Total Suicides ranged from 237,200 to 260,429. 

Image 2

- At 2,895,388, 35–54 years had the highest Total Suicide and was 4,473.21% higher than 5–14 years, which had the lowest Total Suicide at 63,312.
- 35–54 years accounted for 36.07% of Total Suicides. Across all 6 ages, Total Suicides ranged from 63,312 to 2,895,388.

Image 3

- Sum of year and Total Suicide diverged the most when the sex was female when the Sum of the year was 41,840,950 higher than Total Suicide.

Image 4

- 35–54 years accounted for 36.07% of Total Suicides as seen in the visual above.

Image 5

- Total Suicide and total Country Suicide Rate are negatively correlated with each other.
- The Russian Federation accounted for 18.70% of Total Suicides.
- Total Suicide and Country Suicide Rates diverged the most when the country was the Russian Federation, where Total Suicide was 1,500,992 higher than the Country Suicide Rate.
- Countries like Montserrat, Folklands Island, Anguilla, Iraq, Sao Tome and Principe, and many others are among the countries with the lowest suicide rates.

### The Dashboard

Image 6

### Recommendation

Based on the results and findings from this Global Suicide Analysis, here are some potential recommendations and areas for further investigation:

1. Focus on High-Risk Age Group (35–54 years):
Given that the age group 35–54 years has the highest total suicides, consider implementing targeted interventions and mental health support programs for this demographic.
2. Yearly Variation and Trends:
Explore the factors contributing to the peak in suicides in 2003. Consider conducting further analysis to identify any specific events, economic conditions, or societal changes that may have influenced the increase.
3. Gender Disparities:
Investigate the significant divergence between the sum of years and total suicides when the sex is female. Understand the reasons behind this difference and explore potential gender-specific factors influencing suicide rates.
4. Country-Specific Strategies:
Given the negative correlation between Total Suicide and Country Suicide Rate, countries with high total suicides but low rates may need targeted strategies to address specific risk factors.
5. Russian Federation Intervention:
Since the Russian Federation accounts for a significant portion of total suicides, consider exploring tailored mental health programs and interventions to address the unique challenges in this country.
6. Low Suicide Rate Countries:
Examine the characteristics of countries with low suicide rates, such as Montserrat, Falklands Island, Anguilla, Iraq, Sao Tome and Principe. Identify factors that contribute to their lower rates and consider implementing preventive measures in higher-risk regions.
7. Long-Term Analysis:
Extend the analysis beyond the provided 10-year dataset to include a more comprehensive view of suicide trends over a more extended period, which may reveal additional insights.
8. Collaboration and Data Collection:
Collaborate with mental health organizations, government agencies, and researchers to collect more detailed data on social, economic, and cultural factors that may influence suicide rates. This could lead to a more nuanced understanding of the trends.
9. Preventive Education and Awareness:
Develop and implement educational programs and awareness campaigns aimed at reducing the stigma associated with mental health issues, promoting help-seeking behavior, and providing resources for those at risk.
10. Continuous Monitoring and Evaluation:
Establish a system for continuous monitoring and evaluation of suicide prevention programs. Regularly assess the effectiveness of interventions and adjust strategies based on ongoing data analysis.

### Limitations

The analysis was meant to cover 30 years but because of the limitation of visual space, I had to do the top 10 Years from 2000 to 2011.

### Reference

Part of the dataset used as part of my research during this analysis is from the [WHO Motality Database Online tool](http://apps.who.int/healthinfo/statistics/mortality/whodpms/)


#SQL #data #suicide #analysis #who #communication #PowerBI #teamwork #postgressql #dashboard

































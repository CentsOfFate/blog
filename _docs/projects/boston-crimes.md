---
title: Boston Crimes Analysis
permalink: /docs/boston-crimes/
---

The [Boston Crimes](https://www.kaggle.com/ankkur13/boston-crime-data) dataset can be found at Kaggle. This dataset features:

* Year, Month, Day of Week, and Hour Dates/Times
* Streets
* Offense Code Groups
* District and Reporting Area

This dataset is designed more for Machine Learning and making predictions on future crimes based on when reported crimes have already happened. But there's enough here that we can derive some interesting insights on the data.

First, let's start off with some static graphs. There are four years in this datasetL 2015, 2016, 2017 and 2018. **2017** has the most number of offenses committed.

![ss1](/blog/assets/docs 2/Year.png)

When we split the data by month, the majority of crimes committed happen in July, August and September.

![ss2](/blog/assets/docs 2/Month.png)

When we split the data by day, there's slightly more offenses committed on Friday, and less on the weekend.

![ss3](/blog/assets/docs 2/Day.png)

When we look at the data by hour, there's some interesting high points and low points in the data. At Midnight, there seems to be quite a bit of offenses committed, and then there's significantly less during the early morning hours. By around the Noon hour is when is reaches it's peak before it lessens into the early evening. By around 4:00 - 6:00 is when the majority of offenses happen.

![ss4](/blog/assets/docs 2/Hour.png)

There are tons of streets in this dataset. I picked out the Top 10 Streets where Offenses are committed. **Washington Street** is the street with the vast majority of crimes committed. There's a street "name" that is considered a NULL in the dataset. I would assume that an offense is recorded but wasn't at named street or was missing from the data. 

![ss5](/blog/assets/docs 2/Street.png)

Lastly, I wanted to look at what are the Top 10 types of Offenses committed. Motor Vehicle Accident Response is the largest number of offenses followed by Larceny and Medical Assistance.

![ss6](/blog/assets/docs 2/Offense.png)

I created two SQL Server Reporting Services Reports. One has the ability to filter by Years and Months and shows the Number of Offenses by Offense. The other can filter by Years and Streets and shows Number of Offenses by Offense. I have the query that created the report dataset and an example screenshot below:

{% highlight sql %}
SELECT
	CASE WHEN YEAR IS NULL THEN 'All' ELSE CAST(YEAR AS VARCHAR) END AS 'YEAR',
	CASE
		WHEN MONTH = 1 THEN 'January'
		WHEN MONTH = 2 THEN 'February'
		WHEN MONTH = 3 THEN 'March'
		WHEN MONTH = 4 THEN 'April'
		WHEN MONTH = 5 THEN 'May'
		WHEN MONTH = 6 THEN 'June'
		WHEN MONTH = 7 THEN 'July'
		WHEN MONTH = 8 THEN 'August'
		WHEN MONTH = 9 THEN 'September'
		WHEN MONTH = 10 THEN 'October'
		WHEN MONTH = 11 THEN 'November'
		WHEN MONTH = 12 THEN 'December'
		WHEN MONTH IS NULL THEN 'All' 
		ELSE CAST(MONTH AS VARCHAR) 
	END AS 'MONTH',
	OFFENSE_CODE_GROUP,
	COUNT(*) AS 'Number of Offenses'
FROM [Datasets].[dbo].[Boston_Crimes]
GROUP BY 
GROUPING SETS (
	(YEAR, MONTH, OFFENSE_CODE_GROUP),
	(YEAR, OFFENSE_CODE_GROUP),
	(OFFENSE_CODE_GROUP)
)
ORDER BY YEAR, MONTH, OFFENSE_CODE_GROUP
{% endhighlight %}

![ss7](/blog/assets/docs 2/SSRS 1.png)

{% highlight sql %}
SELECT
	CASE WHEN YEAR IS NULL THEN 'All' ELSE CAST(YEAR AS VARCHAR) END AS 'YEAR',
	CASE WHEN STREET IS NULL THEN 'All' ELSE CAST(STREET AS VARCHAR) END AS 'STREET',
	OFFENSE_CODE_GROUP,
	COUNT(*) AS 'Number of Offenses'
FROM [Datasets].[dbo].[Boston_Crimes]
GROUP BY 
GROUPING SETS (
	(YEAR, STREET, OFFENSE_CODE_GROUP),
	(YEAR, OFFENSE_CODE_GROUP),
	(OFFENSE_CODE_GROUP)
)
ORDER BY YEAR, STREET, OFFENSE_CODE_GROUP

{% endhighlight %}

![ss8](/blog/assets/docs 2/SSRS 2.png)

Lastly, I created two Tableau Dashboards that shows the Yearly Trend of Offenses and a Heatmap of Offenses committed on the hour.

* [Boston Crimes Yearly Trend Dashboard](https://public.tableau.com/profile/charlie.dalldorf7293#!/vizhome/BostonCrimesYearlyTrend/LineGraphDashboard)
* [Boston Crimes Heatmap Dashboard](https://public.tableau.com/profile/charlie.dalldorf7293#!/vizhome/BostonCrimesHeatmap/HeatmapDashboard)

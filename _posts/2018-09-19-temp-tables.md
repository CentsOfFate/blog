---
layout: post
title: How To - Temporary Tables and Table Variables
---

As you acquire more skill and finesse with querying databases, you may approach a situation where your SQL code becomes unwieldy. Multiple Joins, GROUP BY aggregations, and subqueries  can make it difficult to read your query, especially if you come back to a project several months later. There are several ways to solve this problem, and we are going to explore two of those methods in this post: Temporary Tables and Table Variables.

## Temporary Tables

For these examples, I will be working off of the Powerlifting Database from Kaggle. For this query, I want to see all the lifters who competed in San Diego, California and are Male:

We went from having 300,000+ rows down to 936. Working from two joined tables, even with as small of a dataset (compared to the real world) as this one, is going to take some time for a query to run. If I started doing GROUP BYs or subqueries, SQL Server is going to start taking a while to complete my request.

Instead of querying off of the joined tables, we should put the data we want to work with in a temporary table, and work off of that temporary table as if it is a table that exists in the database. We can do this using this set of SQL Code:

{% highlight sql %}
-- This checks if the table exists, and deletes it
IF OBJECT_ID('tempdb.dbo.#PL') IS NOT NULL
 DROP TABLE dbo.#PL;
GO

-- We create a Temp table #PL and insert all the data from the joined tables query
SELECT *
INTO #PL
FROM (
 SELECT
 Meets.MeetName
 ,Meets.MeetState
 ,Meets.MeetTown
 ,Meets.Date
 ,PL.Name
 ,PL.Sex
 ,PL.Equipment
 ,PL.Age
 ,PL.Division
 ,PL.BodyweightKg
 ,PL.WeightClassKg
 ,PL.Squat4Kg
 ,PL.BestSquatKg
 ,PL.Bench4Kg
 ,PL.BestBenchKg
 ,PL.Deadlift4Kg
 ,PL.BestDeadliftKg
 ,PL.TotalKg
 ,PL.Place
 ,PL.Wilks
 FROM [Datasets].[dbo].[PowerLifting] AS PL
 JOIN [Datasets].[dbo].[PowerLifting_Meets] AS Meets
 ON PL.MeetID = Meets.MeetID
 WHERE MeetState = 'CA'
 AND MeetTown = 'San Diego'
 AND Sex = 'M'
) AS temp

-- Lastly, we do a SELECT * to see if it works and it does!
SELECT *
FROM #PL
{% endhighlight %}

Some people define the variables being inserted into a temporary table as if the table will exist permanently in the database. It is far faster to do a **SELECT * INTO #temp_table_name FROM ( SQL Query )** to fetch and place all your data into the temporary table.

Temporary Tables exist in a database called tempdb. They remain there until the current session is over. Once you exit out of SSMS and go back to the query you were working on, you have to re-execute your temporary table to get it again.

So let’s do a couple queries off of our Temporary Table. Let’s get the guys who have the highest squat at each meet, and get their name, age, body weight and bench.


{% highlight sql %}
SELECT
 T1.MeetName,
 T1.Name,
 T1.Age,
 T1.BodyweightKg,
 T2.[Max Squat (In Kg)],
 T1.BestBenchKg
FROM #PL AS T1
JOIN (
 SELECT
 MeetName,
 MAX(BestSquatKg) 'Max Squat (In Kg)'
 FROM #PL
 WHERE BestSquatKg IS NOT NULL
 GROUP BY MeetName
) AS T2
ON T1.MeetName = T2.MeetName AND T1.BestSquatKg = T2.[Max Squat (In Kg)]
{% endhighlight %}

That looks good. Let’s do one more. Out of the powerlifter that weigh less than 100 kg, what is their average Deadlift at each meet?

{% highlight sql %}
SELECT
 MeetName,
 ROUND(AVG(BestDeadliftkg), 2) 'Average Deadlift'
FROM #PL
WHERE BodyweightKg < 100
GROUP BY MeetName
{% endhighlight %}

Cool. As you can see, temporary tables make queries much easier to read when you are working with larger datasets.

## Table Variables

Table Variables function similarly to Temporary Tables. They also exist inside tempdb, but they have one caveat. You only exist within a batch of a SQL Query execute. Once the query is done running, the Table Variable will fall out of scope, even in the same session.

Let’s do a quick example. Let’s say I want to find all the lifters who can lift more than the average of all the meets from our Temporary Table. We can do this by declaring a table variable and setting it to hold the average bench press. Then we can use it in a WHERE statement to get our answer.


{% highlight sql %}
-- Declare a Variable
 DECLARE @AVGBench AS FLOAT
 SET @AVGBench = (
 SELECT
 ROUND(AVG(BestBenchKg), 2) AS 'Average Bench'
 FROM #PL
 )

SELECT *
 FROM #PL
 WHERE BestBenchKg > @AVGBench
{% endhighlight %}

And there we go! That’s a simple, but practical use of a Table Variable for our dataset!

## Summary
Temporary Tables and Table Variables are great ways to store data and simplify your T-SQL code. If you are working with a large dataset that has complex logic, you should throw some of your data into either of these. I guarantee you it will make your life easier for yourself now and into the future.

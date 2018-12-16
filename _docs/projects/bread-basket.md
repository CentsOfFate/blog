---
title: Bread Basket Market Analysis
permalink: /docs/bread-basket/
---

This is a dataset I downloaded from Kaggle a while back. I have tried to find it again to link to the page, but it may have been taken down or been removed. This dataset features 4 columns:

**Date**
**Time**
**Transaction**
**Item**

In total, there are 20000+ rows in the file. There's enough here to do some quick and dirty analysis. So the first thing I do is load this dataset into SQL Server and prep Jupyter Notebooks to make some visualizations to see how this data lays out. The first query I run and visualize is the Top 10 Items Sold at the Bakery.

{% highlight py %}
df = pd.read_sql_query('''
SELECT TOP (10)
	Item,
	COUNT(Item) AS  'Number of Items Ordered'
FROM [Datasets].[dbo].[Bakery_Data]	
GROUP BY Item
ORDER BY [Number of Items Ordered] DESC
''', conn)

# Create Figure, 19.20 x 10.80
fig = plt.figure(figsize = (19.20, 10.80), dpi = 300)

# Horizontal Bar Plot
ax = sns.barplot(x = "Number of Items Ordered", y = "Item",
                 data = df,
                 palette = "hls")

# Title, X-Axis Label, Y-Axis Label
plt.title('Bakery Market Basket Analysis \nTop 10 Items Ordered', size = 30, fontweight = 'bold')
plt.xlabel('Number of Items Ordered', size = 26, fontweight = 'bold')
plt.ylabel('Item Name', size = 26, fontweight = 'bold')

# Make X-Axis and Y-Axis Ticks larger
plt.xticks(size = 20)
plt.yticks(size = 20)

# Label Data Points
[ax.text(p[1] + 20, p[0]+.1, p[1], color = 'black', fontsize = 18) for p in zip(ax.get_yticks(), df['Number of Items Ordered'])]

{% endhighlight %}

![ss1](/blog/assets/docs 1/Basket Analysis - Top 10 Items Ordered.png)

What we learn when we aggregate the data is that Coffee, Bread and Tea are the three most bought items in the dataset. Interestingly, there's an item called NONE. I'm not entirely sure what that means in the context of this dataset. It could be a data item entry error, or maybe the dataset tracks customers that walk into the store and buy nothing. 

The next item I am curious about is the the number of transactions made on each month.

{% highlight py %}
df = pd.read_sql_query('''
SELECT
	MONTH(Date) AS 'Month (By Number)',
	CASE
		WHEN MONTH(Date) = 1 THEN 'Janurary'
		WHEN MONTH(Date) = 2 THEN 'February'
		WHEN MONTH(Date) = 3 THEN 'March'
		WHEN MONTH(Date) = 4 THEN 'April'
		WHEN MONTH(Date) = 10 THEN 'October'
		WHEN MONTH(Date) = 11 THEN 'November'
		WHEN MONTH(Date) = 12 THEN 'December'
		END AS 'Month Name',
	COUNT(Item) AS 'Number of Items Ordered per Month'
FROM [Datasets].[dbo].[Bakery_Data]
GROUP BY MONTH(Date)
ORDER BY MONTH(Date)
''', conn)

# Create Figure, 19.20 x 10.80
fig = plt.figure(figsize = (19.20, 10.80), dpi = 300)

# Horizontal Bar Plot
ax = sns.barplot(x = "Number of Items Ordered per Month", y = "Month Name",
                 data = df,
                 palette = "hls")

# Title, X-Axis Label, Y-Axis Label
plt.title('Bakery Market Basket Analysis \nNumber of Orders by Month', size = 30, fontweight = 'bold')
plt.xlabel('Number of Items Ordered', size = 26, fontweight = 'bold')
plt.ylabel('Month', size = 26, fontweight = 'bold')


# Make X-Axis and Y-Axis Ticks larger
plt.xticks(size = 20)
plt.yticks(size = 20)

{% endhighlight %}

![ss2](/blog/assets/docs 1/Basket Analysis - Items Ordered by Month.png)

This dataset has little data in April and October. However, November and March are the months where the most number of transactions are made.

Lastly, I want at what time of day orders are being made. The time is set to be military time.

{% highlight py %}
df = pd.read_sql_query('''
SELECT
	DATEPART(Hour, [Time]) AS 'Hours in a Day',
	COUNT(Item) AS 'Number of Items Ordered by Hour'
FROM [Datasets].[dbo].[Bakery_Data]
GROUP BY DATEPART(Hour, [Time])
ORDER BY DATEPART(Hour, [Time])
''', conn)

# Create Figure, 19.20 x 10.80
fig = plt.figure(figsize = (19.20, 10.80), dpi = 300)

# Horizontal Bar Plot
ax = sns.barplot(x = "Hours in a Day", y = "Number of Items Ordered by Hour",
                 data = df,
                 palette = "hls")

# Title, X-Axis Label, Y-Axis Label
plt.title('Bakery Market Basket Analysis \nNumber of Orders by Hour of Day', size = 30, fontweight = 'bold')
plt.xlabel('Hours in a Day', size = 26, fontweight = 'bold')
plt.ylabel('Number of Items Ordered', size = 26, fontweight = 'bold')


# Make X-Axis and Y-Axis Ticks larger
plt.xticks(size = 20)
plt.yticks(size = 20)

{% endhighlight %}

![ss3](/blog/assets/docs 1/Basket Analysis - Number of Orders by Hour.png)

Most transactions take place around the noon/lunch hour. There is a large drop off in transactions before 9:00 and after 15:00.

To finish up my project, I took all the graphs I made with Seaborn/Matplotlib and put them all into Tableau Dashboards. Those dashboards can be found by clicking on this link:

https://public.tableau.com/shared/C52R6J7N4?:display_count=yes


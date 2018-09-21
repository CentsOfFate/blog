---
layout: post
title: Graphing with Matplotlib and Seaborn (Part 1 of 2)
---

Data visualization is one of the most important aspects to data analysis. Plotting points or lines on a graph is one thing, but having an audience understand your analysis is another. Making graphs is not that difficult, but making good graphs is an art form. In this multi-part blog series, we will be working with Python’s **Matplotlib** and Seaborn graphing libraries. I will demonstrate the breadth of plotting tools and customization as well as some style techniques that the audience you may be presenting to will appreciate.

The Matplotlib library is one of the mainstays in Python. The depth of options it has is nearly endless. However, the default graphs leave much to be desired:

![ss1](/omega/assets/post 1/basic.png)

One of Python’s newer libraries, Seaborn, can turn our above graph from uninspiring to impressive.

![ss2](/omega/assets/post 1/Graph5.png)

Seaborn is a Python library built on top of matplotlib. The library is designed to streamline the process of making graphs. Even right out of the box, Seaborn makes your run of the mill matplotlib plots look a lot better. Combining the customization of matplotlib and the design of Seaborn, you will have the ability to make stunning graphs.

If you don’t have Seaborn installed, you can get it with two ways. The first is with pip:

<div class="message">
  pip install seaborn
</div>

Or you can use Anaconda:

<div class="message">
  conda install seaborn
</div>

## Working with Matplotlib and Seaborn

Once you have Seaborn installed, all you have to do is import Seaborn and other libraries:

{% highlight py %}
import numpy as np
import pandas as pd
from matplotlib import pyplot as plt
import seaborn as sns
{% endhighlight %}

The data set I am going to work with is based on the North American League of Legends Championship Series. Feel free to work off of the Dataframe created below or use your own data that you feel comfortable with.

{% highlight py %}
df = pd.DataFrame({
'Champion': ["Kha'zix", "Jhin", "Ashe", "Varus", "Maokai", "Nautilus", "Karma", "Rengar", "Orianna", "Malzahar"],
'Number of Matches Picked': [106, 102, 100, 97, 92, 92, 89, 78, 71, 67]})
{% endhighlight %}

The first thing we should do is get a Seaborn barplot:

{% highlight py %}
fig = plt.figure(figsize = (15, 8), dpi = 100)

ax = sns.barplot(x = "Champion", y = "Number of Matches Picked",
 data = df,
 palette = "hls")
{% endhighlight %}


**plt.figure** controls how big the graph is, the DPI (Dots per Inch) and how many subplots we want. For now, we just want a reasonable sized graph that’s not too high quality.

**sns.barplot** is what we use to graph our bar graph. We select our X-Axis and Y-Axis based on our columns from our dataframe. **data = df** means that Seaborn knows to use the columns from our dataframe. We will touch on **palette** in a little bit. So let’s take a look at our graph!

![ss3](/omega/assets/post 1/Graph1.png)

It’s already looking a lot better! There’s still a lot more we can add to it before we can call it good. Next, we should add Titles.

{% highlight py %}
# Title, X-Axis Label, Y-Axis Label
plt.title('North American LCS: Top 10 Champions Picked', size = 28, fontweight = 'bold')
plt.xlabel('Champion', size = 20, fontweight = 'bold')
plt.ylabel('Number of Matches Picked', size = 20, fontweight = 'bold')
{% endhighlight %}

![ss4](/omega/assets/post 1/Graph2.png)


We have big bold titles now! But, our text seems a bit off, and our graph seems unusually plain. Seaborn has the ability to add what is called a Style. With just one line of code:

{% highlight py %}
# Set Style
sns.set_style('whitegrid')
{% endhighlight %}

![ss4](/omega/assets/post 1/Graph3.png)

We can get Y-Axis grid lines in the background of the page. Also, whitegrid gave us some better looking font for our titles too. We just have a couple more adjustments to go. Next, let’s make our X-Axis and Y-Axis text bigger and change our Y-Axis Range.


{% highlight py %}
# Make X-Axis and Y-Axis Ticks larger
plt.xticks(size = 18)
plt.yticks(size = 18)

# Set Y-Axis Ticks Range
plt.ylim(0, 120)
{% endhighlight %}

![ss4](/omega/assets/post 1/Graph4.png)

We are almost there. The last part is to put Data Labels above each bar. Here is how you do that in a single line:

{% highlight py %}
# Make X-Axis and Y-Axis Ticks larger
plt.xticks(size = 18)
plt.yticks(size = 18)

# Set Y-Axis Ticks Range
# Label Data Points
[ax.text(p[0] - .13, p[1] + 2, p[1], color = 'black', fontsize = 18) for p in zip(ax.get_xticks(), df['Number of Matches Picked'])]
{% endhighlight %}

![ss4](/omega/assets/post 1/Graph5.png)

For this last portion, we run a For Loop for all of the **df[‘Number of Matches Picked’]** values and write text based on the X-Axis and Y-Axis Position.

## Other Matplotlib/Seaborn Graphs

Matplotlib/Seaborn can create more than just bar graphs. These two libraries can create line graphs, boxplots, heatmaps and much more. Let’s look through some examples (with code).

### Line Graph Example
{% highlight py %}
# Make X-Axis and Y-Axis Ticks larger
plt.xticks(size = 18)
plt.yticks(size = 18)

# Set Y-Axis Ticks Range
# Create Dataframe
df = pd.DataFrame({'Academic_Year' : ['2013-2014', '2014-2015', '2015-2016', '2016-2017', '2017-2018',
 '2013-2014', '2014-2015', '2015-2016', '2016-2017', '2017-2018'],
 'Proficiency' : [0.5124, 0.5234, 0.5465, 0.5342, 0.5621,
 0.5891, 0.6003, 0.5763, 0.5697, 0.5751],
 'Type' : ['English Language Arts', 'English Language Arts', 'English Language Arts', 'English Language Arts', 'English Language Arts',
 'Math', 'Math', 'Math', 'Math', 'Math']})

# Set Style to White Grid
 sns.set_style('whitegrid')

# Set Custom Color Palette
 custom = ["#DC4C46", "#3b86c6", "#95a5a6", "#e74c3c", "#34495e", "#2ecc71"]
 sns.set_palette(custom)

# Set Figure Size and DPI
 fig = plt.figure(figsize = (19.20, 10.80), dpi = 300)

# Plot Points, set scale
 ax = sns.pointplot(x = "Academic_Year", y = "Proficiency", hue = "Type",
 data = df,
 scale = 2)

# Title, X-Axis Label, Y-Axis Label
 plt.title('State Level Smarter Balanced 5 Year Trend', size = 28, fontweight = 'bold')
 plt.xlabel('Academic Year', size = 20, fontweight = 'bold')
 plt.ylabel('Percent Proficient', size = 20, fontweight = 'bold')

# Ticks larger
 plt.xticks(size = 18)
 plt.yticks(size = 18)

# Set Ticks
 plt.ylim(0.50, 0.64)

# Set Y-Axis to be in Percentage Form
 from matplotlib.ticker import FuncFormatter
 ax.yaxis.set_major_formatter(FuncFormatter(lambda y, _: '{:.0%}'.format(y)))

# Set Legend Location and Size
 ax.legend(loc = 2, fontsize = 18)

# Grab Percents for ELA and Math
 elalist = df['Proficiency'].head(5)
 mathlist = df['Proficiency'].tail(5)

# Label Data Points
 [ax.text(p[0] - .15, p[1] +.005, round(p[1] * 100, 2), color = 'black', fontsize = 18) for p in zip(ax.get_xticks(), elalist)]
 [ax.text(p[0] - .004, p[1] +.006, round(p[1] * 100, 2), color = 'black', fontsize = 18) for p in zip(ax.get_xticks(), mathlist)]

# Show Graph and Dataframe
 plt.show()
{% endhighlight %}

![ss6](/omega/assets/post 1/LineGraph.png)

Using a lot of the same functions from the Bar Graph example, we can create line graphs that are simple to read and understand. This line graph shows State Level proficiency on the Smarter Balanced Assessment for English Language Arts and Math (fake data).

### Box Plot Example
{% highlight py %}
# Set Style to White Grid
sns.set_style('whitegrid')

df = pd.read_sql_query('''
SELECT
 [gameid]
 ,[league]
 ,[split]
 ,[date]
 ,[week]
 ,[game]
 ,[side]
 ,[position]
 ,[player]
 ,[gamelength]
 ,[result]
 ,[k]
 ,[d]
 ,[a]
 ,[totalgold] AS 'Total Gold'
 FROM [NA LCS].[dbo].[Mart_Players]
 WHERE position IN ('ADC', 'Middle')
''', conn)

# Create Two Seperate Dataframes to plot
dfMid = df.loc[df['position'] == 'Middle']
dfADC = df.loc[df['position'] == 'ADC']

# Create Figure and Subplots
fig, ax = plt.subplots(1, 2, figsize=(15,8))

sns.boxplot(x = "position", y = "k",
 data = dfMid,
 ax = ax[0],
 color = '#3b86c6')

sns.boxplot(x = "position", y = "k",
 data = dfADC,
 ax = ax[1],
 color = '#DC4C46')

for i in range(2):
 ax[i].set_ylim(0, 16)
 ax[i].set_ylabel('Number of Kills', fontsize = 14, fontweight = 'bold')
 ax[i].set_xlabel('Position', fontsize = 14, fontweight = 'bold')
 ax[i].tick_params(labelsize = 12)

# Write Titles for each
ax[0].set_title('Middle', fontsize = "x-large", fontweight = 'bold')
ax[1].set_title('ADC', fontsize = "x-large", fontweight = 'bold')

# Big Title
fig.suptitle("2017 North American LCS Kills Boxplots",
 fontsize = "xx-large", fontweight = 'bold')

plt.show()
{% endhighlight %}

![ss6](/omega/assets/post 1/Box.png)

Using the 2017 North American League of Legends Championship Series data, we can plot Box Plots of player stats for each match. The Box Plots shown above are Kills Per Match for Mid-Lane (Left) and AD Carry (Right).

In this plot, we introduce the ability  to plot two graphs in the same “plot”. This is done with:

{% highlight py %}
fig, ax = plt.subplots(1, 2, figsize=(15,8))
{% endhighlight %}

Then, we have to specify which of Seaborn’s plotting functions goes to which plot. We denote this with ax = ax[0] (left plot) or ax = ax[1] (right plot). This also means we have to set Titles, X-Axis, and Y-Axis formatting for both plots. To simplify this, we used a For Loop to run through all the functions in each graph.

### Linear Regression Example
{% highlight py %}
# Load Data
df = pd.read_csv('Pokemon.csv')

# Set Style to White Grid
sns.set_style('whitegrid')

# Create a 2 x 2 Figure with Figsize (20, 15)
fig, ax = plt.subplots(ncols = 2, nrows = 2, figsize = (20, 15), dpi = 100)

# Create Lists to loop through
y_col = ['Defense', 'Speed', 'Sp. Def', 'Sp. Atk']
y_names = ['Defense', 'Speed', 'Special Defense', 'Special Attack']

# For Loop X-Axis Label, Tick Size and X & Y axis Limits
for i in range(2):
 for j in range(2):
 sns.regplot(x = 'Attack', y = y_col[i+i+j], data = df, ax = ax[i][j])
 ax[i][j].set_xlabel('Attack Base Stat', fontsize = 14, fontweight = 'bold')
 ax[i][j].set_ylabel(y_names[i+i+j] + ' Base Stat', fontsize = 14, fontweight = 'bold')
 ax[i][j].set_title('Attack x ' + y_names[i+i+j], fontsize = 18, fontweight = 'bold')
 ax[i][j].tick_params(labelsize = 12)
 ax[i][j].set_xlim(0, 200)
 ax[i][j].set_ylim(0, 250)

# Create Big Title
plt.suptitle('Pokemon Linear Regression Attack x Other Stats', fontsize = 24, fontweight = 'bold')
{% endhighlight %}

Using the base stats of Pokemon, we can create a Linear Regression. Just like the Box Plots, we can create multiple subplots. This time, we create a 2 x 2 set using:

{% highlight py %}
fig, ax = plt.subplots(ncols = 2, nrows = 2, figsize = (20, 15), dpi = 100)
{% endhighlight %}

![ss6](/omega/assets/post 1/Quad Lin Reg.png)

We also use a For Loop for everything except plt.suptitle. We create two lists to loop through variables in order to plot and format our Linear Regression plots.

### Distribution Plot Example

{% highlight py %}
# Set Style to White Grid
sns.set_style('whitegrid')

df = pd.read_sql_query('''
SELECT
 T2.nameFirst AS 'First Name'
 ,T2.nameLast AS 'Last Name'
 ,[yearID] AS 'Year'
 ,[H] AS 'Hits Times Reached to Base'
 ,[2B] AS 'Hits Made to Second Base'
 ,[3B] AS 'Hits Made to Third Base'
 ,[HR] AS 'Home Run'
FROM [Lahman Baseball].[dbo].[Batting] AS T1
INNER JOIN (
 SELECT
 playerID,
 nameFirst,
 nameLast
 FROM [Lahman Baseball].[dbo].[People]
) AS T2
ON T1.playerID = T2.playerID
WHERE yearID = 2016
AND [H] > 0
''', conn)

# Figure Size
plt.figure(figsize = (15, 8), dpi = 100)

# Plot Distplot
sns.distplot(df['Hits Times Reached to Base'], color = 'blue')

# Title, X-Axis Label, Y-Axis Label
plt.title('2016 Major League Baseball Hits Distribution Plot', size = 20, fontweight = 'bold')
plt.xlabel('Count', size = 16, fontweight = 'bold')
plt.ylabel('Proportion', size = 16, fontweight = 'bold')

# Ticks larger and X-Axis frequency to 25
plt.xticks(size = 12)
plt.xticks(np.arange(0, 275, 25))
plt.yticks(size = 12)

# Set Ticks
plt.xlim(0, 275)
plt.ylim(0, 0.025)
{% endhighlight %}

For the last example, we show a Distribution Plot for the 2016 MLB Season for Hits. Normally, this plot has X-Axis Ticks set for a frequency of 50. But using this:

{% highlight py %}
plt.xticks(np.arange(0, 275, 25))
{% endhighlight %}

![ss6](/omega/assets/post 1/Distplot.png)

We can arrange the the X-Axis so it’s ticks are being measured at 25.

## Summary
Matplotlib and Seaborn are robust graphing libraries for Python. They allow a near endless supply of customization and creativity in visualizing data from Excel Spreadsheets, CSVs or Databases. I’ve hardly scratched the surface of what these libraries can do. Feel free to read up on [Matplotlib Documentation](https://matplotlib.org/) and [Seaborn Documentation](https://seaborn.pydata.org/) to see what these libraries are capable of.

For part 2 of this blog series, we will review concepts for presenting data to an audience as well as avoiding pitfalls when showing visuals to an audience.

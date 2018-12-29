---
layout: post
title: How To - Horizontal Bar Graphs
---

Cole Knaflic's book *Storytelling with Data* is a great book about creating simple visualizations that maximizes showing the data to the audience and minimizes distractions. Her book encourages the data analyst to take a deeper look into their data in terms of what they are trying to show their audience, and then designing the visual to help tell the story.

For the next couple of blog posts, I'm going to be improving the stylistic elements of the graphs I made in the past by implementing what she recommends in her book. I am going to continue using **Matploblib** and **Seaborn**, however Seaborn is going to be taking more of a backseat since Seaborn's graphs are tougher to customize. 

The first improvement we will be making is creating **Horizontal Bar Graphs**. The data set I will be using is Boston Crimes data from Kaggle: https://www.kaggle.com/ankkur13/boston-crime-data

First thing I do is load the dataset in SQL Server so I can perform data transformations and investigating the data. Since I want to focus on styling Bar Graphs, I will skip this part of the process.

First, let's start off by making our graph. I will be querying 2018 Crimes that happened on a Monday and selecting the Top 5 Crimes in Descending Order. I will be using Matplotlibs **BarH** method to create the bar graph rather than Seaborn's **Barplot**. The reason for this is is the ability to change the width size of the bars, and to make sure that the bars have a zorder above the vertical lines. Doing this requires a bit more work in assigning labels to the Y-Axis. In order to accomplish this, we need to assign all values to a numpy array **Offenses** and we have the arrangement based on the length of the array in **y_pos**. These two arrays will be used to create the **plt.yticks**. To top it off, we assign the Color Palette to **husl** and style to **whitegrid**.

{% highlight py %}
df = pd.read_sql_query('''
WITH CTE AS (
	SELECT TOP (5)
		Year,
		DAY_OF_WEEK,
		OFFENSE_DESCRIPTION,
		CAST(COUNT(*) AS INT) AS 'Number of Offenses'
	FROM [Datasets].[dbo].[Boston_Crimes]
	WHERE YEAR = 2018
	AND DAY_OF_WEEK = 'Monday'
	GROUP BY OFFENSE_DESCRIPTION, DAY_OF_WEEK, Year
	ORDER BY [Number of Offenses] DESC
)

SELECT *
FROM CTE
ORDER BY [Number of Offenses] ASC

''', conn)

# Color Palette
sns.set_palette('husl')

# Set Seaborn Graph Style
sns.set_style('whitegrid')

# Plot using Dataframe.Plot. Allows Width, Horizontal Bar and more flexibility
ax = df.plot(y = 'Number of Offenses', width = 0.50,
             figsize=(19.20, 10.80), zorder = 2,
             kind = 'barh',
             legend = False)

# Get all Offenses and Numerical Positions
Offenses = df['OFFENSE_DESCRIPTION'].values
y_pos = np.arange(len(Offenses))


# Title, x-axis, y-axis
plt.title('Boston - Number of Offenses in 2018 that occured on Monday', size = 28, fontweight = 'bold')
plt.xlabel('Number of Offenses', size = 20, fontweight = 'bold')
plt.ylabel('', size = 20, fontweight = 'bold')

# Ticks larger
plt.xticks(size = 18)
plt.yticks(size = 18)
plt.yticks(y_pos, Offenses)

{% endhighlight %}

![ss1](/blog/assets/post 3/1.png)

Honestly, this graph is already pretty good as it is. We have bold Arial font for the Title and Labels. It's decently sized at 19.20 x 10.80 as the figure size. The color defaults are pleasing to the eye. 

However, Cole would argue that my graph has too many distractions and I am not focusing the audience's attention enough. What is the real story here? Why did I choose the Top 5 Number of Offenses on a Monday? What point am I getting across? All of these are good questions, and we can probably answer all of them if we trim some fat off the graph.

So let's start by changing the the Seaborn Whitegrid Style to White, despining all axis except for the left axis and putting lighter vertical axis cutoffs:

{% highlight py %}
# Set Seaborn Graph Style
sns.set_style('white')

# Despine
sns.despine(bottom=True)

# Draw vertical axis lines
vals = [200, 400, 600, 800, 1000]
for tick in vals:
    ax.axvline(x=tick, alpha=1, color='#eeeeee', zorder=1)

{% endhighlight %}

![ss2](/blog/assets/post 3/2.png)

We already see good improvements on our graph. It's a lot less busy now, and we can get a clearer picture of the data. Let's say the story we want to tell is to show the audience that Sick/Injured/Medical is the top offense in Boston. We can do this by having that category be the only one with a color and grey out the rest. While we are at it, let's soften the font colors so they are more of a dark-grey than a black so we can reduce the sharp contrast. And let's also put numbers on the end of the bars so we can be precise:

{% highlight py %}
# Color Palette
ui = ["#c3c3c3", "#c3c3c3", "#c3c3c3", "#c3c3c3", "#3b86c6"]
sns.set_palette(ui)

# Labels Horizontal Bars
for x, y in enumerate(df['Number of Offenses']):
    ax.text(y + 10, x - .06, str(y), fontweight='bold', fontsize = 20, color = '#484848')    

{% endhighlight %}

![ss3](/blog/assets/post 3/Boston Crimes (Vertical Cutoffs).png)

Now we have a Bar Chart! We have minimized a lot of the non-data ink that we had from the first graph and used color to show what is the most important data point in the graph.

We can go one step further, and that is knocking down all the axis and having only the bars and number of offenses at the end of the bars. This is a stylistic choice if you want to go that minimalist:

![ss4](/blog/assets/post 3/Boston Crimes (Only Bars).png)

And the full code that goes with it:

{% highlight py %}
df = pd.read_sql_query('''
WITH CTE AS (
	SELECT TOP (5)
		Year,
		DAY_OF_WEEK,
		OFFENSE_DESCRIPTION,
		CAST(COUNT(*) AS INT) AS 'Number of Offenses'
	FROM [Datasets].[dbo].[Boston_Crimes]
	WHERE YEAR = 2018
	AND DAY_OF_WEEK = 'Monday'
	GROUP BY OFFENSE_DESCRIPTION, DAY_OF_WEEK, Year
	ORDER BY [Number of Offenses] DESC
)

SELECT *
FROM CTE
ORDER BY [Number of Offenses] ASC

''', conn)

# Set Custom Color Palette
flatui = ["#c3c3c3", "#c3c3c3", "#c3c3c3", "#c3c3c3", "#3b86c6"]
sns.set_palette(flatui)

# Set Seaborn Graph Style
sns.set_style('white')

# Plot using Dataframe.Plot. Allows Width, Horizontal Bar and more flexibility
ax = df.plot(y = 'Number of Offenses', width = 0.50,
             figsize=(19.20, 10.80), zorder = 2,
             kind = 'barh',
             legend = False)

# Title, x-axis, y-axis
plt.title('Boston - Number of Offenses in 2018 that occured on Monday', size = 28, fontweight = 'bold', color = '#484848')
plt.xlabel('', size = 20, fontweight = 'bold')
plt.ylabel('', size = 20, fontweight = 'bold')

# Y-Axis Values and Arrangement
Offenses = df['OFFENSE_DESCRIPTION'].values
y_pos = np.arange(len(Offenses))

# Ticks larger
plt.xticks([])
plt.yticks(size = 18, color = '#484848')
plt.yticks(y_pos, Offenses)

# X-Axis Range
plt.xlim(0, 1000)

# Labels Horizontal Bars
for x, y in enumerate(df['Number of Offenses']):
    ax.text(y + 10, x - .06, str(y), fontweight = 'bold', fontsize = 20, color = '#484848')

# Despine Everything
sns.despine(left = True, bottom = True)

{% endhighlight %}

That was really fun! Next, we are going to tackle a graph that I haven't had any experience with before: The Slope Graph. See you next time!
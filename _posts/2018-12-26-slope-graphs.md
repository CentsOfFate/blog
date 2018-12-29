---
layout: post
title: How To - Slope Graphs
---

Cole Knaflic's book *Storytelling with Data* is a great book about creating simple visualizations that maximizes showing the data to the audience and minimizes distractions. Her book encourages the data analyst to take a deeper look into their data in terms of what they are trying to show their audience, and then designing the visual to help tell the story.

For the next couple of blog posts, I'm going to be improving the stylistic elements of the graphs I made in the past by implementing what she recommends in her book. I am going to continue using **Matploblib** and **Seaborn**, however Seaborn is going to be taking more of a backseat since Seaborn's graphs are tougher to customize. 

For this blog post we will be making is creating **Slope Graph**. I have never heard of a slope graph until I read Cole's book. Essentially it's a line graph where all but the bottom axis is knocked down, and the legend is part of the "Y-Axis". The only points labeled on a Slope Graph is the values on the first and last points. This will make more sense in a bit. The data set I will be using is Boston Crimes data from Kaggle: https://www.kaggle.com/ankkur13/boston-crime-data

First, let's get our data from SQL Server. It's my preference do data transformations and aggregates using SQL and putting it into a dataframe. Next, we will create a Seaborn **Pointplot** with a scale of 2. We will title everything, give everything a default dark grey color. We will use a **husl** color palette and a **whitrgrid** style. This should give us a satisfactory line graph:

{% highlight py %}
df = pd.read_sql_query('''
WITH CTE AS (
	SELECT
		YEAR,
		OFFENSE_DESCRIPTION,
		CAST(COUNT(*) AS INT) AS 'Number of Offenses'
	FROM [Datasets].[dbo].[Boston_Crimes]
	WHERE OFFENSE_DESCRIPTION IN (
		'ASSAULT - AGGRAVATED - BATTERY',
		'BURGLARY - RESIDENTIAL - NO FORCE',
		'LARCENY SHOPLIFTING',
		'M/V - LEAVING SCENE - PROPERTY DAMAGE'
	)
	GROUP BY YEAR, OFFENSE_DESCRIPTION
)

SELECT *
FROM CTE
ORDER BY OFFENSE_DESCRIPTION, YEAR
''', conn)

# White Canvas
sns.set_style('whitegrid')

# Set Custom Color Palette
sns.set_palette('husl')

# Set Color for Text to be #525252
colortext = '#525252'

plt.figure(figsize = (19.20, 10.80), dpi = 300)
ax = sns.pointplot(x = "YEAR", y = "Number of Offenses", hue = "OFFENSE_DESCRIPTION",
                   data = df,
                   scale = 2)

# Title, x-axis, y-axis
plt.title('Boston Crimes across 4 Years', size = 24, fontweight = 'bold', color = colortext, loc = 'center')
plt.xlabel('Years', size = 20, fontweight = 'bold', color = colortext)
plt.ylabel('Number of Offenses', size = 20, fontweight = 'bold', color = colortext)

# Ticks larger and give it light gray color
plt.yticks(size = 18, color = colortext)
plt.xticks(size = 18, color = colortext)
ax.spines['bottom'].set_color('#c3c3c3')

# Set Limit to 0.0 - 4.00
plt.xlim(-1.5, 4.00)
plt.ylim(0.0, 6000)

# Legend
ax.legend(loc = 2, fontsize = 14)

# Need to set Values in Array
np.array([])

# Show Graph and Dataframe
plt.show()

{% endhighlight %}

![ss1](/blog/assets/post 4/L1.png)

Currently, this graph is pretty solid. But, if we are to create a Slope Graph, we are going to have to make some drastic changes. When creating a Slope Graph for the first time, it took me quite a while to figure out how to do everything as efficiently as possible. What needs to happen is multiple things:

* Only have a bottom axis that shows the time span
* Remove the Legend and have the "legend" annotated to the first point in the graph
* Color one line to be focused on; All text and numbers must be the same color.

I have to apply a lot of changes between the current one and the next one otherwise the graph will look strange. I will walkthrough step-by-step all of the changes needed to go from a Line Graph to a Slope Graph.

First, we need to remove all axis except for the bottom, remove the Y-Axis ticks and remove the legend. Let's all change the Seaborn graph to **White**:


{% highlight py %}
# Seaborn Style Graph
sns.set_style('white')

# Set Custom Color Palette
ui = ['#c3c3c3', '#c3c3c3', '#c3c3c3', '#3b86c6']
sns.set_palette(ui)

# Remove X-Ticks
plt.yticks([])
ax.set_yticks([])

# Despine
sns.despine(top = True, left = True)

# Legend
ax.legend('')
{% endhighlight %}

Next, I will create two string arrays with the "Legend" names:

{% highlight py %}
# Need to set Values in Array
Off_Array = ['M/V - LEAVING SCENE - PROPERTY DAMAGE']
Grey_Array = ['ASSAULT - AGGRAVATED - BATTERY',
              'BURGLARY - RESIDENTIAL - NO FORCE',
              'LARCENY SHOPLIFTING']
{% endhighlight %}

The next step was where I started to run into trouble. Being able to programatically annotate the numbers on the sides of the first and last points of the graph requires either changing the current dataframe or running more queries to for loop the intended values to loop through. I went with the latter.

{% highlight py %}
color2015 = pd.read_sql_query('''
SELECT
	YEAR,
	OFFENSE_DESCRIPTION,
	CAST(COUNT(*) AS INT) AS 'Number of Offenses'
FROM [Datasets].[dbo].[Boston_Crimes]
WHERE OFFENSE_DESCRIPTION IN (
	'M/V - LEAVING SCENE - PROPERTY DAMAGE'
)
AND YEAR = 2015
GROUP BY YEAR, OFFENSE_DESCRIPTION
''', conn)

grey2015 = pd.read_sql_query('''
SELECT
	YEAR,
	OFFENSE_DESCRIPTION,
	CAST(COUNT(*) AS INT) AS 'Number of Offenses'
FROM [Datasets].[dbo].[Boston_Crimes]
WHERE OFFENSE_DESCRIPTION IN (
		'ASSAULT - AGGRAVATED - BATTERY',
		'BURGLARY - RESIDENTIAL - NO FORCE',
		'LARCENY SHOPLIFTING'
)
AND YEAR = 2015
GROUP BY YEAR, OFFENSE_DESCRIPTION
''', conn)

{% endhighlight %}

Now that I have two dataframes designed specifically to label the first points on the graph, I annotate the points with the values for the 2015 Number of Offenses for that category. The actual labelling includes adding the string array name with the value as a string.

{% highlight py %}
# Annotate and color M/V as Blue
for i, v in enumerate(color2015['Number of Offenses']):
    ax.text(0 - 0.1, v - 70,
            Off_Array[i] + ' ' + str(v),
            color='#3b86c6', fontweight='bold', fontsize = 16, horizontalalignment = 'right') 

# Annotate Other 3 as Grey
for i, v in enumerate(grey2015['Number of Offenses']):
    ax.text(0 - 0.1, v - 70,
            Grey_Array[i] + ' ' + str(v),
            color='#c3c3c3', fontweight='bold', fontsize = 16, horizontalalignment = 'right')
{% endhighlight %}

![ss2](/blog/assets/post 4/L2.png)

Wow, that's a quite a bit of change from our previous graph! There is significantly less non-data ink and we are now focusing on the lines themselves. 

There are a couple changes we need to make before it's fully a Slope Graph. We need to now annotate the points on the other side of the graph. We also need to left-align the title.

![ss3](/blog/assets/post 4/L3.png)

And heres all the code for our Slope Graph:

{% highlight py %}
df = pd.read_sql_query('''
WITH CTE AS (
	SELECT
		YEAR,
		OFFENSE_DESCRIPTION,
		CAST(COUNT(*) AS INT) AS 'Number of Offenses'
	FROM [Datasets].[dbo].[Boston_Crimes]
	WHERE OFFENSE_DESCRIPTION IN (
		'ASSAULT - AGGRAVATED - BATTERY',
		'BURGLARY - RESIDENTIAL - NO FORCE',
		'LARCENY SHOPLIFTING',
		'M/V - LEAVING SCENE - PROPERTY DAMAGE'
	)
	GROUP BY YEAR, OFFENSE_DESCRIPTION
)

SELECT *
FROM CTE
ORDER BY OFFENSE_DESCRIPTION, YEAR
''', conn)

color2015 = pd.read_sql_query('''
SELECT
	YEAR,
	OFFENSE_DESCRIPTION,
	CAST(COUNT(*) AS INT) AS 'Number of Offenses'
FROM [Datasets].[dbo].[Boston_Crimes]
WHERE OFFENSE_DESCRIPTION IN (
	'M/V - LEAVING SCENE - PROPERTY DAMAGE'
)
AND YEAR = 2015
GROUP BY YEAR, OFFENSE_DESCRIPTION
''', conn)

grey2015 = pd.read_sql_query('''
SELECT
	YEAR,
	OFFENSE_DESCRIPTION,
	CAST(COUNT(*) AS INT) AS 'Number of Offenses'
FROM [Datasets].[dbo].[Boston_Crimes]
WHERE OFFENSE_DESCRIPTION IN (
		'ASSAULT - AGGRAVATED - BATTERY',
		'BURGLARY - RESIDENTIAL - NO FORCE',
		'LARCENY SHOPLIFTING'
)
AND YEAR = 2015
GROUP BY YEAR, OFFENSE_DESCRIPTION
''', conn)

color2018 = pd.read_sql_query('''
SELECT
	YEAR,
	OFFENSE_DESCRIPTION,
	CAST(COUNT(*) AS INT) AS 'Number of Offenses'
FROM [Datasets].[dbo].[Boston_Crimes]
WHERE OFFENSE_DESCRIPTION IN (
	'M/V - LEAVING SCENE - PROPERTY DAMAGE'
)
AND YEAR = 2018
GROUP BY YEAR, OFFENSE_DESCRIPTION
''', conn)

grey2018 = pd.read_sql_query('''
SELECT
	YEAR,
	OFFENSE_DESCRIPTION,
	CAST(COUNT(*) AS INT) AS 'Number of Offenses'
FROM [Datasets].[dbo].[Boston_Crimes]
WHERE OFFENSE_DESCRIPTION IN (
		'ASSAULT - AGGRAVATED - BATTERY',
		'BURGLARY - RESIDENTIAL - NO FORCE',
		'LARCENY SHOPLIFTING'
)
AND YEAR = 2018
GROUP BY YEAR, OFFENSE_DESCRIPTION
''', conn)

# White Canvas
sns.set_style('white')

# Set Custom Color Palette
ui = ['#c3c3c3', '#c3c3c3', '#c3c3c3', '#3b86c6']
sns.set_palette(ui)

# Set Color for Text to be #525252
colortext = '#525252'

plt.figure(figsize = (19.20, 10.80), dpi = 300)
ax = sns.pointplot(x = "YEAR", y = "Number of Offenses", hue = "OFFENSE_DESCRIPTION",
                   data = df,
                   scale = 2)

# Title, x-axis, y-axis
plt.title('Boston Crimes across 4 Years', size = 24, fontweight = 'bold', color = colortext, loc = 'left')
plt.xlabel('Years', size = 20, fontweight = 'bold', color = colortext)
plt.ylabel('')

# Ticks larger and give it light gray color
plt.yticks(size = 18, color = colortext)
plt.xticks(size = 18, color = colortext)
ax.spines['bottom'].set_color('#c3c3c3')

# Set Limit to 0.0 - 0.60
plt.xlim(-1.5, 4.00)
plt.ylim(0.0, 6000)

# Legend
ax.legend('')

# Need to set Values in Array
Off_Array = ['M/V - LEAVING SCENE - PROPERTY DAMAGE']
Grey_Array = ['ASSAULT - AGGRAVATED - BATTERY',
              'BURGLARY - RESIDENTIAL - NO FORCE',
              'LARCENY SHOPLIFTING']

# Remove X-Ticks
plt.yticks([])
ax.set_yticks([])

# Despine
sns.despine(top = True, left = True)

# Annotate and color M/V as Blue
for i, v in enumerate(color2015['Number of Offenses']):
    ax.text(0 - 0.1, v - 70,
            Off_Array[i] + ' ' + str(v),
            color='#3b86c6', fontweight='bold', fontsize = 16, horizontalalignment = 'right') 

# Annotate Other 3 as Grey
for i, v in enumerate(grey2015['Number of Offenses']):
    ax.text(0 - 0.1, v - 70,
            Grey_Array[i] + ' ' + str(v),
            color='#c3c3c3', fontweight='bold', fontsize = 16, horizontalalignment = 'right')
    
# Annotate and color M/V as Blue
for i, v in enumerate(color2018['Number of Offenses']):
    ax.text(3 + 0.10, v - 70,
            str(v),
            color='#3b86c6', fontweight='bold', fontsize = 16, horizontalalignment = 'left') 

# Annotate Other 3 as Grey
for i, v in enumerate(grey2018['Number of Offenses']):
    ax.text(3 + 0.10, v - 70,
            str(v),
            color='#c3c3c3', fontweight='bold', fontsize = 16, horizontalalignment = 'left')    

# Show Graph and Dataframe
plt.show()    
{% endhighlight %}

So there it is, the Slope Graph. It takes a lot more work to get a Slope Graph made properly due to it's aesthetic. Because of that, I'm most likely not going to be using it often for my data analysis projects. 

I hope you enjoyed the process of making a Slope Graph! See you next time!
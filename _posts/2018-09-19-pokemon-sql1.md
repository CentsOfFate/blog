---
layout: post
title: Learning SQL with Pokemon (Part 1 of 2)
---



Welcome to the world of Pokemon! Most people who come through here are looking to start their Pokemon journey by catching, training and discovering Pokemon. It is important for people to go out into the unknown and discover secrets about Pokemon that we have yet to unravel.

<img style="float: right;" src="http://dedalvs.com/kamakawi/words/pikachu.png" width="150" height="150" />

However, there's another option you may take, one that is equally as important, if not more important. You could become a Pokemon Researcher and help construct our Pokemon Database! Trainers out in the field need to have a wealth of accurate information in order for them to make informed decisions on their journey. You will still get the chance interact with Pokemon on a daily basis, but your main role is to work with our new SQL technology to create data tables that will best inform our trainers. Your work here in the Research Lab will affect the lives of thousands, maybe even millions of people!

Does that sound like a journey worth embarking on? When you are ready, press START!

## Before we begin...

Before we begin, we need to talk about some terminology. All of our Pokemon data is stored in what we call a Relational  Database Management System, or RDBMS for short. We use SQL in reading, writing and handling the data. SQL stands for Structured Query Language. Think of a RDBMS as a large room filled with file cabinets and SQL as a tool to help you perform tasks against the file cabinets. As you will learn, SQL is an extremely powerful tool.

NOTE 1: I am using this CSV from Kaggle for this Project: https://www.kaggle.com/abcsds/pokemon
I am also using SQL Server Express and SQL Server Management Studio as my database platform. Feel free to use whichever flavor of SQL you prefer.

NOTE 2: I made a couple of edits in the CSV file. I removed the first column (the numbers) and I changed "Sp. Atk" and "Sp. Def" to "Special Attack" and "Special Defense" respectively. Everything else I kept the same.

NOTE 3: Rather than snipping SQL Server images onto this Jupyter Notebook, I will use pyodbc to connect SQL Server and use pandas to show my results. This will keep things simpler in presenting the data output. Other than that, I will use only SQL to manipulate the data.

## Setting things up...


```python
import numpy as np
import pandas as pd
import pyodbc

conn = pyodbc.connect("Driver={SQL Server Native Client 11.0};"
                      "Server=localhost;"
                      "Database=Pokemon;"
                      "Trusted_Connection=yes;")
cursor = conn.cursor()
```

<img style="float: right;" src="http://files.shandymedia.com/styles/page_full/s3/images/photos/hollyscoop/9-pidgey.png" width="200" height="200" />
## Your first SQL Queries!

Let's begin with the simplest query we can write.


```python
# This Query SELECTs all of our data
df = pd.read_sql_query('''
SELECT *
FROM pokemon
''', conn)

# I then use the head function from Pandas to grab only the first 5 rows to keep things neat
df.head(5)
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Name</th>
      <th>Type 1</th>
      <th>Type 2</th>
      <th>Total</th>
      <th>HP</th>
      <th>Attack</th>
      <th>Defense</th>
      <th>Special Attack</th>
      <th>Special Defense</th>
      <th>Speed</th>
      <th>Generation</th>
      <th>Legendary</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Bulbasaur</td>
      <td>Grass</td>
      <td>Poison</td>
      <td>318.0</td>
      <td>45.0</td>
      <td>49.0</td>
      <td>49.0</td>
      <td>65.0</td>
      <td>65.0</td>
      <td>45.0</td>
      <td>1.0</td>
      <td>False</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Ivysaur</td>
      <td>Grass</td>
      <td>Poison</td>
      <td>405.0</td>
      <td>60.0</td>
      <td>62.0</td>
      <td>63.0</td>
      <td>80.0</td>
      <td>80.0</td>
      <td>60.0</td>
      <td>1.0</td>
      <td>False</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Venusaur</td>
      <td>Grass</td>
      <td>Poison</td>
      <td>525.0</td>
      <td>80.0</td>
      <td>82.0</td>
      <td>83.0</td>
      <td>100.0</td>
      <td>100.0</td>
      <td>80.0</td>
      <td>1.0</td>
      <td>False</td>
    </tr>
    <tr>
      <th>3</th>
      <td>VenusaurMega Venusaur</td>
      <td>Grass</td>
      <td>Poison</td>
      <td>625.0</td>
      <td>80.0</td>
      <td>100.0</td>
      <td>123.0</td>
      <td>122.0</td>
      <td>120.0</td>
      <td>80.0</td>
      <td>1.0</td>
      <td>False</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Charmander</td>
      <td>Fire</td>
      <td>None</td>
      <td>309.0</td>
      <td>39.0</td>
      <td>52.0</td>
      <td>43.0</td>
      <td>60.0</td>
      <td>50.0</td>
      <td>65.0</td>
      <td>1.0</td>
      <td>False</td>
    </tr>
  </tbody>
</table>
</div>



So let's go through the structure of our first query. The SELECT statement is how we grab the columns we need. In SQL, the * (star) is a special character which means "All". The FROM statement tells SQL which table we want to get data from.

If we want to say "SELECT * FROM pokemon" in plain english, we would say "I want to select all the columns from the table pokemon".

Did you understand all of that? We could go into detail in how SQL pulls the data and in which order, but that would be putting the cart before the Rapidash, now would it?

So let's say we want to SELECT only Pokemon Names, both of their types and what generation the Pokemon come from. That query would look like:


```python
# This Query SELECTs Pokemon Name, both of their Types and what generation the Pokemon are from
df = pd.read_sql_query('''
SELECT
    [Name],
    [Type 1],
    [Type 2],
    [Generation]
FROM pokemon
''', conn)

df.head(5)
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Name</th>
      <th>Type 1</th>
      <th>Type 2</th>
      <th>Generation</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Bulbasaur</td>
      <td>Grass</td>
      <td>Poison</td>
      <td>1.0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Ivysaur</td>
      <td>Grass</td>
      <td>Poison</td>
      <td>1.0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Venusaur</td>
      <td>Grass</td>
      <td>Poison</td>
      <td>1.0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>VenusaurMega Venusaur</td>
      <td>Grass</td>
      <td>Poison</td>
      <td>1.0</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Charmander</td>
      <td>Fire</td>
      <td>None</td>
      <td>1.0</td>
    </tr>
  </tbody>
</table>
</div>



Awesome! A couple of things before we go on. For maximum readability since we are learning, I kept the column names in proper case and I have spaces in the columns. In the Real World, most people name their columns in all lower case and without spaces. Because we have spaces, we have to use brackets [] in order for SQL to understand we want a specific column. Also, we need to seperate columns using commas.

Next, we are going to introduce one of the most important statements in SQL: the WHERE statement. The WHERE statement allows you to filter results based on the type of values you want SQL to search for.

For our next example, let's say we want to search for Type 1 Water types. Using the above SQL query as a blueprint:


```python
# Apply WHERE statement to find Type 1 Water types
df = pd.read_sql_query('''
SELECT
    [Name],
    [Type 1],
    [Type 2],
    [Generation]
FROM pokemon
WHERE [Type 1] = 'Water'
''', conn)

df.head(5)
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Name</th>
      <th>Type 1</th>
      <th>Type 2</th>
      <th>Generation</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Squirtle</td>
      <td>Water</td>
      <td>None</td>
      <td>1.0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Wartortle</td>
      <td>Water</td>
      <td>None</td>
      <td>1.0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Blastoise</td>
      <td>Water</td>
      <td>None</td>
      <td>1.0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>BlastoiseMega Blastoise</td>
      <td>Water</td>
      <td>None</td>
      <td>1.0</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Psyduck</td>
      <td>Water</td>
      <td>None</td>
      <td>1.0</td>
    </tr>
  </tbody>
</table>
</div>



As we can see, Pokemon such as Bulbasaur and Charmander got filtered out, and we see Pokemon like Squirtle and Psyduck remain. How a simple WHERE statement works is we type what column we want to apply a condition to. We then select the type operator we want to use, which in this case, we use an equals sign. And lastly, we define our condition, which is 'Water'.

We can use as many WHERE statements as we like, by using the AND operator. So an example of using multiple WHERE statements is asking the Pokemon database to find Pokemon that are both Water AND Ice Types:


```python
# Now let's find Water AND Ice types
df = pd.read_sql_query('''
SELECT
    [Name],
    [Type 1],
    [Type 2],
    [Generation]
FROM pokemon
WHERE [Type 1] = 'Water'
AND [Type 2] = 'Ice'
''', conn)

df.head(5)
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Name</th>
      <th>Type 1</th>
      <th>Type 2</th>
      <th>Generation</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Dewgong</td>
      <td>Water</td>
      <td>Ice</td>
      <td>1.0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Cloyster</td>
      <td>Water</td>
      <td>Ice</td>
      <td>1.0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Lapras</td>
      <td>Water</td>
      <td>Ice</td>
      <td>1.0</td>
    </tr>
  </tbody>
</table>
</div>



We can take this one step further by asking the database that we want Water and Ice Types that have a Defense base stat of over 100:


```python
# Let's add one more WHERE Statement, Defense is over 100
df = pd.read_sql_query('''
SELECT
    [Name],
    [Type 1],
    [Type 2],
    [Generation]
FROM pokemon
WHERE [Type 1] = 'Water'
AND [Type 2] = 'Ice'
AND [Defense] > 100
''', conn)

df.head(5)
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Name</th>
      <th>Type 1</th>
      <th>Type 2</th>
      <th>Generation</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Cloyster</td>
      <td>Water</td>
      <td>Ice</td>
      <td>1.0</td>
    </tr>
  </tbody>
</table>
</div>



With just 3 WHERE statements, we narrowed our search down to just Cloyster! You may have noticed that our WHERE statement applied a condition on a column that we are not SELECTing. We can apply conditions and logic on columns that we are not returning back to us.

<img style="float: right;" src="https://upload.wikimedia.org/wikipedia/en/a/a5/Pok%C3%A9mon_Charmander_art.png" width="150" height="150" />
## Situation 1 - No more Charmanders?!

Professor Oak back in Pallet Town had an unexpected problem! In the last couple of weeks, there has been an influx of young trainers coming to his lab and picking up Charmanders as their first Pokemon for their journey. This has led to a shortage of Charmanders! Professor Oak doesn't want to force trainers to picking Bulbasaur or Squirtle, but he doesn't have to time to get more Charmanders for the next week's trainers.

Instead, Professor Oak is going to have to compromise by selecting a Fire Type that would be suitable for a young trainer. What would be a good choice in the Kanto region for Oak to pick?


```python
df = pd.read_sql_query('''
SELECT
    [Name],
    [Type 1],
    [Type 2],
    [HP],
    [Attack],
    [Defense],
    [Special Attack],
    [Special Defense],
    [Speed]
FROM pokemon
WHERE [Type 1] = 'Fire'
AND [Generation] = 1
AND [Name] NOT LIKE '%Mega%'
AND [Name] NOT IN ('Charmander', 'Charmeleon', 'Charizard')
AND Attack < 70
''', conn)

df.head(10)
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Name</th>
      <th>Type 1</th>
      <th>Type 2</th>
      <th>HP</th>
      <th>Attack</th>
      <th>Defense</th>
      <th>Special Attack</th>
      <th>Special Defense</th>
      <th>Speed</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Vulpix</td>
      <td>Fire</td>
      <td>None</td>
      <td>38.0</td>
      <td>41.0</td>
      <td>40.0</td>
      <td>50.0</td>
      <td>65.0</td>
      <td>65.0</td>
    </tr>
  </tbody>
</table>
</div>



After typing in several lines of SQL Code into the Pokemon database, he seems to have settled on Vulpix as an alternative to Charmander!

Lots of things going on here. Let's go through all of them. We decided to SELECT Name, both types and all of the base stats. Next, we want a WHERE statement for Fire Types and Generation 1 Pokemon.

The hairy stuff comes next. We can create WHERE statements using NOT, which doesn't return certain conditions. The operator LIKE is then used in tandem with the wildcard % inside the single quotations marks. What % does is that it looks for characters similar to the word it is searching for.

So for example if we said 'Slow%', we would search for any Pokemon we the word 'Slow' and anything that comes after it. Likewise, if we searched '%king', we would search for any Pokemon with the word 'king' in it's name, and anything before it.

So what (AND [Name] NOT LIKE '%Mega%') is saying is that we DON'T want any Pokemon with Mega in their name.

(AND [Name] NOT IN ('Charmander', 'Charmeleon', 'Charizard')) is saying we DON'T want to return Pokemon Charmander, Charmeleon or Charizard.

And lastly, we don't Professor Oak to be giving Pokemon too powerful, so he wanted a Pokemon with an Attack base stat of less than 70

## Finishing up the Basics!

So far we have covered 3 basic SQL Statements: SELECT, FROM and WHERE. Now it's time for us to tackle the last 3 basic SQL Statements: GROUP BY, HAVING and ORDER BY.

Let's start by GROUP BY. GROUP BY allows us to group values together by an aggregate statement. Some aggregate statements include COUNT, MAX, MIN, SUM, and AVG. This will allows us to create a GROUP set. I think it will be easier to explain with an example.

So let's say we want to COUNT the number of Type 1 Water types in each Generation:


```python
df = pd.read_sql_query('''
SELECT
    [Generation],
    COUNT([Type 1]) AS 'Number of Water Types'
FROM pokemon
WHERE [Type 1] = 'Water'
GROUP BY [Generation]
''', conn)

df.head(10)
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Generation</th>
      <th>Number of Water Types</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1.0</td>
      <td>31</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2.0</td>
      <td>18</td>
    </tr>
    <tr>
      <th>2</th>
      <td>3.0</td>
      <td>27</td>
    </tr>
    <tr>
      <th>3</th>
      <td>4.0</td>
      <td>13</td>
    </tr>
    <tr>
      <th>4</th>
      <td>5.0</td>
      <td>18</td>
    </tr>
    <tr>
      <th>5</th>
      <td>6.0</td>
      <td>5</td>
    </tr>
  </tbody>
</table>
</div>



Several things going on here. We want a WHERE statement to grab only our Water types. Our new SQL Statement GROUP BY is then use on the Generation column. Since we want to COUNT the number of Water Types, in the SELECT statement, we add a COUNT() and put [Type 1] inside the parenthesis.

What we have after the COUNT([Type 1]) is an alias for the column. The AS statement after our column allows us to rename our column to whatever we want to name it. What I have in single quotes is Number of Water Types.

Based on this GROUP BY, we see that Generation 1 has 31 Water Types, Generation 2 has 18 Water Types and so on.

Let's do another one. Let's find the AVG Special Attack among all Type 1s:


```python
df = pd.read_sql_query('''
SELECT
    [Type 1],
    AVG([Special Attack]) AS 'Average Special Attack'
FROM pokemon
GROUP BY [Type 1]
''', conn)

df.head(20)
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Type 1</th>
      <th>Average Special Attack</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Bug</td>
      <td>53.869565</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Dark</td>
      <td>74.645161</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Dragon</td>
      <td>96.843750</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Electric</td>
      <td>90.022727</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Fairy</td>
      <td>78.529412</td>
    </tr>
    <tr>
      <th>5</th>
      <td>Fighting</td>
      <td>53.111111</td>
    </tr>
    <tr>
      <th>6</th>
      <td>Fire</td>
      <td>88.980769</td>
    </tr>
    <tr>
      <th>7</th>
      <td>Flying</td>
      <td>94.250000</td>
    </tr>
    <tr>
      <th>8</th>
      <td>Ghost</td>
      <td>79.343750</td>
    </tr>
    <tr>
      <th>9</th>
      <td>Grass</td>
      <td>77.500000</td>
    </tr>
    <tr>
      <th>10</th>
      <td>Ground</td>
      <td>56.468750</td>
    </tr>
    <tr>
      <th>11</th>
      <td>Ice</td>
      <td>77.541667</td>
    </tr>
    <tr>
      <th>12</th>
      <td>Normal</td>
      <td>55.816327</td>
    </tr>
    <tr>
      <th>13</th>
      <td>Poison</td>
      <td>60.428571</td>
    </tr>
    <tr>
      <th>14</th>
      <td>Psychic</td>
      <td>98.403509</td>
    </tr>
    <tr>
      <th>15</th>
      <td>Rock</td>
      <td>63.340909</td>
    </tr>
    <tr>
      <th>16</th>
      <td>Steel</td>
      <td>67.518519</td>
    </tr>
    <tr>
      <th>17</th>
      <td>Water</td>
      <td>74.812500</td>
    </tr>
  </tbody>
</table>
</div>



Let's build on top of that Query. We can further filter down this query using a HAVING statement. Think of a HAVING statement as filtering data AFTER a GROUP BY has been applied.

So let's say we want to grab only the Pokemon Types that have an Average Special Attack of above 70:


```python
df = pd.read_sql_query('''
SELECT
    [Type 1],
    AVG([Special Attack]) AS 'Average Special Attack'
FROM pokemon
GROUP BY [Type 1]
HAVING AVG([Special Attack]) > 70
''', conn)

df.head(20)
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Type 1</th>
      <th>Average Special Attack</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Dark</td>
      <td>74.645161</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Dragon</td>
      <td>96.843750</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Electric</td>
      <td>90.022727</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Fairy</td>
      <td>78.529412</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Fire</td>
      <td>88.980769</td>
    </tr>
    <tr>
      <th>5</th>
      <td>Flying</td>
      <td>94.250000</td>
    </tr>
    <tr>
      <th>6</th>
      <td>Ghost</td>
      <td>79.343750</td>
    </tr>
    <tr>
      <th>7</th>
      <td>Grass</td>
      <td>77.500000</td>
    </tr>
    <tr>
      <th>8</th>
      <td>Ice</td>
      <td>77.541667</td>
    </tr>
    <tr>
      <th>9</th>
      <td>Psychic</td>
      <td>98.403509</td>
    </tr>
    <tr>
      <th>10</th>
      <td>Water</td>
      <td>74.812500</td>
    </tr>
  </tbody>
</table>
</div>



Great! Now, let's sort the Average Special Attack in order from greatest to least:


```python
df = pd.read_sql_query('''
SELECT
    [Type 1],
    AVG([Special Attack]) AS 'Average Special Attack'
FROM pokemon
GROUP BY [Type 1]
HAVING AVG([Special Attack]) > 70
ORDER BY 'Average Special Attack' DESC
''', conn)

df.head(20)
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Type 1</th>
      <th>Average Special Attack</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Psychic</td>
      <td>98.403509</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Dragon</td>
      <td>96.843750</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Flying</td>
      <td>94.250000</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Electric</td>
      <td>90.022727</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Fire</td>
      <td>88.980769</td>
    </tr>
    <tr>
      <th>5</th>
      <td>Ghost</td>
      <td>79.343750</td>
    </tr>
    <tr>
      <th>6</th>
      <td>Fairy</td>
      <td>78.529412</td>
    </tr>
    <tr>
      <th>7</th>
      <td>Ice</td>
      <td>77.541667</td>
    </tr>
    <tr>
      <th>8</th>
      <td>Grass</td>
      <td>77.500000</td>
    </tr>
    <tr>
      <th>9</th>
      <td>Water</td>
      <td>74.812500</td>
    </tr>
    <tr>
      <th>10</th>
      <td>Dark</td>
      <td>74.645161</td>
    </tr>
  </tbody>
</table>
</div>



Perfect! How an ORDER BY works is that we can sort a column (in this case, we can sort an alias, more on that later), in order. The default is to sort in order by A to Z for letters and least to greatest for numerical values. If you use a DESC after the column you want to sort, you will get greatest to least.

<img style="float: right;" src="https://cdn.bulbagarden.net/upload/f/fa/164Noctowl.png" width="200" height="200" />
## Situation 2 - Pewter City under attack!

Team Rocket is attacking Pewter City! Brock is out of town and you happen to be the only veteran trainer ready to stop them from stealing the fossils from the Pewter Museum! The Pokemon they are using are Koffings, Ekans, and Zubats. Does your team have the strength to defeat Team Rocket!?

Your Pokemon Party at the moment is Quilava, Vulpix and Growlithe. You cannot use the Pokemon center to retreive your other Pokemon. What you have is what you have to fight with.

We need to see if your Pokemon's AVG strength is strong enough to adequately defeat Team Rocket!


```python
# Your Party AVG Strength
df = pd.read_sql_query('''
SELECT
    [Type 1],
    AVG([Total]) AS 'Average Total Base Stats'
FROM pokemon
WHERE [Type 1] = 'Fire'
AND NAME IN ('Quilava', 'Vulpix', 'Growlithe')
GROUP BY [Type 1]
''', conn)

df.head(20)
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Type 1</th>
      <th>Average Total Base Stats</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Fire</td>
      <td>351.333333</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Team Rocket's AVG Strength
df = pd.read_sql_query('''
SELECT
    [Type 1],
    AVG([Total]) AS 'Average Total Base Stats'
FROM pokemon
WHERE [Type 1] = 'Poison'
AND NAME IN ('Koffing', 'Ekans', 'Zubat')
GROUP BY [Type 1]
''', conn)

df.head(20)
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Type 1</th>
      <th>Average Total Base Stats</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Poison</td>
      <td>291.0</td>
    </tr>
  </tbody>
</table>
</div>



It seems like your party is a quite a bit stronger than Team Rocket. We should be able to take them down easy!

<img style="float: right;" src="https://cdn.bulbagarden.net/upload/thumb/9/92/FireRed_LeafGreen_Blaine.png/436px-FireRed_LeafGreen_Blaine.png" width="200" height="200" />
# Situation 3 - So many choices!?


You are about to challenge Blaine to a Gym Battle! After being defeated by Red and Gary Oak, Blaine has been training extra hard to make sure he doesn't lose so easily to younger trainers. Some of the Pokemon on his roster include Ninetails, Camerupt, Ampharos, Jumpluff and Arcanine.

The mix of Electric and Grass in his roster will make it difficult to just fight him with Water or Grounds types. What will you do?

With such an evolved team, you need to have a powerful roster to begin with. Next to pick some Pokemon that can contend with all his types. Let's see what we can do...


```python
# Team Rocket's AVG Strength
df = pd.read_sql_query('''
SELECT
    [Name],
    [Type 1],
    [Type 2],
    [Total],
    [HP],
    [Attack],
    [Defense],
    [Special Attack],
    [Special Defense],
    [Speed]
  FROM [Pokemon].[dbo].[pokemon]
  WHERE [Special Attack] > 100
  AND Legendary = 0
  AND [Name] NOT LIKE '%Mega%'
  AND [Type 1] IN ('Water', 'Ground')
  AND [Speed] > 60
  OR [Attack] > 100
  AND Legendary = 0
  AND [Name] NOT LIKE '%Mega%'
  AND [Type 1] IN ('Water', 'Ground')
  AND [Speed] > 60
  ORDER BY [Special Attack] DESC
''', conn)

df.head(20)
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Name</th>
      <th>Type 1</th>
      <th>Type 2</th>
      <th>Total</th>
      <th>HP</th>
      <th>Attack</th>
      <th>Defense</th>
      <th>Special Attack</th>
      <th>Special Defense</th>
      <th>Speed</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>KeldeoOrdinary Forme</td>
      <td>Water</td>
      <td>Fighting</td>
      <td>580.0</td>
      <td>91.0</td>
      <td>72.0</td>
      <td>90.0</td>
      <td>129.0</td>
      <td>90.0</td>
      <td>108.0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>KeldeoResolute Forme</td>
      <td>Water</td>
      <td>Fighting</td>
      <td>580.0</td>
      <td>91.0</td>
      <td>72.0</td>
      <td>90.0</td>
      <td>129.0</td>
      <td>90.0</td>
      <td>108.0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Vaporeon</td>
      <td>Water</td>
      <td>None</td>
      <td>525.0</td>
      <td>130.0</td>
      <td>65.0</td>
      <td>60.0</td>
      <td>110.0</td>
      <td>95.0</td>
      <td>65.0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Samurott</td>
      <td>Water</td>
      <td>None</td>
      <td>528.0</td>
      <td>95.0</td>
      <td>100.0</td>
      <td>85.0</td>
      <td>108.0</td>
      <td>70.0</td>
      <td>70.0</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Greninja</td>
      <td>Water</td>
      <td>Dark</td>
      <td>530.0</td>
      <td>72.0</td>
      <td>95.0</td>
      <td>67.0</td>
      <td>103.0</td>
      <td>71.0</td>
      <td>122.0</td>
    </tr>
    <tr>
      <th>5</th>
      <td>Sharpedo</td>
      <td>Water</td>
      <td>Dark</td>
      <td>460.0</td>
      <td>70.0</td>
      <td>120.0</td>
      <td>40.0</td>
      <td>95.0</td>
      <td>40.0</td>
      <td>95.0</td>
    </tr>
    <tr>
      <th>6</th>
      <td>Floatzel</td>
      <td>Water</td>
      <td>None</td>
      <td>495.0</td>
      <td>85.0</td>
      <td>105.0</td>
      <td>55.0</td>
      <td>85.0</td>
      <td>50.0</td>
      <td>115.0</td>
    </tr>
    <tr>
      <th>7</th>
      <td>Feraligatr</td>
      <td>Water</td>
      <td>None</td>
      <td>530.0</td>
      <td>85.0</td>
      <td>105.0</td>
      <td>100.0</td>
      <td>79.0</td>
      <td>83.0</td>
      <td>78.0</td>
    </tr>
    <tr>
      <th>8</th>
      <td>Krookodile</td>
      <td>Ground</td>
      <td>Dark</td>
      <td>519.0</td>
      <td>95.0</td>
      <td>117.0</td>
      <td>80.0</td>
      <td>65.0</td>
      <td>70.0</td>
      <td>92.0</td>
    </tr>
    <tr>
      <th>9</th>
      <td>Gyarados</td>
      <td>Water</td>
      <td>Flying</td>
      <td>540.0</td>
      <td>95.0</td>
      <td>125.0</td>
      <td>79.0</td>
      <td>60.0</td>
      <td>100.0</td>
      <td>81.0</td>
    </tr>
    <tr>
      <th>10</th>
      <td>Kingler</td>
      <td>Water</td>
      <td>None</td>
      <td>475.0</td>
      <td>55.0</td>
      <td>130.0</td>
      <td>115.0</td>
      <td>50.0</td>
      <td>50.0</td>
      <td>75.0</td>
    </tr>
    <tr>
      <th>11</th>
      <td>Excadrill</td>
      <td>Ground</td>
      <td>Steel</td>
      <td>508.0</td>
      <td>110.0</td>
      <td>135.0</td>
      <td>60.0</td>
      <td>50.0</td>
      <td>65.0</td>
      <td>88.0</td>
    </tr>
  </tbody>
</table>
</div>



Lots of stuff going on here. The two big WHERE statements are we want Pokemon that have Special Attack OR Attack base stats of above 100. Then we add in that we don't want Megas, Legendaries, the Type 1 has to be Water or Ground and their speed has to above 60. Overall, we want relatively quick Pokemon that are really powerful.

Looking at the list, we have some good choices here.

Excadrill at the bottom has 135 Attack, which is the strongest Pokemon here.
We also have Vaporeon, who has high HP and decent Special Attack.
Gyarados is pretty strong too, but needs to avoid Blaine's Ampharos.
Greninja is the fastest Pokemon in the list, and still has 103 Special Attack.

Looking at what we have, I'd say Excadrill, Vaporeon, Gyarados and Greninja would be fantastic choices against Blaine's party. Let's challenge him!

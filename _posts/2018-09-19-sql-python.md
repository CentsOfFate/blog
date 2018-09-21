---
layout: post
title: SQL - Python Conversions
---

Python (with the library `pandas`) can perform many of the same SQL clauses. Let's do a quick run through to see what we can do in Python that can also be done in SQL and vice versa.

We will be using the Pokemon dataset with all the Typings, Base Stats and Generations.


```python
# Import Numpy and Pandas
import numpy as np
import pandas as pd

# Load Data
df = pd.read_csv('Pokemon.csv')
```

## SELECT (TOP 5) *

This first one is going to be a simple `SELECT TOP(5) *` (because I don't want to take up a lot of the page with the Python Execute).

### SQL Statement

`SELECT TOP (5) *
 FROM [Datasets].[dbo].[Pokemon]`

### Python Equivalent


```python
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
      <th>Sp. Atk</th>
      <th>Sp. Def</th>
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
      <td>318</td>
      <td>45</td>
      <td>49</td>
      <td>49</td>
      <td>65</td>
      <td>65</td>
      <td>45</td>
      <td>1</td>
      <td>False</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Ivysaur</td>
      <td>Grass</td>
      <td>Poison</td>
      <td>405</td>
      <td>60</td>
      <td>62</td>
      <td>63</td>
      <td>80</td>
      <td>80</td>
      <td>60</td>
      <td>1</td>
      <td>False</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Venusaur</td>
      <td>Grass</td>
      <td>Poison</td>
      <td>525</td>
      <td>80</td>
      <td>82</td>
      <td>83</td>
      <td>100</td>
      <td>100</td>
      <td>80</td>
      <td>1</td>
      <td>False</td>
    </tr>
    <tr>
      <th>3</th>
      <td>VenusaurMega Venusaur</td>
      <td>Grass</td>
      <td>Poison</td>
      <td>625</td>
      <td>80</td>
      <td>100</td>
      <td>123</td>
      <td>122</td>
      <td>120</td>
      <td>80</td>
      <td>1</td>
      <td>False</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Charmander</td>
      <td>Fire</td>
      <td>NaN</td>
      <td>309</td>
      <td>39</td>
      <td>52</td>
      <td>43</td>
      <td>60</td>
      <td>50</td>
      <td>65</td>
      <td>1</td>
      <td>False</td>
    </tr>
  </tbody>
</table>
</div>



## SELECT Statement

In order to do the Python equivalent of a `SELECT` statement, we need to call our dataframe and encampsulate which columns we want in double brackets. For example:

`df[['Name', 'Sp. Atk', 'Sp. Def']]`

### SQL Query

`SELECT TOP (5)
	Name,
	Attack,
	Defense
FROM [Datasets].[dbo].[Pokemon]`

### Python Equivalent


```python
df[['Name', 'Attack', 'Defense']].head(5)
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
      <th>Attack</th>
      <th>Defense</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Bulbasaur</td>
      <td>49</td>
      <td>49</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Ivysaur</td>
      <td>62</td>
      <td>63</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Venusaur</td>
      <td>82</td>
      <td>83</td>
    </tr>
    <tr>
      <th>3</th>
      <td>VenusaurMega Venusaur</td>
      <td>100</td>
      <td>123</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Charmander</td>
      <td>52</td>
      <td>43</td>
    </tr>
  </tbody>
</table>
</div>



## WHERE Statement

In order to do a WHERE statement in Python, we need to use the method `loc` to filter results in our dataframe. We call the method on our dataframe:

`df.loc`

Then we need to call our dataframe inside the method with the column we want to perform a filter operation on, then we perform the mathmatical operation:

`df.loc[df['Speed'] > 75]`

### SQL Query

`SELECT *
FROM [Datasets].[dbo].[Pokemon]
WHERE Name = 'Charmander'`

### Python Equivalent


```python
df.loc[df['Name'] == 'Charmander']
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
      <th>Sp. Atk</th>
      <th>Sp. Def</th>
      <th>Speed</th>
      <th>Generation</th>
      <th>Legendary</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>4</th>
      <td>Charmander</td>
      <td>Fire</td>
      <td>NaN</td>
      <td>309</td>
      <td>39</td>
      <td>52</td>
      <td>43</td>
      <td>60</td>
      <td>50</td>
      <td>65</td>
      <td>1</td>
      <td>False</td>
    </tr>
  </tbody>
</table>
</div>



## WHERE ...
## AND ...

This gets a bit complicated when it comes to typing out the Python code for this. In order to do an AND for our `loc` method, we use the `&` in between the two calls

We do the same as before, we call our dataframe inside the `loc` method. But, we need to add parentheses to seperate each `loc` method statement. So it will look like this:

`df.loc[(df['Speed'] > 75]) & (df['Generation'] == 3])`

### SQL Query

`SELECT *
FROM [Datasets].[dbo].[Pokemon]
WHERE Attack > 80
AND Defense < 45`

### Python Equivalent


```python
df.loc[(df['Attack'] > 80) & (df['Defense'] < 45)].head(5)
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
      <th>Sp. Atk</th>
      <th>Sp. Def</th>
      <th>Speed</th>
      <th>Generation</th>
      <th>Legendary</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>18</th>
      <td>Beedrill</td>
      <td>Bug</td>
      <td>Poison</td>
      <td>395</td>
      <td>65</td>
      <td>90</td>
      <td>40</td>
      <td>45</td>
      <td>80</td>
      <td>75</td>
      <td>1</td>
      <td>False</td>
    </tr>
    <tr>
      <th>19</th>
      <td>BeedrillMega Beedrill</td>
      <td>Bug</td>
      <td>Poison</td>
      <td>495</td>
      <td>65</td>
      <td>150</td>
      <td>40</td>
      <td>15</td>
      <td>80</td>
      <td>145</td>
      <td>1</td>
      <td>False</td>
    </tr>
    <tr>
      <th>213</th>
      <td>Murkrow</td>
      <td>Dark</td>
      <td>Flying</td>
      <td>405</td>
      <td>60</td>
      <td>85</td>
      <td>42</td>
      <td>85</td>
      <td>42</td>
      <td>91</td>
      <td>2</td>
      <td>False</td>
    </tr>
    <tr>
      <th>347</th>
      <td>Carvanha</td>
      <td>Water</td>
      <td>Dark</td>
      <td>305</td>
      <td>45</td>
      <td>90</td>
      <td>20</td>
      <td>65</td>
      <td>20</td>
      <td>65</td>
      <td>3</td>
      <td>False</td>
    </tr>
    <tr>
      <th>348</th>
      <td>Sharpedo</td>
      <td>Water</td>
      <td>Dark</td>
      <td>460</td>
      <td>70</td>
      <td>120</td>
      <td>40</td>
      <td>95</td>
      <td>40</td>
      <td>95</td>
      <td>3</td>
      <td>False</td>
    </tr>
  </tbody>
</table>
</div>



## WHERE ...
## OR ...

This is the same as the AND statement, but we use a `|` to denote the OR.

### SQL Query

`SELECT *
FROM [Datasets].[dbo].[Pokemon]
WHERE Name = 'Charmander'
OR Name = 'Squirtle'`

### Python Equivalent


```python
df.loc[(df['Name'] == 'Charmander') | (df['Name'] == 'Squirtle')]
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
      <th>Sp. Atk</th>
      <th>Sp. Def</th>
      <th>Speed</th>
      <th>Generation</th>
      <th>Legendary</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>4</th>
      <td>Charmander</td>
      <td>Fire</td>
      <td>NaN</td>
      <td>309</td>
      <td>39</td>
      <td>52</td>
      <td>43</td>
      <td>60</td>
      <td>50</td>
      <td>65</td>
      <td>1</td>
      <td>False</td>
    </tr>
    <tr>
      <th>9</th>
      <td>Squirtle</td>
      <td>Water</td>
      <td>NaN</td>
      <td>314</td>
      <td>44</td>
      <td>48</td>
      <td>65</td>
      <td>50</td>
      <td>64</td>
      <td>43</td>
      <td>1</td>
      <td>False</td>
    </tr>
  </tbody>
</table>
</div>



## GROUP BY Statement

The Python equivalent to SQL's `GROUP BY` is a `.groupby()`. The same logic applies in SQL, we need to denote a column we want to group on and perform a mathmatical operation for the whole column.

### SQL Query

`SELECT
	[Type 1]
	,AVG(HP) AS 'Average HP'
	,AVG(Attack) AS 'Average Attack'
	,AVG(Defense) AS 'Average Defense'
FROM [Datasets].[dbo].[Pokemon]
GROUP BY [Type 1]`


### Python Equivalent


```python
df[['Type 1', 'HP', 'Attack', 'Defense']].groupby(['Type 1']).mean()
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
      <th>HP</th>
      <th>Attack</th>
      <th>Defense</th>
    </tr>
    <tr>
      <th>Type 1</th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Bug</th>
      <td>56.884058</td>
      <td>70.971014</td>
      <td>70.724638</td>
    </tr>
    <tr>
      <th>Dark</th>
      <td>66.806452</td>
      <td>88.387097</td>
      <td>70.225806</td>
    </tr>
    <tr>
      <th>Dragon</th>
      <td>83.312500</td>
      <td>112.125000</td>
      <td>86.375000</td>
    </tr>
    <tr>
      <th>Electric</th>
      <td>59.795455</td>
      <td>69.090909</td>
      <td>66.295455</td>
    </tr>
    <tr>
      <th>Fairy</th>
      <td>74.117647</td>
      <td>61.529412</td>
      <td>65.705882</td>
    </tr>
    <tr>
      <th>Fighting</th>
      <td>69.851852</td>
      <td>96.777778</td>
      <td>65.925926</td>
    </tr>
    <tr>
      <th>Fire</th>
      <td>69.903846</td>
      <td>84.769231</td>
      <td>67.769231</td>
    </tr>
    <tr>
      <th>Flying</th>
      <td>70.750000</td>
      <td>78.750000</td>
      <td>66.250000</td>
    </tr>
    <tr>
      <th>Ghost</th>
      <td>64.437500</td>
      <td>73.781250</td>
      <td>81.187500</td>
    </tr>
    <tr>
      <th>Grass</th>
      <td>67.271429</td>
      <td>73.214286</td>
      <td>70.800000</td>
    </tr>
    <tr>
      <th>Ground</th>
      <td>73.781250</td>
      <td>95.750000</td>
      <td>84.843750</td>
    </tr>
    <tr>
      <th>Ice</th>
      <td>72.000000</td>
      <td>72.750000</td>
      <td>71.416667</td>
    </tr>
    <tr>
      <th>Normal</th>
      <td>77.275510</td>
      <td>73.469388</td>
      <td>59.846939</td>
    </tr>
    <tr>
      <th>Poison</th>
      <td>67.250000</td>
      <td>74.678571</td>
      <td>68.821429</td>
    </tr>
    <tr>
      <th>Psychic</th>
      <td>70.631579</td>
      <td>71.456140</td>
      <td>67.684211</td>
    </tr>
    <tr>
      <th>Rock</th>
      <td>65.363636</td>
      <td>92.863636</td>
      <td>100.795455</td>
    </tr>
    <tr>
      <th>Steel</th>
      <td>65.222222</td>
      <td>92.703704</td>
      <td>126.370370</td>
    </tr>
    <tr>
      <th>Water</th>
      <td>72.062500</td>
      <td>74.151786</td>
      <td>72.946429</td>
    </tr>
  </tbody>
</table>
</div>



## HAVING Statement

As far as I know, you cannot perform a HAVING statement after a `.groupby()` method has been called. However, we can just assign the dataframe to a new dataframe variable and then just call a `.loc` method on top of it.

### SQL Query

`SELECT
	[Type 1]
	,AVG(HP) AS 'Average HP'
	,AVG(Attack) AS 'Average Attack'
	,AVG(Defense) AS 'Average Defense'
FROM [Datasets].[dbo].[Pokemon]
GROUP BY [Type 1]
HAVING AVG(HP) > 70`

### Python Equivalent


```python
gdf = df[['Type 1', 'HP', 'Attack', 'Defense']].groupby(['Type 1']).mean()
gdf.loc[gdf['HP'] > 70]
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
      <th>HP</th>
      <th>Attack</th>
      <th>Defense</th>
    </tr>
    <tr>
      <th>Type 1</th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Dragon</th>
      <td>83.312500</td>
      <td>112.125000</td>
      <td>86.375000</td>
    </tr>
    <tr>
      <th>Fairy</th>
      <td>74.117647</td>
      <td>61.529412</td>
      <td>65.705882</td>
    </tr>
    <tr>
      <th>Flying</th>
      <td>70.750000</td>
      <td>78.750000</td>
      <td>66.250000</td>
    </tr>
    <tr>
      <th>Ground</th>
      <td>73.781250</td>
      <td>95.750000</td>
      <td>84.843750</td>
    </tr>
    <tr>
      <th>Ice</th>
      <td>72.000000</td>
      <td>72.750000</td>
      <td>71.416667</td>
    </tr>
    <tr>
      <th>Normal</th>
      <td>77.275510</td>
      <td>73.469388</td>
      <td>59.846939</td>
    </tr>
    <tr>
      <th>Psychic</th>
      <td>70.631579</td>
      <td>71.456140</td>
      <td>67.684211</td>
    </tr>
    <tr>
      <th>Water</th>
      <td>72.062500</td>
      <td>74.151786</td>
      <td>72.946429</td>
    </tr>
  </tbody>
</table>
</div>



## ORDER BY Statement

Lastly, we can perform an `ORDER BY` in Python by doing a `.sort_values()`.

### SQL Query

`SELECT
	[Type 1]
	,AVG(HP) AS 'Average HP'
	,AVG(Attack) AS 'Average Attack'
	,AVG(Defense) AS 'Average Defense'
FROM [Datasets].[dbo].[Pokemon]
GROUP BY [Type 1]
ORDER BY [Average HP]`

### Python Equivalent


```python
(df[['Type 1', 'HP', 'Attack', 'Defense']]
       .groupby(['Type 1'])
       .mean()
       .sort_values(by = 'HP'))
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
      <th>HP</th>
      <th>Attack</th>
      <th>Defense</th>
    </tr>
    <tr>
      <th>Type 1</th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Bug</th>
      <td>56.884058</td>
      <td>70.971014</td>
      <td>70.724638</td>
    </tr>
    <tr>
      <th>Electric</th>
      <td>59.795455</td>
      <td>69.090909</td>
      <td>66.295455</td>
    </tr>
    <tr>
      <th>Ghost</th>
      <td>64.437500</td>
      <td>73.781250</td>
      <td>81.187500</td>
    </tr>
    <tr>
      <th>Steel</th>
      <td>65.222222</td>
      <td>92.703704</td>
      <td>126.370370</td>
    </tr>
    <tr>
      <th>Rock</th>
      <td>65.363636</td>
      <td>92.863636</td>
      <td>100.795455</td>
    </tr>
    <tr>
      <th>Dark</th>
      <td>66.806452</td>
      <td>88.387097</td>
      <td>70.225806</td>
    </tr>
    <tr>
      <th>Poison</th>
      <td>67.250000</td>
      <td>74.678571</td>
      <td>68.821429</td>
    </tr>
    <tr>
      <th>Grass</th>
      <td>67.271429</td>
      <td>73.214286</td>
      <td>70.800000</td>
    </tr>
    <tr>
      <th>Fighting</th>
      <td>69.851852</td>
      <td>96.777778</td>
      <td>65.925926</td>
    </tr>
    <tr>
      <th>Fire</th>
      <td>69.903846</td>
      <td>84.769231</td>
      <td>67.769231</td>
    </tr>
    <tr>
      <th>Psychic</th>
      <td>70.631579</td>
      <td>71.456140</td>
      <td>67.684211</td>
    </tr>
    <tr>
      <th>Flying</th>
      <td>70.750000</td>
      <td>78.750000</td>
      <td>66.250000</td>
    </tr>
    <tr>
      <th>Ice</th>
      <td>72.000000</td>
      <td>72.750000</td>
      <td>71.416667</td>
    </tr>
    <tr>
      <th>Water</th>
      <td>72.062500</td>
      <td>74.151786</td>
      <td>72.946429</td>
    </tr>
    <tr>
      <th>Ground</th>
      <td>73.781250</td>
      <td>95.750000</td>
      <td>84.843750</td>
    </tr>
    <tr>
      <th>Fairy</th>
      <td>74.117647</td>
      <td>61.529412</td>
      <td>65.705882</td>
    </tr>
    <tr>
      <th>Normal</th>
      <td>77.275510</td>
      <td>73.469388</td>
      <td>59.846939</td>
    </tr>
    <tr>
      <th>Dragon</th>
      <td>83.312500</td>
      <td>112.125000</td>
      <td>86.375000</td>
    </tr>
  </tbody>
</table>
</div>

---
layout: post
title: Learning SQL with Pokemon (Part 2 of 2)
---

Welcome to the world of Pokemon! Most people who come through here are looking to start their Pokemon journey by catching, training and discovering Pokemon. It is important for people to go out into the unknown and discover secrets about Pokemon that we have yet to unravel.

<img style="float: right;" src="https://cdn.bulbagarden.net/upload/e/e2/133Eevee.png" width="150" height="150" />

However, there's another option you may take, one that is equally as important, if not more important. You could become a Pokemon Researcher and help construct our Pokemon Database! Trainers out in the field need to have a wealth of accurate information in order for them to make informed decisions on their journey. You will still get the chance interact with Pokemon on a daily basis, but your main role is to work with our new SQL technology to create data tables that will best inform our trainers. Your work here in the Research Lab will affect the lives of thousands, maybe even millions of people!

Does that sound like a journey worth embarking on? When you are ready, press START!

# Before we begin

NOTE: I have added Abilites.xlsx to my SQL Server Pokemon Database so we can demonstrate JOINs, UNIONs and Subqueries


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

<img style="float: right;" src="https://cdn.bulbagarden.net/upload/5/53/054Psyduck.png" width="150" height="150" />
# What is joining tables?


To put it simply, SQL has the ability to "join" two tables together when they share values in common. There are 3 main types of joins: Cross Joins, Inner Joins and Outer Joins. I think an example would help explain this better.

The abilities table contains the Pokemon abilities absent from our main Pokemon data table. So let's join them together!


```python
# First we do a Cross Join, and...something doesn't look right
df = pd.read_sql_query('''
SELECT
    p.[Name],
    [Type 1],
    [Type 2],
    [Ability 1],
    [Ability 2],
    [Generation]
FROM pokemon AS p
CROSS JOIN abilities AS a
''', conn)

df.head(10)
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Name</th>
      <th>Type 1</th>
      <th>Type 2</th>
      <th>Ability 1</th>
      <th>Ability 2</th>
      <th>Generation</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Bulbasaur</td>
      <td>Grass</td>
      <td>Poison</td>
      <td>Overgrow</td>
      <td>-----</td>
      <td>1.0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Ivysaur</td>
      <td>Grass</td>
      <td>Poison</td>
      <td>Overgrow</td>
      <td>-----</td>
      <td>1.0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Venusaur</td>
      <td>Grass</td>
      <td>Poison</td>
      <td>Overgrow</td>
      <td>-----</td>
      <td>1.0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>VenusaurMega Venusaur</td>
      <td>Grass</td>
      <td>Poison</td>
      <td>Overgrow</td>
      <td>-----</td>
      <td>1.0</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Charmander</td>
      <td>Fire</td>
      <td>None</td>
      <td>Overgrow</td>
      <td>-----</td>
      <td>1.0</td>
    </tr>
    <tr>
      <th>5</th>
      <td>Charmeleon</td>
      <td>Fire</td>
      <td>None</td>
      <td>Overgrow</td>
      <td>-----</td>
      <td>1.0</td>
    </tr>
    <tr>
      <th>6</th>
      <td>Charizard</td>
      <td>Fire</td>
      <td>Flying</td>
      <td>Overgrow</td>
      <td>-----</td>
      <td>1.0</td>
    </tr>
    <tr>
      <th>7</th>
      <td>CharizardMega Charizard X</td>
      <td>Fire</td>
      <td>Dragon</td>
      <td>Overgrow</td>
      <td>-----</td>
      <td>1.0</td>
    </tr>
    <tr>
      <th>8</th>
      <td>CharizardMega Charizard Y</td>
      <td>Fire</td>
      <td>Flying</td>
      <td>Overgrow</td>
      <td>-----</td>
      <td>1.0</td>
    </tr>
    <tr>
      <th>9</th>
      <td>Squirtle</td>
      <td>Water</td>
      <td>None</td>
      <td>Overgrow</td>
      <td>-----</td>
      <td>1.0</td>
    </tr>
  </tbody>
</table>
</div>



That's not right! We all know that Charmander and Squirtle don't have Overgrow as an Ability! What gives!?

What Cross Joins does is that it creates a Cartesian Product between the two. So if you have m rows in table 1 and n rows in table 2, the result set is m x n rows.

Cross Joins do have their uses in other areas of SQL and Databases, but we need a different kind of join in order to get what we want. So let's try out an Inner Join!


```python
# First we do a Cross Join, and...something doesn't look right
df = pd.read_sql_query('''
SELECT
    p.[Name],
    [Type 1],
    [Type 2],
    [Ability 1],
    [Ability 2],
    [Generation]
FROM pokemon AS p
INNER JOIN abilities AS a
ON p.[Name] = a.[Name]
WHERE a.[Form] <> 'Mega'
''', conn)

df.head(10)
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Name</th>
      <th>Type 1</th>
      <th>Type 2</th>
      <th>Ability 1</th>
      <th>Ability 2</th>
      <th>Generation</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Abomasnow</td>
      <td>Grass</td>
      <td>Ice</td>
      <td>Snow Warning</td>
      <td>-----</td>
      <td>4.0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Abra</td>
      <td>Psychic</td>
      <td>None</td>
      <td>Synchronize</td>
      <td>Inner Focus</td>
      <td>1.0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Absol</td>
      <td>Dark</td>
      <td>None</td>
      <td>Pressure</td>
      <td>Super Luck</td>
      <td>3.0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Accelgor</td>
      <td>Bug</td>
      <td>None</td>
      <td>Hydration</td>
      <td>Sticky Hold</td>
      <td>5.0</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Aerodactyl</td>
      <td>Rock</td>
      <td>Flying</td>
      <td>Rock Head</td>
      <td>Pressure</td>
      <td>1.0</td>
    </tr>
    <tr>
      <th>5</th>
      <td>Aggron</td>
      <td>Steel</td>
      <td>Rock</td>
      <td>Sturdy</td>
      <td>Rock Head</td>
      <td>3.0</td>
    </tr>
    <tr>
      <th>6</th>
      <td>Aipom</td>
      <td>Normal</td>
      <td>None</td>
      <td>Run Away</td>
      <td>Pickup</td>
      <td>2.0</td>
    </tr>
    <tr>
      <th>7</th>
      <td>Alakazam</td>
      <td>Psychic</td>
      <td>None</td>
      <td>Synchronize</td>
      <td>Inner Focus</td>
      <td>1.0</td>
    </tr>
    <tr>
      <th>8</th>
      <td>Alomomola</td>
      <td>Water</td>
      <td>None</td>
      <td>Healer</td>
      <td>Hydration</td>
      <td>5.0</td>
    </tr>
    <tr>
      <th>9</th>
      <td>Altaria</td>
      <td>Dragon</td>
      <td>Flying</td>
      <td>Natural Cure</td>
      <td>-----</td>
      <td>3.0</td>
    </tr>
  </tbody>
</table>
</div>



That's much better! Instead of creating a Cartesian Product, we "joined" the Names from Pokemon and the Names from Abilities, and that allows us to Query the Ability 1 and Ability 2 columns. Now our Pokemon will have their Types, Base Stats and Abilities to go with them!

<img style="float: right;" src="https://cdn.bulbagarden.net/upload/4/41/373Salamence.png" width="150" height="150" />
# Situation 1 - Intimidated?


You are getting the rare opportunity to train with Bruno from the Elite Four! He uses mainly Fighting Type Pokemon, with high Attack power. We need Pokemon to be able to handle his team.

With our newfound ability to join tables, we can filter which Pokemon have the Ability Intimidate, which lowers the Attack power of the other Pokemon when it is sent into battle.


```python
df = pd.read_sql_query('''
SELECT
    p.[Name],
    [Type 1],
    [Type 2],
    [Ability 1],
    [Ability 2],
    [Generation]
FROM pokemon AS p
INNER JOIN abilities AS a
ON p.[Name] = a.[Name]
WHERE a.[Form] <> 'Mega'
AND [Ability 1] = 'Intimidate'
OR [Ability 2] = 'Intimidate'
''', conn)

df
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Name</th>
      <th>Type 1</th>
      <th>Type 2</th>
      <th>Ability 1</th>
      <th>Ability 2</th>
      <th>Generation</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Ekans</td>
      <td>Poison</td>
      <td>None</td>
      <td>Intimidate</td>
      <td>Shed Skin</td>
      <td>1.0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Arbok</td>
      <td>Poison</td>
      <td>None</td>
      <td>Intimidate</td>
      <td>Shed Skin</td>
      <td>1.0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Growlithe</td>
      <td>Fire</td>
      <td>None</td>
      <td>Intimidate</td>
      <td>Flash Fire</td>
      <td>1.0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Arcanine</td>
      <td>Fire</td>
      <td>None</td>
      <td>Intimidate</td>
      <td>Flash Fire</td>
      <td>1.0</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Tauros</td>
      <td>Normal</td>
      <td>None</td>
      <td>Intimidate</td>
      <td>Anger Point</td>
      <td>1.0</td>
    </tr>
    <tr>
      <th>5</th>
      <td>Gyarados</td>
      <td>Water</td>
      <td>Flying</td>
      <td>Intimidate</td>
      <td>-----</td>
      <td>1.0</td>
    </tr>
    <tr>
      <th>6</th>
      <td>Snubbull</td>
      <td>Fairy</td>
      <td>None</td>
      <td>Intimidate</td>
      <td>Run Away</td>
      <td>2.0</td>
    </tr>
    <tr>
      <th>7</th>
      <td>Granbull</td>
      <td>Fairy</td>
      <td>None</td>
      <td>Intimidate</td>
      <td>Quick Feet</td>
      <td>2.0</td>
    </tr>
    <tr>
      <th>8</th>
      <td>Stantler</td>
      <td>Normal</td>
      <td>None</td>
      <td>Intimidate</td>
      <td>Frisk</td>
      <td>2.0</td>
    </tr>
    <tr>
      <th>9</th>
      <td>Hitmontop</td>
      <td>Fighting</td>
      <td>None</td>
      <td>Intimidate</td>
      <td>Technician</td>
      <td>2.0</td>
    </tr>
    <tr>
      <th>10</th>
      <td>Mightyena</td>
      <td>Dark</td>
      <td>None</td>
      <td>Intimidate</td>
      <td>Quick Feet</td>
      <td>3.0</td>
    </tr>
    <tr>
      <th>11</th>
      <td>Masquerain</td>
      <td>Bug</td>
      <td>Flying</td>
      <td>Intimidate</td>
      <td>-----</td>
      <td>3.0</td>
    </tr>
    <tr>
      <th>12</th>
      <td>Mawile</td>
      <td>Steel</td>
      <td>Fairy</td>
      <td>Hyper Cutter</td>
      <td>Intimidate</td>
      <td>3.0</td>
    </tr>
    <tr>
      <th>13</th>
      <td>Salamence</td>
      <td>Dragon</td>
      <td>Flying</td>
      <td>Intimidate</td>
      <td>-----</td>
      <td>3.0</td>
    </tr>
    <tr>
      <th>14</th>
      <td>Staravia</td>
      <td>Normal</td>
      <td>Flying</td>
      <td>Intimidate</td>
      <td>-----</td>
      <td>4.0</td>
    </tr>
    <tr>
      <th>15</th>
      <td>Staraptor</td>
      <td>Normal</td>
      <td>Flying</td>
      <td>Intimidate</td>
      <td>-----</td>
      <td>4.0</td>
    </tr>
    <tr>
      <th>16</th>
      <td>Shinx</td>
      <td>Electric</td>
      <td>None</td>
      <td>Rivalry</td>
      <td>Intimidate</td>
      <td>4.0</td>
    </tr>
    <tr>
      <th>17</th>
      <td>Luxio</td>
      <td>Electric</td>
      <td>None</td>
      <td>Rivalry</td>
      <td>Intimidate</td>
      <td>4.0</td>
    </tr>
    <tr>
      <th>18</th>
      <td>Luxray</td>
      <td>Electric</td>
      <td>None</td>
      <td>Rivalry</td>
      <td>Intimidate</td>
      <td>4.0</td>
    </tr>
    <tr>
      <th>19</th>
      <td>Herdier</td>
      <td>Normal</td>
      <td>None</td>
      <td>Intimidate</td>
      <td>Sand Rush</td>
      <td>5.0</td>
    </tr>
    <tr>
      <th>20</th>
      <td>Stoutland</td>
      <td>Normal</td>
      <td>None</td>
      <td>Intimidate</td>
      <td>Sand Rush</td>
      <td>5.0</td>
    </tr>
    <tr>
      <th>21</th>
      <td>Sandile</td>
      <td>Ground</td>
      <td>Dark</td>
      <td>Intimidate</td>
      <td>Moxie</td>
      <td>5.0</td>
    </tr>
    <tr>
      <th>22</th>
      <td>Krokorok</td>
      <td>Ground</td>
      <td>Dark</td>
      <td>Intimidate</td>
      <td>Moxie</td>
      <td>5.0</td>
    </tr>
    <tr>
      <th>23</th>
      <td>Krookodile</td>
      <td>Ground</td>
      <td>Dark</td>
      <td>Intimidate</td>
      <td>Moxie</td>
      <td>5.0</td>
    </tr>
  </tbody>
</table>
</div>



Looking at our list, we see several good choices. Salamence and Gyarados would be definitely good picks against Bruno's team. The Flying type adavantage would make it difficult for him to do good damage to us. Arcanine wouldn't be too bad either. We are probably going to have to pick Pokemon outside this, because most of the other Pokemon with Intimidate are either Dark or Normal types, which get pummeled by Fighting types.

# Situation 2 - One on One Showdown!


Your rival, Blue, wants to challenge you to a One on One Pokemon Battle! He will use Mega Blaziken as his Pokemon of choice. Mega Blaziken is outrageously powerful, so using any Pokemon that are weak against it would be suicide. We need to pick a Pokemon that can do good damage while also having enough defense to tank hits.


```python
df = pd.read_sql_query('''
SELECT
    p.[Name],
    [Type 1],
    [Type 2],
    [Ability 1],
    [Ability 2],
    [HP],
    [Attack],
    [Defense],
    [Special Attack],
    [Special Defense],
    [Speed],
    [Generation]
FROM pokemon AS p
INNER JOIN abilities AS a
ON p.[Name] = a.[Name]
WHERE [Type 1] IN ('Water', 'Psychic', 'Dragon')
AND [Defense] > 80
AND [Attack] > 80
AND [Legendary] = 0
OR [Type 2] IN ('Water', 'Psychic', 'Dragon')
AND [Defense] > 80
AND [Attack] > 80
AND [Legendary] = 0
''', conn)

df.head(100)
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Name</th>
      <th>Type 1</th>
      <th>Type 2</th>
      <th>Ability 1</th>
      <th>Ability 2</th>
      <th>HP</th>
      <th>Attack</th>
      <th>Defense</th>
      <th>Special Attack</th>
      <th>Special Defense</th>
      <th>Speed</th>
      <th>Generation</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Barbaracle</td>
      <td>Rock</td>
      <td>Water</td>
      <td>Tough Claws</td>
      <td>Sniper</td>
      <td>72.0</td>
      <td>105.0</td>
      <td>115.0</td>
      <td>54.0</td>
      <td>86.0</td>
      <td>68.0</td>
      <td>6.0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Blastoise</td>
      <td>Water</td>
      <td>None</td>
      <td>Torrent</td>
      <td>-----</td>
      <td>79.0</td>
      <td>83.0</td>
      <td>100.0</td>
      <td>85.0</td>
      <td>105.0</td>
      <td>78.0</td>
      <td>1.0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Blastoise</td>
      <td>Water</td>
      <td>None</td>
      <td>Mega Launcher</td>
      <td>-----</td>
      <td>79.0</td>
      <td>83.0</td>
      <td>100.0</td>
      <td>85.0</td>
      <td>105.0</td>
      <td>78.0</td>
      <td>1.0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Bronzong</td>
      <td>Steel</td>
      <td>Psychic</td>
      <td>Levitate</td>
      <td>Heatproof</td>
      <td>67.0</td>
      <td>89.0</td>
      <td>116.0</td>
      <td>79.0</td>
      <td>116.0</td>
      <td>33.0</td>
      <td>4.0</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Carracosta</td>
      <td>Water</td>
      <td>Rock</td>
      <td>Solid Rock</td>
      <td>Sturdy</td>
      <td>74.0</td>
      <td>108.0</td>
      <td>133.0</td>
      <td>83.0</td>
      <td>65.0</td>
      <td>32.0</td>
      <td>5.0</td>
    </tr>
    <tr>
      <th>5</th>
      <td>Celebi</td>
      <td>Psychic</td>
      <td>Grass</td>
      <td>Natural Cure</td>
      <td>-----</td>
      <td>100.0</td>
      <td>100.0</td>
      <td>100.0</td>
      <td>100.0</td>
      <td>100.0</td>
      <td>100.0</td>
      <td>2.0</td>
    </tr>
    <tr>
      <th>6</th>
      <td>Cloyster</td>
      <td>Water</td>
      <td>Ice</td>
      <td>Shell Armor</td>
      <td>Skill Link</td>
      <td>50.0</td>
      <td>95.0</td>
      <td>180.0</td>
      <td>85.0</td>
      <td>45.0</td>
      <td>70.0</td>
      <td>1.0</td>
    </tr>
    <tr>
      <th>7</th>
      <td>Crawdaunt</td>
      <td>Water</td>
      <td>Dark</td>
      <td>Hyper Cutter</td>
      <td>Shell Armor</td>
      <td>63.0</td>
      <td>120.0</td>
      <td>85.0</td>
      <td>90.0</td>
      <td>55.0</td>
      <td>55.0</td>
      <td>3.0</td>
    </tr>
    <tr>
      <th>8</th>
      <td>Dragonite</td>
      <td>Dragon</td>
      <td>Flying</td>
      <td>Inner Focus</td>
      <td>-----</td>
      <td>91.0</td>
      <td>134.0</td>
      <td>95.0</td>
      <td>100.0</td>
      <td>100.0</td>
      <td>80.0</td>
      <td>1.0</td>
    </tr>
    <tr>
      <th>9</th>
      <td>Druddigon</td>
      <td>Dragon</td>
      <td>None</td>
      <td>Rough Skin</td>
      <td>Sheer Force</td>
      <td>77.0</td>
      <td>120.0</td>
      <td>90.0</td>
      <td>60.0</td>
      <td>90.0</td>
      <td>48.0</td>
      <td>5.0</td>
    </tr>
    <tr>
      <th>10</th>
      <td>Empoleon</td>
      <td>Water</td>
      <td>Steel</td>
      <td>Torrent</td>
      <td>-----</td>
      <td>84.0</td>
      <td>86.0</td>
      <td>88.0</td>
      <td>111.0</td>
      <td>101.0</td>
      <td>60.0</td>
      <td>4.0</td>
    </tr>
    <tr>
      <th>11</th>
      <td>Exeggutor</td>
      <td>Grass</td>
      <td>Psychic</td>
      <td>Chlorophyll</td>
      <td>-----</td>
      <td>95.0</td>
      <td>95.0</td>
      <td>85.0</td>
      <td>125.0</td>
      <td>65.0</td>
      <td>55.0</td>
      <td>1.0</td>
    </tr>
    <tr>
      <th>12</th>
      <td>Feraligatr</td>
      <td>Water</td>
      <td>None</td>
      <td>Torrent</td>
      <td>-----</td>
      <td>85.0</td>
      <td>105.0</td>
      <td>100.0</td>
      <td>79.0</td>
      <td>83.0</td>
      <td>78.0</td>
      <td>2.0</td>
    </tr>
    <tr>
      <th>13</th>
      <td>Garchomp</td>
      <td>Dragon</td>
      <td>Ground</td>
      <td>Sand Veil</td>
      <td>-----</td>
      <td>108.0</td>
      <td>130.0</td>
      <td>95.0</td>
      <td>80.0</td>
      <td>85.0</td>
      <td>102.0</td>
      <td>4.0</td>
    </tr>
    <tr>
      <th>14</th>
      <td>Garchomp</td>
      <td>Dragon</td>
      <td>Ground</td>
      <td>Sand Force</td>
      <td>-----</td>
      <td>108.0</td>
      <td>130.0</td>
      <td>95.0</td>
      <td>80.0</td>
      <td>85.0</td>
      <td>102.0</td>
      <td>4.0</td>
    </tr>
    <tr>
      <th>15</th>
      <td>Gorebyss</td>
      <td>Water</td>
      <td>None</td>
      <td>Swift Swim</td>
      <td>-----</td>
      <td>55.0</td>
      <td>84.0</td>
      <td>105.0</td>
      <td>114.0</td>
      <td>75.0</td>
      <td>52.0</td>
      <td>3.0</td>
    </tr>
    <tr>
      <th>16</th>
      <td>Haxorus</td>
      <td>Dragon</td>
      <td>None</td>
      <td>Rivalry</td>
      <td>Mold Breaker</td>
      <td>76.0</td>
      <td>147.0</td>
      <td>90.0</td>
      <td>60.0</td>
      <td>70.0</td>
      <td>97.0</td>
      <td>5.0</td>
    </tr>
    <tr>
      <th>17</th>
      <td>Huntail</td>
      <td>Water</td>
      <td>None</td>
      <td>Swift Swim</td>
      <td>-----</td>
      <td>55.0</td>
      <td>104.0</td>
      <td>105.0</td>
      <td>94.0</td>
      <td>75.0</td>
      <td>52.0</td>
      <td>3.0</td>
    </tr>
    <tr>
      <th>18</th>
      <td>Hydreigon</td>
      <td>Dark</td>
      <td>Dragon</td>
      <td>Levitate</td>
      <td>-----</td>
      <td>92.0</td>
      <td>105.0</td>
      <td>90.0</td>
      <td>125.0</td>
      <td>90.0</td>
      <td>98.0</td>
      <td>5.0</td>
    </tr>
    <tr>
      <th>19</th>
      <td>Kabutops</td>
      <td>Rock</td>
      <td>Water</td>
      <td>Swift Swim</td>
      <td>Battle Armor</td>
      <td>60.0</td>
      <td>115.0</td>
      <td>105.0</td>
      <td>65.0</td>
      <td>70.0</td>
      <td>80.0</td>
      <td>1.0</td>
    </tr>
    <tr>
      <th>20</th>
      <td>Kingdra</td>
      <td>Water</td>
      <td>Dragon</td>
      <td>Swift Swim</td>
      <td>Sniper</td>
      <td>75.0</td>
      <td>95.0</td>
      <td>95.0</td>
      <td>95.0</td>
      <td>95.0</td>
      <td>85.0</td>
      <td>2.0</td>
    </tr>
    <tr>
      <th>21</th>
      <td>Kingler</td>
      <td>Water</td>
      <td>None</td>
      <td>Hyper Cutter</td>
      <td>Shell Armor</td>
      <td>55.0</td>
      <td>130.0</td>
      <td>115.0</td>
      <td>50.0</td>
      <td>50.0</td>
      <td>75.0</td>
      <td>1.0</td>
    </tr>
    <tr>
      <th>22</th>
      <td>Krabby</td>
      <td>Water</td>
      <td>None</td>
      <td>Hyper Cutter</td>
      <td>Shell Armor</td>
      <td>30.0</td>
      <td>105.0</td>
      <td>90.0</td>
      <td>25.0</td>
      <td>25.0</td>
      <td>50.0</td>
      <td>1.0</td>
    </tr>
    <tr>
      <th>23</th>
      <td>Malamar</td>
      <td>Dark</td>
      <td>Psychic</td>
      <td>Contrary</td>
      <td>Suction Cups</td>
      <td>86.0</td>
      <td>92.0</td>
      <td>88.0</td>
      <td>68.0</td>
      <td>75.0</td>
      <td>73.0</td>
      <td>6.0</td>
    </tr>
    <tr>
      <th>24</th>
      <td>Manaphy</td>
      <td>Water</td>
      <td>None</td>
      <td>Hydration</td>
      <td>-----</td>
      <td>100.0</td>
      <td>100.0</td>
      <td>100.0</td>
      <td>100.0</td>
      <td>100.0</td>
      <td>100.0</td>
      <td>4.0</td>
    </tr>
    <tr>
      <th>25</th>
      <td>Metagross</td>
      <td>Steel</td>
      <td>Psychic</td>
      <td>Clear Body</td>
      <td>-----</td>
      <td>80.0</td>
      <td>135.0</td>
      <td>130.0</td>
      <td>95.0</td>
      <td>90.0</td>
      <td>70.0</td>
      <td>3.0</td>
    </tr>
    <tr>
      <th>26</th>
      <td>Metagross</td>
      <td>Steel</td>
      <td>Psychic</td>
      <td>Tough Claws</td>
      <td>-----</td>
      <td>80.0</td>
      <td>135.0</td>
      <td>130.0</td>
      <td>95.0</td>
      <td>90.0</td>
      <td>70.0</td>
      <td>3.0</td>
    </tr>
    <tr>
      <th>27</th>
      <td>Mew</td>
      <td>Psychic</td>
      <td>None</td>
      <td>Synchronize</td>
      <td>-----</td>
      <td>100.0</td>
      <td>100.0</td>
      <td>100.0</td>
      <td>100.0</td>
      <td>100.0</td>
      <td>100.0</td>
      <td>1.0</td>
    </tr>
    <tr>
      <th>28</th>
      <td>Poliwrath</td>
      <td>Water</td>
      <td>Fighting</td>
      <td>Water Absorb</td>
      <td>Damp</td>
      <td>90.0</td>
      <td>95.0</td>
      <td>95.0</td>
      <td>70.0</td>
      <td>90.0</td>
      <td>70.0</td>
      <td>1.0</td>
    </tr>
    <tr>
      <th>29</th>
      <td>Quagsire</td>
      <td>Water</td>
      <td>Ground</td>
      <td>Damp</td>
      <td>Water Absorb</td>
      <td>95.0</td>
      <td>85.0</td>
      <td>85.0</td>
      <td>65.0</td>
      <td>65.0</td>
      <td>35.0</td>
      <td>2.0</td>
    </tr>
    <tr>
      <th>30</th>
      <td>Relicanth</td>
      <td>Water</td>
      <td>Rock</td>
      <td>Swift Swim</td>
      <td>Rock Head</td>
      <td>100.0</td>
      <td>90.0</td>
      <td>130.0</td>
      <td>45.0</td>
      <td>65.0</td>
      <td>55.0</td>
      <td>3.0</td>
    </tr>
    <tr>
      <th>31</th>
      <td>Samurott</td>
      <td>Water</td>
      <td>None</td>
      <td>Torrent</td>
      <td>-----</td>
      <td>95.0</td>
      <td>100.0</td>
      <td>85.0</td>
      <td>108.0</td>
      <td>70.0</td>
      <td>70.0</td>
      <td>5.0</td>
    </tr>
    <tr>
      <th>32</th>
      <td>Shelgon</td>
      <td>Dragon</td>
      <td>None</td>
      <td>Rock Head</td>
      <td>-----</td>
      <td>65.0</td>
      <td>95.0</td>
      <td>100.0</td>
      <td>60.0</td>
      <td>50.0</td>
      <td>50.0</td>
      <td>3.0</td>
    </tr>
    <tr>
      <th>33</th>
      <td>Solrock</td>
      <td>Rock</td>
      <td>Psychic</td>
      <td>Levitate</td>
      <td>-----</td>
      <td>70.0</td>
      <td>95.0</td>
      <td>85.0</td>
      <td>55.0</td>
      <td>65.0</td>
      <td>70.0</td>
      <td>3.0</td>
    </tr>
    <tr>
      <th>34</th>
      <td>Swampert</td>
      <td>Water</td>
      <td>Ground</td>
      <td>Torrent</td>
      <td>-----</td>
      <td>100.0</td>
      <td>110.0</td>
      <td>90.0</td>
      <td>85.0</td>
      <td>90.0</td>
      <td>60.0</td>
      <td>3.0</td>
    </tr>
    <tr>
      <th>35</th>
      <td>Swampert</td>
      <td>Water</td>
      <td>Ground</td>
      <td>Swift Swim</td>
      <td>-----</td>
      <td>100.0</td>
      <td>110.0</td>
      <td>90.0</td>
      <td>85.0</td>
      <td>90.0</td>
      <td>60.0</td>
      <td>3.0</td>
    </tr>
    <tr>
      <th>36</th>
      <td>Tyrantrum</td>
      <td>Rock</td>
      <td>Dragon</td>
      <td>Strong Jaw</td>
      <td>-----</td>
      <td>82.0</td>
      <td>121.0</td>
      <td>119.0</td>
      <td>69.0</td>
      <td>59.0</td>
      <td>71.0</td>
      <td>6.0</td>
    </tr>
  </tbody>
</table>
</div>



A lot of considerations here. We went with Base Stats for Attack and Defense above 80 since Mega Blaziken is going to hit really hard. Swampert wouldn't be too bad with his big HP and good tanking stats. Garchomp and its Sand Force ability could allow us to hit his Mega Blaziken harder. Dragonite would be a good pick also.

# What is a Subquery?

Now we are getting to the cool stuff. Subqueries allow us to nest queries inside queries. Let's look at an example:


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
WHERE [Special Attack] > (
    SELECT
        AVG([Special Attack])
    FROM pokemon
    WHERE [Speed] > 50
    AND [Type 1] = 'Water'
)
AND [Defense] > 135
''', conn)

df.head(100)
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
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
      <td>SlowbroMega Slowbro</td>
      <td>Water</td>
      <td>Psychic</td>
      <td>95.0</td>
      <td>75.0</td>
      <td>180.0</td>
      <td>130.0</td>
      <td>80.0</td>
      <td>30.0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Cloyster</td>
      <td>Water</td>
      <td>Ice</td>
      <td>50.0</td>
      <td>95.0</td>
      <td>180.0</td>
      <td>85.0</td>
      <td>45.0</td>
      <td>70.0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>TyranitarMega Tyranitar</td>
      <td>Rock</td>
      <td>Dark</td>
      <td>100.0</td>
      <td>164.0</td>
      <td>150.0</td>
      <td>95.0</td>
      <td>120.0</td>
      <td>71.0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Torkoal</td>
      <td>Fire</td>
      <td>None</td>
      <td>70.0</td>
      <td>85.0</td>
      <td>140.0</td>
      <td>85.0</td>
      <td>70.0</td>
      <td>20.0</td>
    </tr>
    <tr>
      <th>4</th>
      <td>MetagrossMega Metagross</td>
      <td>Steel</td>
      <td>Psychic</td>
      <td>80.0</td>
      <td>145.0</td>
      <td>150.0</td>
      <td>105.0</td>
      <td>110.0</td>
      <td>110.0</td>
    </tr>
    <tr>
      <th>5</th>
      <td>Groudon</td>
      <td>Ground</td>
      <td>None</td>
      <td>100.0</td>
      <td>150.0</td>
      <td>140.0</td>
      <td>100.0</td>
      <td>90.0</td>
      <td>90.0</td>
    </tr>
    <tr>
      <th>6</th>
      <td>GroudonPrimal Groudon</td>
      <td>Ground</td>
      <td>Fire</td>
      <td>100.0</td>
      <td>180.0</td>
      <td>160.0</td>
      <td>150.0</td>
      <td>90.0</td>
      <td>90.0</td>
    </tr>
    <tr>
      <th>7</th>
      <td>Cofagrigus</td>
      <td>Ghost</td>
      <td>None</td>
      <td>58.0</td>
      <td>50.0</td>
      <td>145.0</td>
      <td>95.0</td>
      <td>105.0</td>
      <td>30.0</td>
    </tr>
    <tr>
      <th>8</th>
      <td>Diancie</td>
      <td>Rock</td>
      <td>Fairy</td>
      <td>50.0</td>
      <td>100.0</td>
      <td>150.0</td>
      <td>100.0</td>
      <td>150.0</td>
      <td>50.0</td>
    </tr>
  </tbody>
</table>
</div>



So the logic for this query starts with the WHERE statement. The WHERE Statement is saying, "I want to find the Special Attack that is higher than the Average Special Attack of Pokemon with a Speed base Stat of 50 AND are Water Types". After that, we do another where statement on Pokemon with a Defense Base Stat of over 135.

Diving into all of the use-cases of Subqueries is outside of the scope of this Juypter Notebook. Just be aware that Subqueries will allow you to perform logic on Data Tables in creative ways, while maintaining readability.

# Situation 3 - Gym Leader Double Defense

Gym Leaders Erika and Sabrina have teamed up to ward off Giovanni from taking over Saffron City! They will each carry 3 Pokemon from their respective rosters, and team up to fight against Team Rocket!


```python
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
WHERE [Type 1] = 'Psychic'
AND Legendary = 0
AND [Name] NOT LIKE '%Mega%'
AND [Special Attack] > 120
AND [Speed] > 70

UNION ALL

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
WHERE [Type 1] = 'Grass'
AND Legendary = 0
AND [Name] NOT LIKE '%Mega%'
AND [Special Defense] > 90
AND [Defense] > 90
AND [HP] > 70
''', conn)

df.head(100)
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
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
      <td>Alakazam</td>
      <td>Psychic</td>
      <td>None</td>
      <td>500.0</td>
      <td>55.0</td>
      <td>50.0</td>
      <td>45.0</td>
      <td>135.0</td>
      <td>95.0</td>
      <td>120.0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Espeon</td>
      <td>Psychic</td>
      <td>None</td>
      <td>525.0</td>
      <td>65.0</td>
      <td>65.0</td>
      <td>60.0</td>
      <td>130.0</td>
      <td>95.0</td>
      <td>110.0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Gardevoir</td>
      <td>Psychic</td>
      <td>Fairy</td>
      <td>518.0</td>
      <td>68.0</td>
      <td>65.0</td>
      <td>65.0</td>
      <td>125.0</td>
      <td>115.0</td>
      <td>80.0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Bellossom</td>
      <td>Grass</td>
      <td>None</td>
      <td>490.0</td>
      <td>75.0</td>
      <td>80.0</td>
      <td>95.0</td>
      <td>90.0</td>
      <td>100.0</td>
      <td>50.0</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Serperior</td>
      <td>Grass</td>
      <td>None</td>
      <td>528.0</td>
      <td>75.0</td>
      <td>75.0</td>
      <td>95.0</td>
      <td>75.0</td>
      <td>95.0</td>
      <td>113.0</td>
    </tr>
    <tr>
      <th>5</th>
      <td>Ferrothorn</td>
      <td>Grass</td>
      <td>Steel</td>
      <td>489.0</td>
      <td>74.0</td>
      <td>94.0</td>
      <td>131.0</td>
      <td>54.0</td>
      <td>116.0</td>
      <td>20.0</td>
    </tr>
  </tbody>
</table>
</div>



We introduce UNIONS for this Situation. UNIONS allow us to combine two sets of SELECT statements as long as the columns are the same in both of them. In this scenario, I used multiple WHERE statements to construct a 6 Party team for Sabrina (Psychic Types) and Erika (Grass Types).


# Situation 4 - Lt. Surge promoted?

Major General Olivier Mira Armstrong has asked Lt. Surge to come out of retirement to defend The Northern Wall of Briggs. Armstrong realizes Surge's leadership abilities, and promoted him to Major for the upcoming battle. Surge has asked you to take over the gym while he's away, but you have to use his Pokemon as part of the deal.


```python
df = pd.read_sql_query('''
WITH surge AS (
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
FROM [Pokemon].[dbo].[pokemon]
WHERE [Type 1] = 'Electric'
AND Legendary = 0
AND [Name] NOT LIKE '%Mega%'
)

SELECT *
FROM surge
WHERE [Speed] > 120
AND [Special Attack] > 100

UNION ALL

SELECT *
FROM surge
WHERE [Defense] > 100
AND [Special Attack] > 120

UNION ALL

SELECT *
FROM surge
WHERE [Attack] > 100
AND [Speed] > 90
''', conn)

df.head(100)
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
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
      <td>Jolteon</td>
      <td>Electric</td>
      <td>None</td>
      <td>65.0</td>
      <td>65.0</td>
      <td>60.0</td>
      <td>110.0</td>
      <td>95.0</td>
      <td>130.0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Magnezone</td>
      <td>Electric</td>
      <td>Steel</td>
      <td>70.0</td>
      <td>70.0</td>
      <td>115.0</td>
      <td>130.0</td>
      <td>90.0</td>
      <td>60.0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Electivire</td>
      <td>Electric</td>
      <td>None</td>
      <td>75.0</td>
      <td>123.0</td>
      <td>67.0</td>
      <td>95.0</td>
      <td>85.0</td>
      <td>95.0</td>
    </tr>
  </tbody>
</table>
</div>



Combining UNIONS with Common Table Expressions (CTE) we can create multiple SELECT statements while keeping readability. The CTE at the top we define as "surge" and we get the base stats, the name and we have a WHERE statement on Electric types. After that, we can use surge as a temporary table to query off of. We then have 3 seperate SELECT statements linked together with UNION ALLs in order to get our 3 Pokemon team!

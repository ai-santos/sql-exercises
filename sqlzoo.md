##SQL ZOO 

#Table of Contents
[Main Page](https://github.com/lumodon/pastoral-rhea/blob/master/README.md)<br><br>
[SELECT Basics](#select-basics)<br>
[SELECT from WORLD Tutorial](#select-from-world-tutorial)<br>
[SELECT from Nobel Tutorial](#select-from-nobel-tutorial)<br>
[SELECT within SELECT Tutorial](#select-within-select-tutorial)<br>
[SUM and COUNT](#sum-and-count)<br>
[The JOIN operation](#the-join-operation)<br>
[Self JOIN](#self-join)<br>
[Using NULL](#using-null)<br>
[More JOIN operations](#more-join-operations)<br>
[Adventure Works Bonus Questions](#adventure-works-bonus-questions)<br>

## SELECT Basics
[Back to Table of Contents](#table-of-contents)
* Using table (world) from: http://sqlzoo.net/wiki/SELECT_basics

* The example uses a WHERE clause to show the population of 'France'. Note that strings (pieces of text that are data) should be in 'single quotes';
Modify it to show the population of Germany

```
SELECT population FROM world
  WHERE name = 'Germany'
```

* Checking a list The word IN allows us to check if an item is in a list. The example shows the name and population for the countries 'Luxembourg', 'Mauritius' and 'Samoa'.
Show the name and the population for 'Ireland', 'Iceland' and 'Denmark'.

```
SELECT name, population FROM world
  WHERE name IN ('Ireland', 'Iceland', 'Denmark')
```

* Which countries are not too small and not too big? BETWEEN allows range checking (range specified is inclusive of boundary values). The example below shows countries with an area of 250,000-300,000 sq. km.
Modify it to show the country and the area for countries with an area between 200,000 and 250,000.

```
SELECT name, area FROM world
  WHERE area BETWEEN 200000 AND 250000
```

## SELECT from WORLD Tutorial
[Back to Table of Contents](#table-of-contents)
* Read the notes about this table. Observe the result of running a simple SQL command.

```
SELECT name, continent, population FROM world
```

* How to use WHERE to filter records. Show the name for the countries that have a population of at least 200 million. 200 million is 200000000, there are eight zeros.

```
SELECT name FROM world
WHERE population>200000000
```

* Give the name and the per capita GDP for those countries with a population of at least 200 million.
HELP:How to calculate per capita GDP

```
SELECT name, gdp/population FROM world WHERE population>200000000  
```

* Show the name and population in millions for the countries of the continent 'South America'. Divide the population by 1000000 to get population in millions.

```
SELECT name, population/1000000 FROM world WHERE continent='South America'
```

* Show the name and population for France, Germany, Italy

```
SELECT name, population FROM world WHERE name IN ('France', 'Germany', 'Italy') 
```

* Show the countries which have a name that includes the word 'United'

```
SELECT name FROM world WHERE name LIKE 'United%'
```

* Two ways to be big: A country is big if it has an area of more than 3 million sq km or it has a population of more than 250 million.
Show the countries that are big by area or big by population. Show name, population and area.

```
SELECT name, population, area
FROM world
WHERE area>3000000
OR population>250000000 
```

* Exclusive OR (XOR). Show the countries that are big by area or big by population but not both. Show name, population and area.
Australia has a big area but a small population, it should be included.
Indonesia has a big population but a small area, it should be included.
China has a big population and big area, it should be excluded.
United Kingdom has a small population and a small area, it should be excluded.

```
SELECT name, population, area
FROM world
WHERE  area>3000000
XOR population>250000000 
```

* Show the name and population in millions and the GDP in billions for the countries of the continent 'South America'. Use the ROUND function to show the values to two decimal places.
For South America show population in millions and GDP in billions both to 2 decimal places.
Millions and billions

```
SELECT name,
ROUND(population/1000000,2),
ROUND(GDP/1000000000,2) 
FROM world 
WHERE continent='South America'
```

* Show the name and per-capita GDP for those countries with a GDP of at least one trillion (1000000000000; that is 12 zeros). Round this value to the nearest 1000.
Show per-capita GDP for the trillion dollar countries to the nearest $1000.

```
SELECT name, 
ROUND(gdp/population,-3)
FROM world
WHERE gdp>1000000000000
```

* The CASE statement shown is used to substitute North America for Caribbean in the third column.
Show the name - but substitute Australasia for Oceania - for countries beginning with N.

```
SELECT name,
       CASE WHEN continent='Oceania' THEN 'Australasia'
            ELSE continent END
  FROM world
 WHERE name LIKE 'N%'
```

* Show the name and the continent - but substitute Eurasia for Europe and Asia; substitute America - for each country in North America or South America or Caribbean. Show countries beginning with A or B

```
SELECT name,  
       CASE WHEN continent IN ('Europe', 'Asia') 
                 THEN 'Eurasia'
                 WHEN continent IN ('North America', 'South America','Caribbean') 
                 THEN 'America' 
                 ELSE continent 
        END 
FROM world WHERE name LIKE 'A%' OR name LIKE 'B%'
```

* Show the name and the continent - but substitute Eurasia for Europe and Asia; substitute America - for each country in North America or South America or Caribbean. Show countries beginning with A or B

```
SELECT name,  
       CASE WHEN continent IN ('Europe', 'Asia') 
                 THEN 'Eurasia'
                 WHEN continent IN ('North America', 'South America','Caribbean') 
                 THEN 'America' 
                 ELSE continent 
        END 
FROM world WHERE name LIKE 'A%' OR name LIKE 'B%'
```
* Put the continents right...
Oceania becomes Australasia
Countries in Eurasia and Turkey go to Europe/Asia
Caribbean islands starting with 'B' go to North America, other Caribbean islands go to South America
Order by country name in ascending order
Test your query using the WHERE clause with the following:
WHERE tld IN ('.ag','.ba','.bb','.ca','.cn','.nz','.ru','.tr','.uk')
Show the name, the original continent and the new continent of all countries.

```
SELECT name, continent, CASE WHEN continent='Eurasia' THEN 'Europe/Asia' 
WHEN name='Turkey' THEN 'Europe/Asia'
WHEN continent='Oceania' THEN 'Australasia'

WHEN continent='Caribbean' AND name LIKE 'B%' THEN 'North America'
WHEN continent='Caribbean' THEN 'South America' 
 ELSE continent END
  FROM world
WHERE tld IN ('.ag','.ba','.bb','.ca','.cn','.nz','.ru','.tr','.uk')
ORDER BY name ASC
```

## SELECT from Nobel Tutorial
[Back to Table of Contents](#table-of-contents)
* Using table (nobel) from: http://sqlzoo.net/wiki/SELECT_from_Nobel_Tutorial

* Change the query shown so that it displays Nobel prizes for 1950.

```
SELECT yr, subject, winner
  FROM nobel
 WHERE yr = 1950
```

* Show who won the 1962 prize for Literature.

```
SELECT winner
  FROM nobel
 WHERE yr = 1962
   AND subject = 'Literature'
```

* Show the year and subject that won 'Albert Einstein' his prize.

```
SELECT yr, subject
  FROM nobel
 WHERE winner='Albert Einstein'
```

* Give the name of the 'Peace' winners since the year 2000, including 2000.

```
SELECT winner
  FROM nobel
 WHERE subject='Peace' AND yr>=2000
```

* Show all details (yr, subject, winner) of the Literature prize winners for 1980 to 1989 inclusive.

```
SELECT * FROM nobel WHERE yr BETWEEN 1980 AND 1989 AND subject = "Literature";
```

* Show all details of the presidential winners:
   * Theodore Roosevelt
   * Woodrow Wilson
   * Jimmy Carter

```
SELECT * FROM nobel
 WHERE winner IN ('Theodore Roosevelt',
                  'Woodrow Wilson',
                  'Jimmy Carter')
```

* Show the winners with first name John

```
SELECT winner FROM nobel
 WHERE winner LIKE 'John%'
```

* Show the Physics winners for 1980 together with the Chemistry winners for 1984.

```
SELECT * FROM nobel
WHERE (subject='Physics' AND yr=1980) OR (subject='Chemistry' AND yr=1984)
```

* Show the winners for 1980 excluding the Chemistry and Medicine

```
SELECT * FROM nobel
WHERE yr=1980 AND subject NOT IN ('Chemistry', 'Medicine') 
```

* Show who won a 'Medicine' prize in an early year (before 1910, not including 1910) together with winners of a 'Literature' prize in a later year (after 2004, including 2004)

```
SELECT * FROM nobel
WHERE (subject='Medicine' AND yr<1910) OR (subject='Literature' AND yr>=2004)
```
* Find all details of the prize won by PETER GRÜNBERG
Non-ASCII characters

```
SELECT * FROM nobel WHERE winner LIKE 'Peter grÜnberg'
```
* Find all details of the prize won by EUGENE O'NEILL

```
SELECT * FROM nobel WHERE winner = "EUGENE O'NEILL"
```
* Knights in order
List the winners, year and subject where the winner starts with Sir. Show the the most recent first, then by name order.

```
SELECT winner, yr, subject FROM nobel WHERE winner LIKE 'Sir %' ORDER BY yr DESC, winner
```
* The expression subject IN ('Chemistry','Physics') can be used as a value - it will be 0 or 1.
Show the 1984 winners and subject ordered by subject and winner name; but list Chemistry and Physics last.

```
SELECT winner, subject
  FROM nobel
 WHERE yr=1984
 ORDER BY subject IN ('Physics','Chemistry') ASC, subject, winner
```

## SELECT within SELECT Tutorial
[Back to Table of Contents](#table-of-contents)
* Using table (world) from: http://sqlzoo.net/wiki/SELECT_within_SELECT_Tutorial

* List each country name where the population is larger than that of 'Russia'.

```
SELECT name FROM world
  WHERE population >
     (SELECT population FROM world
      WHERE name='Russia')
```

* Show the countries in Europe with a per capita GDP greater than 'United Kingdom'.
Per Capita GDP

```
SELECT name FROM world  
WHERE continent = 'Europe' AND GDP/population > 
(SELECT GDP/population FROM world
WHERE name = 'United Kingdom')
```

* List the name and continent of countries in the continents containing either Argentina or Australia. Order by name of the country.

```
SELECT name, continent FROM world 
WHERE continent IN (SELECT continent FROM world 
WHERE name IN ('Argentina' ,'Australia')) ORDER BY name
```

* Which country has a population that is more than Canada but less than Poland? Show the name and the population.

```
SELECT name, population FROM world 
WHERE population > (SELECT population FROM world 
WHERE name = 'CANADA') AND population < 
(SELECT population FROM world WHERE name = 'Poland')
```

* Germany (population 80 million) has the largest population of the countries in Europe. Austria (population 8.5 million) has 11% of the population of Germany.
Show the name and the population of each country in Europe. Show the population as a percentage of the population of Germany.
Decimal places
Percent symbol %

```
SELECT name, concat(ROUND(population/(SELECT population
FROM world WHERE name='Germany') * 100, 0), '%') AS population 
FROM world WHERE continent='Europe'
```

* Which countries have a GDP greater than every country in Europe? [Give the name only.] (Some countries may have NULL gdp values)

```
SELECT name FROM world 
WHERE  gdp > 
(SELECT MAX(gdp) FROM world WHERE continent = 'Europe')
```

* Find the largest country (by area) in each continent, show the continent, the name and the area:

```
SELECT continent, name, area FROM world x
  WHERE area >= ALL
    (SELECT area FROM world y
        WHERE y.continent=x.continent
          AND area>0)
```

* List each continent and the name of the country that comes first alphabetically.

```
SELECT continent, name FROM world x 
WHERE name = 
ALL(SELECT name FROM world y 
WHERE x.continent = y.continent AND y.name < x.name)
```

* Find the continents where all countries have a population <= 25000000. Then find the names of the countries associated with these continents. Show name, continent and population **(Had to hard code 'Russia' exception probably due to an error from the DB)**.

```
SELECT name, continent, population FROM world x 
WHERE name != 'Russia' AND name = 
ALL(SELECT name FROM world y 
WHERE x.continent = y.continent AND population >= 25000000)
```
* Some countries have populations more than three times that of any of their neighbours (in the same continent). Give the countries and continents.

```
SELECT name, continent FROM world x 
WHERE population IS NOT NULL AND population = 
ALL(SELECT population FROM world y 
WHERE x.continent = y.continent AND x.population < 3 * y.population )
```

## SUM and COUNT
[Back to Table of Contents](#table-of-contents)

* Show the total population of the world.

```
SELECT SUM(population)
FROM world;
```

* List all the continents - just once each.

```
SELECT DISTINCT(continent) FROM world;
```

* Give the total GDP of Africa

```
SELECT SUM(gdp) FROM world WHERE continent='Africa';
```

* How many countries have an area of at least 1000000

```
SELECT COUNT(name) FROM world WHERE area >= 1000000;
```

* What is the total population of ('France','Germany','Spain')

```
SELECT SUM(population) FROM world WHERE name IN('France', 'Germany', 'Spain'); 
```

* For each continent show the continent and number of countries.

```
SELECT continent, COUNT(name) FROM world GROUP BY continent;
```

* For each continent show the continent and number of countries with populations of at least 10 million.

```
SELECT DISTINCT continent, COUNT(name) continent 
FROM world WHERE population > 10000000 
GROUP BY continent;
```

* List the continents that have a total population of at least 100 million.

```
SELECT DISTINCT continent 
FROM world GROUP BY continent 
HAVING SUM(population)>=100000000
```

## The JOIN operation
[Back to Table of Contents](#table-of-contents)

* The first example shows the goal scored by a player with the last name 'Bender'. The * says to list all the columns in the table - a shorter way of saying matchid, teamid, player, gtime
Modify it to show the matchid and player name for all goals scored by Germany. To identify German players, check for: teamid = 'GER'

```
SELECT matchid, player FROM goal 
  WHERE teamid='Ger';
```

* 

##SQL BOLT

#Table of Contents
[Main Page](https://github.com/lumodon/pastoral-rhea/blob/master/README.md)<br><br>
[QUERIES with Constraints](#queries-with-constraints)<br>
[SQL REVIEW: SIMPLE SELECT QUERIES](#sql-review)<br>
[SQL Outer Joins](#sql-outer-joins)<br>
[SQL Nulls](#sql-nulls)<br>
[SUM and COUNT](#sum-and-count)<br>
[The JOIN operation](#the-join-operation)<br>
[Self JOIN](#self-join)<br>
[Using NULL](#using-null)<br>
[More JOIN operations](#more-join-operations)<br>
[Adventure Works Bonus Questions](#adventure-works-bonus-questions)<br>

## QUERIES with Constraints
[Back to Table of Contents](#table-of-contents)

* The example uses a WHERE clause to show the population of 'France'. Note that strings (pieces of text that are data) should be in 'single quotes';
Modify it to show the population of Germany

```
SELECT population FROM world
  WHERE name = 'Germany'
```

## SQL Review
[Back to Table of Contents](#table-of-contents)
* List all the Canadian cities and their populations

```
SELECT city, population FROM north_american_cities WHERE country='Canada'; 
```

* Order all the cities in the United States by their latitude from north to south 

```
SELECT city, latitude FROM north_american_cities WHERE country='United States' ORDER BY latitude DESC; 
```

* List all cities west of Chicago, ordered from west to east

```
SELECT city, longitude FROM north_american_cities
WHERE longitude < -87.629798
ORDER BY longitude ASC;
```

* List the 2 largest cities in Mexico

```
SELECT city, population FROM north_american_cities
WHERE country LIKE "Mexico"
ORDER BY population DESC
LIMIT 2;
```

* List the third and fourth largest cities (by population) in the United States and their population 

```
SELECT city, population FROM north_american_cities WHERE country='United States' ORDER BY population DESC LIMIT 2 OFFSET 2;
```

* Find the domestic and international sales for each movie

```
SELECT title, domestic_sales, international_sales 
FROM movies
  JOIN boxoffice
    ON movies.id = boxoffice.movie_id;
```

* Show the sales numbers for each movie that did better internationally rather than domestically

```
SELECT name, population, area
FROM world
WHERE area>3000000
OR population>250000000 
```

* Find the domestic and international sales for each movie 

```
SELECT title, domestic_sales, international_sales 
FROM movies
  JOIN boxoffice
    WHERE international_sales > domestic_sales;
```

* Show the sales numbers for each movie that did better internationally rather than domestically

```
SELECT title, domestic_sales, international_sales
FROM movies
  JOIN boxoffice
    ON movies.id = boxoffice.movie_id
WHERE international_sales > domestic_sales;
```

* List all ratings by movie title in descending order

```
SELECT title, rating
FROM movies
  JOIN boxoffice
    ON movies.id = boxoffice.movie_id
ORDER BY rating DESC;
```


## SQL Outer Joins
[Back to Table of Contents](#table-of-contents)

* Find the list of all buildings that have employees

```
SELECT DISTINCT(building_name) FROM buildings
 JOIN employees 
    ON employees.building = buildings.building_name;
```

* Find the list of all the buildings and their capacity

```
SELECT DISTINCT(building_name), capacity FROM buildings
 LEFT JOIN employees 
    ON employees.building = buildings.building_name;
```

* List all buildings and the distinct employee roles in each building (including empty buildings)

```
SELECT DISTINCT building_name, role FROM buildings
 LEFT JOIN employees 
    ON building_name = building;
```


## SQL Nulls
[Back to Table of Contents](#table-of-contents)
* Using table (world) from: http://sqlzoo.net/wiki/SELECT_within_SELECT_Tutorial

* Find the name and role of all employees who have not been assigned to a building 

```
SELECT name, role FROM employees
WHERE building IS NULL;
```

* Find the names of the buildings that hold no employees 


```
SELECT DISTINCT building_name
FROM buildings 
  LEFT JOIN employees
    ON building_name = building
WHERE role IS NULL;
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

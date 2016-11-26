Leet Code Database Challenges
#Table of Contents

[Main Page](https://github.com/asantos3026/lisa-aileen-sql/blob/master/README.md)<br><br>
[Combine Two Tables](#combine-two-tables)<br>
[Second Highest Salary](#second-highest-salary)<br>
[Nth Highest Salary](#nth-highest-salary)<br>
[Rank Scores](#rank-scores)<br>
[Consecutive Numbers](#consecutive-numbers)<br>
[Employees Earning More Than Their Managers](#employees-earning-more-than-their-managers)<br>
[Self JOIN](#self-join)<br>
[Using NULL](#using-null)<br>
[More JOIN operations](#more-join-operations)<br>
[Adventure Works Bonus Questions](#adventure-works-bonus-questions)<br>

## Combine Two Tables
[Back to Table of Contents](#table-of-contents)

[Combine 2 Tables](http://i.imgur.com/Y9AsIfj.png)


```
SELECT FirstName, LastName, City, State FROM Person
LEFT JOIN Address ON Address.personid=Person.personid;
```

## Second Highest Salary
[Back to Table of Contents](#table-of-contents)

[Second Highest Salary](http://i.imgur.com/x2DGGfN.png)

```
SELECT MAX(Salary) AS SecondHighestSalary FROM Employee 
    WHERE Salary < ALL(SELECT MAX(Salary) FROM Employee);
```

## Nth Highest Salary
[Back to Table of Contents](#table-of-contents)

[Nth Highest Salary](http://i.imgur.com/7Zdr9xO.png)

```
SELECT name, area FROM world
    WHERE area BETWEEN 200000 AND 250000
```

## Rank Scores
[Back to Table of Contents](#table-of-contents)

[Rank Scores](http://i.imgur.com/prWt6ZZ.png)

```
SELECT name, continent, population FROM world
```

## Consecutive Numbers
[Back to Table of Contents](#table-of-contents)

[Consecutive Numbers](http://i.imgur.com/aOt4kPH.png)


```
SELECT name FROM world
WHERE population>200000000
```

## Employees Earning More Than Their Managers
[Back to Table of Contents](#table-of-contents)

[Employees Earning More Than Their Managers](http://i.imgur.com/aOt4kPH.png)


```
SELECT name, GDP/population FROM world WHERE population > 200000000
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


## SUM and COUNT
[Back to Table of Contents](#table-of-contents)
* Using table (world) from: http://sqlzoo.net/wiki/SUM_and_COUNT

* Show the total population of the world.

```
SELECT SUM(population)
FROM world
```

## The JOIN operation
[Back to Table of Contents](#table-of-contents)
* Using tables (game, goal, and eteam) from: http://sqlzoo.net/wiki/The_JOIN_operation

* The first example shows the goal scored by a player with the last name 'Bender'. The * says to list all the columns in the table - a shorter way of saying matchid, teamid, player, gtime
Modify it to show the matchid and player name for all goals scored by Germany. To identify German players, check for: teamid = 'GER'

```
SELECT matchid, player FROM goal 
  WHERE teamid = 'GER';
```

## Self JOIN
[Back to Table of Contents](#table-of-contents)
* Using tables (stops, and routes) from: http://sqlzoo.net/wiki/Self_join
stops( id, name )
route( num, company, pos, stop )

* How many stops are in the database.
```
SELECT COUNT(id) FROM stops
```

## More JOIN operations
[Back to Table of Contents](#table-of-contents)
* Using tables (movie, actor, and casting) from: http://sqlzoo.net/wiki/More_JOIN_operations
movie( id, title, yr, director, budget, gross )
actor( id, name )
casting( movieid, actorid, ord )

* List the films where the yr is 1962 [Show id, title]

```
SELECT id, title
 FROM movie
 WHERE yr=1962
```

## Harder Questions
[Back to Table of Contents](#table-of-contents)
* Which were the busiest years for 'John Travolta', show the year and the number of movies he made each year for any year in which he made more than 2 movies.
```
SELECT yr,COUNT(title) FROM
  movie JOIN casting ON movie.id=movieid
         JOIN actor   ON actorid=actor.id
WHERE name='John Travolta'
GROUP BY yr
HAVING COUNT(title)=(SELECT MAX(c) FROM
(SELECT yr,COUNT(title) AS c FROM
   movie JOIN casting ON movie.id=movieid
         JOIN actor   ON actorid=actor.id
 WHERE name='John Travolta'
 GROUP BY yr) AS t
)
```

## Adventure Works Bonus Questions

[Back to Table of Contents](#table-of-contents)

* Tables used at: http://sqlzoo.net/wiki/AdventureWorks

* Show the first name and the email address of customer with CompanyName 'Bike World'

```
SELECT FirstName, EmailAddress
FROM CustomerAW WHERE CompanyName = 'Bike World';
```


# SQLZOO Solutions
I've compiled the solutions to all of all 10 levels on the [SQLZOO Tutoral](http://sqlzoo.net/wiki/SQL_Tutorial).  

## Sections:
1. [SELECT basics](#select-basics)
2. [SELECT from WORLD](#select-from-world)
3. [SELECT from NOBEL](#select-from-nobel)
4. [SELECT in SELECT](#select-in-select)
5. [SUM and COUNT](#sum-and-count)
6. [JOIN](#join)
7. [More JOIN](#more-join)
8. [Using NULL](#using-null)
9. [Self JOIN](#self-join)

## SELECT basics

1.
```sql
SELECT population FROM world
  WHERE name = 'Germany'
```
2.
```sql
SELECT name, population FROM world
  WHERE name IN ('Sweden', 'Norway', 'Denmark');
```
3.
```sql
SELECT name, area FROM world
  WHERE area BETWEEN 200000 AND 250000
```

## SELECT from WORLD

1.
```sql
SELECT name, continent, population FROM world
```
2.
```sql
SELECT name FROM world
  WHERE population >= 200000000
```
3.
```sql
SELECT name, gdp/population AS 'per capital GDP' FROM world
  WHERE population >= 200000000
```
4.
```sql
SELECT name, population/1000000 AS populations FROM world
  WHERE continent = 'South America'
```
5.
```sql
SELECT name, population FROM world
  WHERE name IN ('France', 'Germany', 'Italy')
```
6.
```sql
SELECT name FROM world
  WHERE name LIKE '%united%'
```
7.
```sql
SELECT name, population, area FROM world
  WHERE area > 3000000 OR population > 250000000
```
8.
```sql
SELECT name, population, area FROM world
  WHERE area > 3000000 XOR population > 250000000
```
9.
```sql
SELECT name, ROUND(population/1000000,2) AS population, ROUND(gdp/1000000000,2) AS GDP FROM world
  WHERE continent = 'South America'
```
10.
```sql
SELECT name, ROUND(gdp/population,-3) FROM world
  WHERE gdp > 1000000000000
```
11.
```sql
SELECT name, capital FROM world
  WHERE LENGTH(name) = LENGTH(capital)
```
12.
```sql
SELECT name, capital FROM world
  WHERE name <> capital AND LEFT(name,1) = LEFT(capital,1)
```
13.
```sql
SELECT name FROM world
  WHERE name LIKE '%a%' AND name LIKE '%e%' AND name LIKE '%i%' AND name LIKE '%o%' AND name LIKE '%u%'
  AND name NOT LIKE '% %'
```

## SELECT from NOBEL

1.
```sql
SELECT yr, subject, winner FROM nobel
  WHERE yr = '1950'
```
2.
```sql
SELECT winner FROM nobel
  WHERE yr = '1962' AND subject = 'literature'
```
3.
```sql
SELECT yr, subject FROM nobel
  WHERE winner = 'Albert Einstein'
```
4.
```sql
SELECT winner FROM nobel
  WHERE subject = 'peace' AND yr >= 2000
```
5.
```sql
SELECT yr, subject, winner FROM nobel
  WHERE subject = 'literature' AND yr BETWEEN 1980 AND 1989
```
6.
```sql
SELECT * FROM nobel
  WHERE winner IN ('Theodore Roosevelt', 'Thomas Woodrow Wilson', 'Jimmy Carter', 'Barack Obama')
```
7.
```sql
SELECT winner FROM nobel
  WHERE winner LIKE "john%"
```
8.
```sql
SELECT yr, subject, winner FROM nobel
  WHERE (subject = 'physics' AND yr = '1980') OR (subject = 'chemistry' AND yr = '1984')
```
9.
```sql
SELECT yr, subject, winner FROM nobel
  WHERE subject NOT IN ('chemistry','medicine') AND yr = '1980'
```
10.
```sql
SELECT yr, subject, winner FROM nobel
  WHERE (subject = 'medicine' AND yr < 1910) OR (subject = 'literature' AND yr >= 2004)
```
11.
```sql
SELECT * FROM nobel
  WHERE winner LIKE 'peter%' AND winner LIKE '%GR_NBERG%'
```
12.
```sql
SELECT * FROM nobel
  WHERE winner LIKE 'eugene O''neill'
```
13.
```sql
SELECT winner, yr, subject FROM nobel
  WHERE winner LIKE 'sir%'
ORDER BY yr DESC, winner ASC
```
14.
```sql
SELECT winner, subject FROM nobel
 WHERE yr = 1984
 ORDER BY subject IN ('physics','chemistry'), subject, winner
```

## SELECT in SELECT

1.
```sql
SELECT name FROM world
  WHERE population > (SELECT population FROM world WHERE name = 'Russia')
```
2.
```sql
SELECT name FROM world
  WHERE continent = 'Europe' AND gdp/population > (SELECT gdp/population FROM world WHERE name = 'United Kingdom')
```
3.
```sql
SELECT name, continent FROM world
  WHERE continent IN (SELECT continent FROM world WHERE name = 'Argentina' OR name = 'Australia')
ORDER BY name
```
4.
```sql
SELECT name, population FROM world
  WHERE population > (SELECT population FROM world WHERE name = 'United Kingdom') AND
  population < (SELECT population FROM world WHERE name = 'Germany')
```
5.
```sql
SELECT name, CONCAT(ROUND(population/(SELECT population FROM world WHERE name = 'Germany')*100,0),'%') AS percentage FROM world
  WHERE continent = 'Europe'
```
6.
```sql
SELECT name FROM world
  WHERE gdp > ALL (SELECT gdp FROM world WHERE continent = 'Europe' AND gdp > 0)
```
7.
```sql
SELECT continent, name, area FROM world x
  WHERE area >= ALL (SELECT area FROM world y
                            WHERE y.continent = x.continent AND area >0)
```
8.
```sql
SELECT continent, name FROM world x
  WHERE name <= ALL (SELECT name FROM world y
                  WHERE y.continent = x.continent)
```
9.
```sql
SELECT continent, name, population FROM world x
  WHERE 25000000 <= ALL (SELECT population FROM world y
                             WHERE y.continent = x.continent)
```
10.
```sql
SELECT name, continent FROM world x
  WHERE population > ALL (SELECT population*3 FROM world y
                            WHERE y.continent = x.continent AND y.name <> x.name)
```

## SUM and COUNT

1.
```sql
SELECT SUM(population) FROM world
```
2.
```sql
SELECT DISTINCT(continent) FROM world
```
3.
```sql
SELECT SUM(gdp) FROM world
  WHERE continent = 'Africa'
```
4.
```sql
SELECT COUNT(name) FROM world
  WHERE area >= 1000000
```
5.
```sql
SELECT SUM(population) FROM world
  WHERE name IN ('Estonia', 'Latvia', 'Lithuania')
```
6.
```sql
SELECT continent, COUNT(name) FROM world
GROUP BY continent
```
7.
```sql
SELECT continent, COUNT(name) FROM world
  WHERE population >= 10000000
GROUP BY continent
```
8.
```sql
SELECT continent FROM world
GROUP BY continent
HAVING SUM(population) >= 100000000
```

## JOIN

1.
```sql
SELECT matchid, player FROM goal
  WHERE teamid = 'GER'
```
2.
```sql
SELECT id, stadium, team1, team2 FROM game
  WHERE id = '1012'
```
3.
```sql
SELECT player, teamid, stadium, mdate 
FROM goal JOIN game ON (game.id = goal.matchid)
  WHERE teamid = 'GER'
```
4.
```sql
SELECT team1, team2, player
FROM goal JOIN game ON (game.id = goal.matchid)
  WHERE player LIKE 'Mario%'
```
5.
```sql
SELECT player, teamid, coach, gtime
FROM goal JOIN eteam ON (goal.teamid = eteam.id)
  WHERE gtime <= 10
```
6.
```sql
SELECT mdate, teamname
FROM game JOIN eteam ON (game.team1 = eteam.id)
  WHERE eteam.coach = 'Fernando Santos'
```
7.
```sql
SELECT player 
FROM goal JOIN game ON (game.id = goal.matchid)
  WHERE stadium = 'National Stadium, Warsaw'
```
8.
```sql
SELECT DISTINCT(player)
  FROM game JOIN goal ON matchid = id 
    WHERE (team1 = 'GER' OR team2 = 'GER') AND teamid <> 'GER'
```
9.
```sql
SELECT teamname, COUNT(player)
FROM eteam JOIN goal ON (eteam.id = goal.teamid)
GROUP BY teamname
```
10.
```sql
SELECT stadium, COUNT(player)
FROM game JOIN goal ON (game.id = goal.matchid)
GROUP BY stadium
```
11.
```sql
SELECT matchid, mdate, COUNT(player)
FROM goal JOIN game ON (goal.matchid = game.id)
  WHERE (team1 = 'POL' OR team2 = 'POL')
GROUP BY matchid
```
12.
```sql
SELECT matchid, mdate, COUNT(player) 
FROM goal JOIN game ON (goal.matchid = game.id)
  WHERE (team1 = 'GER' OR team2 = 'GER') AND (teamid = 'GER')
GROUP BY matchid
```
13.
```sql
SELECT mdate, team1,
  SUM(CASE WHEN teamid = team1 THEN 1 
       ELSE 0 
  END) AS score1,
  team2,
  SUM(CASE WHEN teamid = team2 THEN 1
       ELSE 0
  END) AS score2
FROM game LEFT JOIN goal ON (goal.matchid = game.id)
GROUP BY mdate, team1, team2
ORDER BY mdate, matchid, team1, team2
```

## More JOIN

1.
```sql
SELECT id, title FROM movie
  WHERE yr = '1962'
```
2.
```sql
SELECT yr FROM movie
  WHERE title = 'Citizen Kane'
```
3.
```sql
SELECT id, title, yr FROM movie
  WHERE title LIKE '%Star Trek%'
ORDER BY yr
```
4.
```sql
SELECT id FROM actor
  WHERE name = 'Glenn Close'
```
5.
```sql
SELECT id FROM movie
  WHERE title = 'Casablanca'
```
6.
```sql
SELECT name
FROM casting JOIN actor ON casting.actorid = actor.id
  WHERE movieid = '11768'
```
7.
```sql
SELECT name
FROM casting JOIN actor ON (casting.actorid = actor.id)
             JOIN movie ON (casting.movieid = movie.id)
  WHERE title = 'Alien'
```
8.
```sql
SELECT title
FROM casting JOIN actor ON (casting.actorid = actor.id)
             JOIN movie ON (casting.movieid = movie.id)
  WHERE name = 'Harrison Ford'
```
9.
```sql
SELECT title
FROM casting JOIN actor ON (casting.actorid = actor.id)
             JOIN movie ON (casting.movieid = movie.id)
  WHERE name = 'Harrison Ford' AND ord <> 1
```
10.
```sql
SELECT title, name
FROM casting JOIN actor ON (casting.actorid = actor.id)
             JOIN movie ON (casting.movieid = movie.id)
  WHERE ord = 1 AND yr = '1962'
```
11.
```sql
SELECT yr, COUNT(title)
FROM casting JOIN movie ON (casting.movieid = movie.id)
             JOIN actor ON (casting.actorid = actor.id)
  WHERE name = 'Rock Hudson'
GROUP BY yr
HAVING COUNT(title) > 2
```
12.
```sql
SELECT title, name
FROM casting JOIN movie ON (casting.movieid = movie.id)
             JOIN actor ON (casting.actorid = actor.id)
  WHERE movieid IN (SELECT movieid FROM casting
                      WHERE actorid IN (SELECT id FROM actor
                                          WHERE name='Julie Andrews')
                    ) AND ord = 1
```
13.
```sql
SELECT name
FROM casting JOIN actor ON (casting.actorid = actor.id)
  WHERE ord = 1
GROUP BY name
HAVING COUNT(*) >= 15
ORDER BY name
```
14.
```sql
SELECT title, COUNT(actorid)
FROM casting JOIN movie ON (casting.movieid = movie.id)
  WHERE yr = '1978'
GROUP BY title
ORDER BY 2 DESC, 1 ASC
```
15.
```sql
SELECT name
FROM casting JOIN actor ON (casting.actorid = actor.id)
  WHERE movieid IN (SELECT movieid FROM casting
                     WHERE actorid = (SELECT id FROM actor WHERE name = 'Art Garfunkel')
                    ) AND name <> 'Art Garfunkel'
```

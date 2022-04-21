1. **List each country name where the population is larger than that of 'Russia'.**

```
world(name, continent, area, population, gdp)
```

```sql
SELECT name
FROM   world
WHERE  population > (SELECT population
                     FROM   world
                     WHERE  name = 'Russia');
```

2. **Show the countries in Europe with a per capita GDP greater than 'United Kingdom'.**

```sql
SELECT name
FROM   world
WHERE  continent = 'Europe'
       AND ( gdp / population ) > (SELECT gdp / population
                                   FROM   world
                                   WHERE  name = 'United Kingdom'); 
```

3. **List the name and continent of countries in the continents containing either Argentina or Australia. Order by name of the country.**

```sql
SELECT name,
       continent
FROM   world
WHERE  continent IN (SELECT continent
                     FROM   world
                     WHERE  name IN ( 'Argentina', 'Australia' ))
ORDER  BY name; 
```

4. **Which country has a population that is more than Canada but less than Poland? Show the name and the population.**

```sql
SELECT name,
       population
FROM   world
WHERE  population > (SELECT population
                     FROM   world
                     WHERE  name = 'Canada')
       AND population < (SELECT population
                         FROM   world
                         WHERE  name = 'Poland');
```

5. Germany (population 80 million) has the largest population of the countries in Europe. Austria (population 8.5 million) has 11% of the population of Germany.

**Show the name and the population of each country in Europe. Show the population as a percentage of the population of Germany.**

```sql
SELECT NAME,
       CONCAT(CAST(ROUND(population * 100 / (SELECT population
                                             FROM   world
                                             WHERE  name = 'Germany'), 0) AS INT
              ), '%')
       AS population
FROM   world
WHERE  continent = 'Europe'; 
```

6. **Which countries have a GDP greater than every country in Europe?**

```sql
SELECT name
FROM   world
WHERE  gdp >= ALL (SELECT gdp
                   FROM   world
                   WHERE  gdp > 0
                          AND continent = 'Europe')
       AND continent <> 'Europe';
```

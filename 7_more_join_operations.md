1. List the films where the **yr** is 1962 [Show **id**, **title**]

```sql
SELECT id,
       title
FROM   movie
WHERE  yr = 1962;
```

2. Give year of 'Citizen Kane'.

```sql
SELECT yr
FROM   movie
WHERE  title = 'Citizen Kane'; 
```

3. List all of the Star Trek movies, include the **id**, **title** and **yr** (all of these movies include the words Star Trek in the title). Order results by year.

```sql
SELECT id,
       title,
       yr
FROM   movie
WHERE  title LIKE '%Star Trek%'
ORDER  BY yr;
```

4. What **id** number does the actor 'Glenn Close' have?

```sql
SELECT id
FROM   actor
WHERE  name = 'Glenn Close';
```

5. What is the **id** of the film 'Casablanca'

```sql
SELECT id
FROM   movie
WHERE  title = 'Casablanca'; 
```

6. Obtain the cast list for 'Casablanca'.

```sql
SELECT name
FROM   casting
       JOIN actor
         ON ( actorid = id )
WHERE  movieid = 27;
```

7. Obtain the cast list for the film 'Alien'

```sql
SELECT name
FROM   casting
       JOIN actor
         ON ( actorid = id )
WHERE  movieid = (SELECT id
                  FROM   movie
                  WHERE  title = 'Alien');
```

8. List the films in which 'Harrison Ford' has appeared

```sql
SELECT title
FROM   movie
       JOIN casting
         ON ( id = movieid )
WHERE  actorid = (SELECT id
                  FROM   actor
                  WHERE  name = 'Harrison Ford'); 
```

9. List the films where 'Harrison Ford' has appeared - but not in the starring role.

```sql
SELECT title
FROM   movie
       JOIN casting
         ON ( id = movieid )
WHERE  actorid = (SELECT id
                  FROM   actor
                  WHERE  name = 'Harrison Ford')
       AND ord <> 1;
```

10. List the films together with the leading star for all 1962 films.

```sql
SELECT title,
       (SELECT name
        FROM   actor
        WHERE  id = actorid)
FROM   movie
       JOIN casting
         ON ( id = movieid )
WHERE  ord = 1
       AND yr = 1962;
```

11. Which were the busiest years for 'Rock Hudson', show the year and the number of movies he made each year for any year in which he made more than 2 movies.

```sql
SELECT yr,
       COUNT(title)
FROM   movie
       JOIN casting
         ON movie.id = movieid
       JOIN actor
         ON actorid = actor.id
WHERE  name = 'Rock Hudson'
GROUP  BY yr
HAVING COUNT(title) > 2;
```

12. List the film title and the leading actor for all of the films 'Julie Andrews' played in.

```sql
SELECT title,
       (SELECT name
        FROM   actor a
        WHERE  actorid = a.id)
FROM   movie m
       JOIN casting
         ON ( m.id = movieid )
WHERE  ord = 1
       AND movieid IN (SELECT movieid
                       FROM   casting
                       WHERE  actorid = (SELECT id
                                         FROM   actor
                                         WHERE  name = 'Julie Andrews'));
```

13. Obtain a list, in alphabetical order, of actors who've had at least 15 **starring** roles.

```sql
SELECT name
FROM   actor
       JOIN casting
         ON ( id = actorid )
WHERE  ord = 1
GROUP  BY name
HAVING COUNT(ord) >= 15
ORDER  BY name; 
```

14. List the films released in the year 1978 ordered by the number of actors in the cast, then by title.

```sql
SELECT title,
       COUNT(actorid)
FROM   movie
       JOIN casting
         ON ( id = movieid )
WHERE  yr = 1978
GROUP  BY title
ORDER  BY COUNT(actorid) DESC,
          title; 
```

15. List all the people who have worked with 'Art Garfunkel'.

```sql
SELECT name
FROM   actor
       JOIN casting
         ON ( id = actorid )
WHERE  movieid IN (SELECT movieid
                   FROM   casting
                   WHERE  actorid = (SELECT id
                                     FROM   actor
                                     WHERE  name = 'Art Garfunkel'))
       AND name <> 'Art Garfunkel'; 
```

1. How many stops are in the database.

```sql
SELECT COUNT(*)
FROM   stops; 
```

2. Find the id value for the stop 'Craiglockhart'

```sql
SELECT id
FROM   stops
WHERE  name = 'Craiglockhart'; 
```

3. Give the id and the name for the stops on the '4' 'LRT' service.

```sql
SELECT id,
       name
FROM   stops
       JOIN route
         ON ( id = stop )
WHERE  num = '4'
       AND company = 'LRT';
```

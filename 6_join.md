1. The first example shows the goal scored by a player with the last name 'Bender'. The `*` says to list all the columns in the table - a shorter way of saying `matchid, teamid, player, gtime`

**Modify it to show the matchid and player name for all goals scored by Germany. To identify German players, check for: `teamid = 'GER'`**

```sql
SELECT matchid,
       player
FROM   goal
WHERE  teamid = 'GER';
```

2. From the previous query you can see that Lars Bender's scored a goal in game 1012. Now we want to know what teams were playing in that match.

Notice in the that the column `matchid` in the `goal` table corresponds to the `id` column in the `game` table. We can look up information about game 1012 by finding that row in the **game** table.

**Show id, stadium, team1, team2 for just game 1012**

```sql
SELECT id,
       stadium,
       team1,
       team2
FROM   game
WHERE  id = 1012;
```

3. You can combine the two steps into a single query with a `JOIN`.

```
SELECT *
  FROM game JOIN goal ON (id=matchid)
```

The **FROM** clause says to merge data from the goal table with that from the game table. The **ON** says how to figure out which rows in **game** go with which rows in **goal** - the **matchid** from **goal** must match **id** from **game**. (If we wanted to be more clear/specific we could say `ON (game.id=goal.matchid)`

The code below shows the player (from the goal) and stadium name (from the game table) for every goal scored.

**Modify it to show the player, teamid, stadium and mdate for every German goal.**

```sql
SELECT player,
       teamid,
       stadium,
       mdate
FROM   game
       JOIN goal
         ON ( id = matchid )
WHERE  teamid = 'GER';
```

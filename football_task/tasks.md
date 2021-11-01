# Football Matches - Tasks

Each of the questions/tasks below can be answered using a `SELECT` query. When you find a solution copy it into the code block under the question before pushing your solution to GitHub.

1) Find all the matches from 2017.

```sql

SELECT * FROM matches WHERE season = 2017;

```

2) Find all the matches featuring Barcelona.

```sql

SELECT * FROM matches WHERE hometeam = 'Barcelona' OR awayteam = 'Barcelona';

```

3) What are the names of the Scottish divisions included?

```sql

SELECT * FROM divisions  WHERE name LIKE '%Scottish%'; (mine)
SELECT name FROM divisions WHERE country = 'Scotland'; (better)

```

4) Find the division code for the Bundesliga. Use that code to find out how many matches Freiburg have played in the Bundesliga since the data started being collected.

```sql

SELECT * FROM divisions WHERE name = 'Bundesliga';
SELECT COUNT(*) FROM matches WHERE (division_code, awayteam) = ('D1', 'Freiburg'); (187)
SELECT COUNT(*) FROM matches WHERE (division_code, hometeam) = ('D1', 'Freiburg'); (187)

USING AND/OR:  
SELECT code FROM divisions WHERE name = 'Bundesliga';
SELECT COUNT(*) FROM matches WHERE division_code = 'D1' AND (hometeam = 'Freiburg' or awayteam = 'Freiburg'); (374)

```

5) Find the unique names of the teams which include the word "City" in their name (as entered in the database)

```sql
SELECT * FROM matches WHERE hometeam LIKE '%City%'; <- forgot the distinct! 
SELECT DISTINCT hometeam FROM matches WHERE hometeam LIKE '%City%';

```

6) How many different teams have played in matches recorded in a French division?

```sql
SELECT * FROM divisions WHERE country = 'France';
// find matches with division_code F1 or F2 
SELECT * FROM matches WHERE division_code = 'F1' OR division_code = 'F2';
// count number of matches with french division code
SELECT COUNT(*) FROM matches GROUP BY division_code = 'F1' OR division_code = 'F2';
or 
// count number of hometeam entries where the division is french 
SELECT COUNT(hometeam) FROM matches WHERE division_code = 'F1' OR division_code = 'F2'; (11959)

// to get the number of different teams, not every match played in either league 
SELECT code FROM divisions WHERE country = 'France';
SELECT COUNT(DISTINCT hometeam) From matches WHERE division_code = 'F1' OR division_code = 'F2'; (61)

IN keyword: if checking for OR in same property:
SELECT COUNT(DISTINCT hometeam) From matches WHERE division_code IN ('F1', 'F2');

```

7) Have Huddersfield played Swansea in the period covered?

```sql
 SELECT * FROM matches WHERE (hometeam, awayteam) = ('Huddersfield', 'Swansea') or (hometeam, awayteam) = ('Swansea', 'Huddersfield');
yes 
 SELECT COUNT(*) FROM matches WHERE (hometeam, awayteam) = ('Huddersfield', 'Swansea') or (hometeam, awayteam) = ('Swansea', 'Huddersfield');
12 times 
```

8) How many draws were there in the Eredivisie between 2010 and 2015?

```sql
SELECT COUNT(*) FROM matches WHERE fthg = ftag;
(32794)
// eredivisie division_code = n1
SELECT code FROM divisions WHERE name = 'Eredivisie'; 
SELECT COUNT(*) FROM matches WHERE fthg = ftag AND division_code = 'N1';
(1114)

// between 2010 and 2015! and the ftr column does result lol 
SELECT COUNT(*) FROM matches WHERE ftr = 'D' AND division_code = 'N1' AND season BETWEEN 2010 AND 2015;
(431)
```

9) Select the matches played in the Premier League in order of total goals scored from highest to lowest. Where there is a tie the match with more home goals should come first.

```sql
Premier League div_code = 'E0'

// this orders all premier league matches by number of goals, highest first 
SELECT * FROM matches WHERE division_code = 'E0' ORDER BY fthg + ftag DESC; 

// where tie, match with more home goals comes first
SELECT * FROM matches WHERE division_code = 'E0' ORDER BY fthg + ftag DESC; 

solution: 
SELECT * FROM matches WHERE division_code = 'E0' ORDER BY (fthg + ftag, fthg) DESC;
can give ODRDER BY multiple arguments - will order by first one and if tie, order by the second one (the argument after the comma)
```

10) In which division and which season were the most goals scored?

```sql

SELECT division_code, season, SUM(fthg + ftag) FROM matches GROUP BY division_code, season ORDER BY SUM DESC;
SELECT name FROM divisions WHERE code = 'EC';
(national league)

LIMIT 1 at the end = limit how many results it returns to you! 
```

### Useful Resources

- [Filtering results](https://www.w3schools.com/sql/sql_where.asp)
- [Ordering results](https://www.w3schools.com/sql/sql_orderby.asp)
- [Grouping results](https://www.w3schools.com/sql/sql_groupby.asp)
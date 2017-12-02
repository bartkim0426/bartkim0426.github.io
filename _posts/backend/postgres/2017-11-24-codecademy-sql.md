---
layout: post
title: "Basic of sql - codecademy"
categories: postgres
tags: postgres sql codecademy
---


## Manuplation

### `CREATE TABLE`
```sql
CREATE TABLE tablename;
```

### `INSERT INTO`
```sql
INSERT INTO tablename (id, name, age)
VALUES (1, "seul", 26);
```

### `SELECT`
```sql
SELECT * FROM tablename;
SELECT id, name FROM tablename;
```

### `UPDATE`
```sql
UPDATE tablename
SET age = 27
WHERE id = 1;
```

### `ALTER`
```sql
ALTER TABLE tablename 
ADD COLUMN email TEXT;
```

### `DELETE`
```sql
DELETE FROM tablename
WHERE email IS NULL;
```


## Queries

### `SELECT`
```sql
SELECT name, imdb_rating FROM movies;
SELECT DISTINCT genre FROM movies;
```

**WHERE**   

```sql
SELECT * FROM movies WHERE imdb_rating > 8;
```


**LIKE**   
비슷한 value를 찾아낼 때 유용
```sql
# using _
SELECT * FROM movies 
WHERE name LIKE "se_en";
# result is
# seven
# se7en

# using %
# start from a
SELECT * FROM movies 
WHERE name LIKE "a%";
# including man
SELECT * FROM movies
WHERE name LIKE "%man%";

# using BETWEEN
SELECT * FROM movies
WHERE name BETWEEN "A" AND "J";
SELECT * FROM movies
WHERE year BETWEEN 1990 AND 2000;
```

**AND**     
여러 query들을 묶을 수 있음
```sql
SELECT * FROM movies
WHERE year BETWEEN 1990 AND 2000
AND genre = "comedy";
```

**OR**    
```sql
SELECT * FROM movies
WHERE genre = "comedy"
OR year < 1980;
```

### `ORDER`
```sql
# ordering by desc
SELECT * FROM movies
ORDER BY imdb_rating DESC;

# ordernig by asc and get only 3
SELECT * FROM movies
ORDER BY imdb_rating ASC
LIMIT 3;
```


## Aggregate

### `COUNT`
```sql
# 가격이 0인 앱들 카운팅
SELECT COUNT(*) FROM fake apps
WHERE price = 0;
```

### `GROUP BY`
```
# 가격별로 카운팅
SELECT price, COUNT(*) FROM fake_apps
GROUP BY price;

SELECT price, COUNT(*) FROM fake_apps
WHERE downloads > 20000
GROUP BY price;
```

### `SUM`
```sql
SELECT SUM(downloads) FROM fake_apps;

SELECT category, SUM(downloads) FROM fake_apps
GROUP BY category;
```

### `MAX`
```sql
SELECT name, category, MAX(downloads) FROM fake_apps
GROUP BY category;
```

### `MIN`
```sql
SELECT name, category, MIN(downloads)
FROM fake_apps
GROUP BY category;
```

###  `AVG`
```sql
SELECT price, AVG(downloads)
FROM fake_apps
GROUP BY price;
```

### `ROUND`
정수화시키기
```sql
SELECT price, ROUND(AVG(downloads))
FROM fake_apps
GROUP BY price;
```


## Multiple tables

### select by other table
```sql
SELECT * FROM albums
WHERE artist_id = 3;
```

### Cross join
```sql
SELECT artists.name, albums.name
FROM artists, albums
```

### Inner join
```sql
SELECT * 
FROM 
	albums
JOIN artist ON
	albums.artist_id = artist.id;
```
### Left outer join
Inner join과 다르게 join condition이 충족되지 않아도 괜찮음
```sql
SELECT
	*
FROM
	albums
LEFT JOIN artists ON
	albums.artist_id = artists.id;
```

### Alias
```sql
SELECT
	albums.name AS "Album",
	albums.year,
	artists.name AS "Artist"
FROM
	albums
JOIN artists ON
	albums.artist_id = artist.id;
WHERE
	albums.year > 1980;
```

### create table safely with Foreign Key constraint
```sql
DROP TABLE IF EXISTS albums;
CREATE TABLE IF NOT EXISTS albums(
  id INTEGER PRIMARY KEY, 
  name TEXT,
  year INTEGER,
  artist_id INTEGER,
  FOREIGN KEY(artist_id) REFERENCES artist(id)
);
```

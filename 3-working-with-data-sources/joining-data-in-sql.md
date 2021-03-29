1. Introducing Joins
```SQL
SELECT *
  FROM facts JOIN cities
    ON facts.id = cities.facts_id
 LIMIT 10;
```

2. Understanding Inner Joins
```SQL
SELECT c.*, f.name AS country_name 
  FROM cities AS c 
 INNER JOIN facts AS f ON c.facts_id = f.id
 LIMIT 5;
```

3. Practicing Inner Joins
```SQL

```

4. Left Joins
```SQL

```

5. Finding the Most Populous Capital Cities
```SQL

```

6. Combining Joins with Subqueries
```SQL

```

7. Challenge: Complex Query with Joins and Subqueries
```SQL

```
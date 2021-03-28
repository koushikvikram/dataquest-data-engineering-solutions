1. A Simple Question
```SQL
SELECT MIN(Unemployment_rate)
  FROM recent_grads;
```

2. Aggregate Functions
```SQL
SELECT SUM(Total) 
  FROM recent_grads;
```

3. Order of Execution
```SQL
SELECT COUNT(Major) 
  FROM recent_grads 
 WHERE ShareWomen<0.5;
```

4. Missing Values
```SQL
SELECT COUNT(*), COUNT(Unemployment_rate)
  FROM recent_grads;
```

5. Combining Multiple Aggregation Functions
```SQL
SELECT AVG(Total), MIN(Men), MAX(Women) 
  FROM recent_grads;
```

6. Customizing the Results
```SQL
SELECT COUNT(*) AS "Number of Majors", MAX(Unemployment_rate) AS "Highest Unemployment Rate" 
  FROM recent_grads;
```

7. Counting Unique Values
```SQL
SELECT COUNT(DISTINCT(Major)) unique_majors, COUNT(DISTINCT(Major_category)) unique_major_categories, COUNT(DISTINCT(Major_code)) unique_major_codes 
  FROM recent_grads;
```

8. String Functions and Operations 
```SQL
SELECT "Major: " || LOWER(Major) AS Major,
       Total, Men, Women, Unemployment_rate,
       LENGTH(Major) AS Length_of_name
  FROM recent_grads
 ORDER BY Unemployment_rate DESC;
```

9. Performing Arithmetic in SQL
```SQL
SELECT Major, Major_category, P75th - P25th quartile_spread 
  FROM recent_grads 
 ORDER BY quartile_spread 
 LIMIT 20;
```
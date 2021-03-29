1. Writing More Complex Queries
```SQL
SELECT Major, ShareWomen
  FROM recent_grads
 WHERE ShareWomen>(SELECT AVG(ShareWomen)
                   FROM recent_grads);
```

2. Subqueries
```SQL
SELECT Major, Unemployment_rate 
  FROM recent_grads 
 WHERE Unemployment_rate < (SELECT AVG(Unemployment_rate) 
                            FROM recent_grads);
```

3. Subquery in SELECT
```SQL
SELECT (SELECT COUNT(*) 
          FROM recent_grads
         WHERE ShareWomen> (SELECT AVG(ShareWomen)
                              FROM recent_grads))*1.0/ 
       (SELECT COUNT(*) 
          FROM recent_grads) AS proportion_abv_avg;
```

4. The IN Operator
```SQL
SELECT Major_category, Major
  FROM recent_grads
 WHERE Major_category IN ('Business', 'Humanities & Liberal Arts', 'Education');
```

5. Returning Multiple Results in Subqueries
```SQL
SELECT Major_category, Major
  FROM recent_grads
 WHERE Major_category IN (SELECT Major_category
                            FROM recent_grads
                           GROUP BY Major_category
                           ORDER BY SUM(TOTAL) DESC
                           LIMIT 3);
```

6. Building Complex Subqueries
```SQL
SELECT AVG(Sample_size*1.0/Total) AS avg_ratio
  FROM recent_grads;
```

7. Practice Integrating A Subquery With The Outer Query
```SQL
SELECT Major, Major_category, CAST(Sample_size AS FLOAT)/CAST(Total AS FLOAT) AS ratio 
  FROM recent_grads 
 WHERE ratio > (SELECT AVG(CAST(Sample_size AS FLOAT)/CAST(Total AS FLOAT)) AS avg_ratio    
                  FROM recent_grads);
```
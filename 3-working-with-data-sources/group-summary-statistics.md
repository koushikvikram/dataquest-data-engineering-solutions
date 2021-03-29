1. If/Then in SQL
```SQL
SELECT (CASE
        WHEN Sample_size<200 THEN 'Small'
        WHEN Sample_size>=200 AND Sample_size<1000 THEN 'Medium'
        WHEN Sample_size>=1000 THEN 'Large' END) AS Sample_category
       FROM recent_grads;
```

2. Dissecting CASE
```SQL
SELECT Major, Sample_size,
       (CASE
        WHEN Sample_size<200 THEN 'Small'
        WHEN Sample_size>=200 AND Sample_size<1000 THEN 'Medium'
        WHEN Sample_size>=1000 THEN 'Large' END) AS Sample_category
       FROM recent_grads;
```

3. Calculating Group-Level Summary Statistics
```SQL
SELECT Major_category, SUM(Total) AS Total_graduates
  FROM recent_grads
 GROUP BY Major_category;
```

4. GROUP BY Visuual Breakdown
```SQL
SELECT Major_category, AVG(ShareWomen) AS Average_women
  FROM recent_grads 
 GROUP BY Major_category;
```

5. Multiple Summary Statistics by Group
```SQL
SELECT Major_category, SUM(Women) AS Total_women, AVG(ShareWomen) AS Mean_women, SUM(Total)*AVG(ShareWomen) AS Estimate_women 
  FROM recent_grads
 GROUP BY Major_category;
```

6. Multiple Group Columns
```SQL
SELECT Major_category, Sample_category, AVG(ShareWomen) AS Mean_women, SUM(Total) AS Total_graduates 
  FROM new_grads
 GROUP BY Major_category, Sample_category;
```

7. Querying Virtual Columns With the HAVING Statement 
```SQL
SELECT Major_category, AVG(Low_wage_jobs)/AVG(Total) AS Share_low_wage 
  FROM new_grads 
 GROUP BY Major_category 
HAVING share_low_wage>0.1;
```

8. Rounding Results With the ROUND() Function
```SQL
SELECT ROUND(ShareWomen, 4) AS Rounded_women, Major_category 
  FROM recent_grads 
 LIMIT 10; 
```

9. Nesting functions
```SQL
SELECT Major_category, ROUND(AVG(College_jobs)/AVG(Total), 3) AS Share_degree_jobs 
  FROM recent_grads 
 GROUP BY Major_category 
HAVING share_degree_jobs<0.3;
```

10. Casting
```SQL
SELECT Major_category, CAST(SUM(Women) AS Float)/CAST(SUM(Total) AS Float) AS SW       
  FROM recent_grads 
 GROUP BY Major_category 
 ORDER BY SW;
```
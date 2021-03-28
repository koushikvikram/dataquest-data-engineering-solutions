1. Your First Query
```SQL
SELECT *
  FROM recent_grads;
```

2. Understanding Your First Query
```SQL
SELECT * 
  FROM recent_grads;

select *
  from recent_grads;
```

3. The LIMIT Clause
```SQL
SELECT *
  FROM recent_grads
 LIMIT 5;
```

4. Selecting Specific Columns
```SQL
SELECT Major, ShareWomen
  FROM recent_grads;
```

5. Filtering Rows Using 'WHERE' 
```SQL
SELECT major, sharewomen 
  FROM recent_grads
 WHERE sharewomen<0.5;
```

6. Expressing Multiple Filter Criteria Using 'AND'
```SQL
SELECT major, major_category, median, sharewomen 
  FROM recent_grads 
 WHERE sharewomen>0.5 AND median>50000;
```

7. Returning One of Several Conditions With 'OR'
```SQL
SELECT major, median, unemployed 
  FROM recent_grads 
 WHERE median>=10000 OR sharewomen<0.5
 LIMIT 20;
```

8. Grouping Operators with Parentheses
```SQL
SELECT major, major_category, sharewomen, unemployment_rate
  FROM recent_grads
 WHERE major_category='Engineering' AND (sharewomen>0.5 OR unemployment_rate<0.051);
```

9. Ordering Results Using 'ORDER BY'
```SQL
SELECT major, sharewomen, unemployment_rate 
  FROM recent_grads 
 WHERE sharewomen>0.3 AND unemployment_rate<.1
 ORDER BY sharewomen DESC;
```

10. Practice Writing a Query
```SQL
SELECT major_category, major, unemployment_rate
  FROM recent_grads
 WHERE major_category IN ('Engineering', 'Physical Sciences')
 ORDER BY unemployment_rate;
```
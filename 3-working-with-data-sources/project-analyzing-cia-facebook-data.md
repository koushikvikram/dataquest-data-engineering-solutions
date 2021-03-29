
# Analyzing CIA Factbook Data Using SQL


```python
%%capture
%load_ext sql
%sql sqlite:///factbook.db
```




    'Connected: None@factbook.db'



## Overview of the data


```python
%%sql
SELECT *
  FROM facts
 LIMIT 5;
```

    Done.





<table>
    <tr>
        <th>id</th>
        <th>code</th>
        <th>name</th>
        <th>area</th>
        <th>area_land</th>
        <th>area_water</th>
        <th>population</th>
        <th>population_growth</th>
        <th>birth_rate</th>
        <th>death_rate</th>
        <th>migration_rate</th>
    </tr>
    <tr>
        <td>1</td>
        <td>af</td>
        <td>Afghanistan</td>
        <td>652230</td>
        <td>652230</td>
        <td>0</td>
        <td>32564342</td>
        <td>2.32</td>
        <td>38.57</td>
        <td>13.89</td>
        <td>1.51</td>
    </tr>
    <tr>
        <td>2</td>
        <td>al</td>
        <td>Albania</td>
        <td>28748</td>
        <td>27398</td>
        <td>1350</td>
        <td>3029278</td>
        <td>0.3</td>
        <td>12.92</td>
        <td>6.58</td>
        <td>3.3</td>
    </tr>
    <tr>
        <td>3</td>
        <td>ag</td>
        <td>Algeria</td>
        <td>2381741</td>
        <td>2381741</td>
        <td>0</td>
        <td>39542166</td>
        <td>1.84</td>
        <td>23.67</td>
        <td>4.31</td>
        <td>0.92</td>
    </tr>
    <tr>
        <td>4</td>
        <td>an</td>
        <td>Andorra</td>
        <td>468</td>
        <td>468</td>
        <td>0</td>
        <td>85580</td>
        <td>0.12</td>
        <td>8.13</td>
        <td>6.96</td>
        <td>0.0</td>
    </tr>
    <tr>
        <td>5</td>
        <td>ao</td>
        <td>Angola</td>
        <td>1246700</td>
        <td>1246700</td>
        <td>0</td>
        <td>19625353</td>
        <td>2.78</td>
        <td>38.78</td>
        <td>11.49</td>
        <td>0.46</td>
    </tr>
</table>



## Summary Statistics


```python
%%sql
SELECT MIN(population) AS min_population, MAX(population) AS max_population, MIN(population_growth) AS min_population_growth, MAX(population_growth) AS max_population_growth
  FROM facts;
```

    Done.





<table>
    <tr>
        <th>min_population</th>
        <th>max_population</th>
        <th>min_population_growth</th>
        <th>max_population_growth</th>
    </tr>
    <tr>
        <td>0</td>
        <td>7256490011</td>
        <td>0.0</td>
        <td>4.02</td>
    </tr>
</table>



## Exploring Outliers


```python
%%sql
SELECT *
  FROM facts
 WHERE population = (SELECT MIN(population)
                       FROM facts);
```

    Done.





<table>
    <tr>
        <th>id</th>
        <th>code</th>
        <th>name</th>
        <th>area</th>
        <th>area_land</th>
        <th>area_water</th>
        <th>population</th>
        <th>population_growth</th>
        <th>birth_rate</th>
        <th>death_rate</th>
        <th>migration_rate</th>
    </tr>
    <tr>
        <td>250</td>
        <td>ay</td>
        <td>Antarctica</td>
        <td>None</td>
        <td>280000</td>
        <td>None</td>
        <td>0</td>
        <td>None</td>
        <td>None</td>
        <td>None</td>
        <td>None</td>
    </tr>
</table>




```python
%%sql
SELECT *
  FROM facts
 WHERE population = (SELECT MAX(population)
                       FROM facts);
```

    Done.





<table>
    <tr>
        <th>id</th>
        <th>code</th>
        <th>name</th>
        <th>area</th>
        <th>area_land</th>
        <th>area_water</th>
        <th>population</th>
        <th>population_growth</th>
        <th>birth_rate</th>
        <th>death_rate</th>
        <th>migration_rate</th>
    </tr>
    <tr>
        <td>261</td>
        <td>xx</td>
        <td>World</td>
        <td>None</td>
        <td>None</td>
        <td>None</td>
        <td>7256490011</td>
        <td>1.08</td>
        <td>18.6</td>
        <td>7.8</td>
        <td>None</td>
    </tr>
</table>




```python
%%sql
SELECT MIN(population) AS min_population, MAX(population) AS max_population, MIN(population_growth) AS min_population_growth, MAX(population_growth) AS max_population_growth
  FROM facts
 WHERE name <> 'World';
```

    Done.





<table>
    <tr>
        <th>min_population</th>
        <th>max_population</th>
        <th>min_population_growth</th>
        <th>max_population_growth</th>
    </tr>
    <tr>
        <td>0</td>
        <td>1367485388</td>
        <td>0.0</td>
        <td>4.02</td>
    </tr>
</table>



## Exploring Average Population and Area


```python
%%sql
SELECT AVG(population) AS avg_population, AVG(area) AS avg_area
  FROM facts
 WHERE name <> 'World';
```

    Done.





<table>
    <tr>
        <th>avg_population</th>
        <th>avg_area</th>
    </tr>
    <tr>
        <td>32242666.56846473</td>
        <td>555093.546184739</td>
    </tr>
</table>



## Finding Densely Populated Countries


```python
%%sql
SELECT *
  FROM facts
 WHERE population > (SELECT AVG(population)
                       FROM facts
                      WHERE name <> 'World') AND
             area < (SELECT AVG(area)
                       FROM facts
                      WHERE name <> 'World');
```

    Done.





<table>
    <tr>
        <th>id</th>
        <th>code</th>
        <th>name</th>
        <th>area</th>
        <th>area_land</th>
        <th>area_water</th>
        <th>population</th>
        <th>population_growth</th>
        <th>birth_rate</th>
        <th>death_rate</th>
        <th>migration_rate</th>
    </tr>
    <tr>
        <td>14</td>
        <td>bg</td>
        <td>Bangladesh</td>
        <td>148460</td>
        <td>130170</td>
        <td>18290</td>
        <td>168957745</td>
        <td>1.6</td>
        <td>21.14</td>
        <td>5.61</td>
        <td>0.46</td>
    </tr>
    <tr>
        <td>65</td>
        <td>gm</td>
        <td>Germany</td>
        <td>357022</td>
        <td>348672</td>
        <td>8350</td>
        <td>80854408</td>
        <td>0.17</td>
        <td>8.47</td>
        <td>11.42</td>
        <td>1.24</td>
    </tr>
    <tr>
        <td>80</td>
        <td>iz</td>
        <td>Iraq</td>
        <td>438317</td>
        <td>437367</td>
        <td>950</td>
        <td>37056169</td>
        <td>2.93</td>
        <td>31.45</td>
        <td>3.77</td>
        <td>1.62</td>
    </tr>
    <tr>
        <td>83</td>
        <td>it</td>
        <td>Italy</td>
        <td>301340</td>
        <td>294140</td>
        <td>7200</td>
        <td>61855120</td>
        <td>0.27</td>
        <td>8.74</td>
        <td>10.19</td>
        <td>4.1</td>
    </tr>
    <tr>
        <td>85</td>
        <td>ja</td>
        <td>Japan</td>
        <td>377915</td>
        <td>364485</td>
        <td>13430</td>
        <td>126919659</td>
        <td>0.16</td>
        <td>7.93</td>
        <td>9.51</td>
        <td>0.0</td>
    </tr>
    <tr>
        <td>91</td>
        <td>ks</td>
        <td>Korea, South</td>
        <td>99720</td>
        <td>96920</td>
        <td>2800</td>
        <td>49115196</td>
        <td>0.14</td>
        <td>8.19</td>
        <td>6.75</td>
        <td>0.0</td>
    </tr>
    <tr>
        <td>120</td>
        <td>mo</td>
        <td>Morocco</td>
        <td>446550</td>
        <td>446300</td>
        <td>250</td>
        <td>33322699</td>
        <td>1.0</td>
        <td>18.2</td>
        <td>4.81</td>
        <td>3.36</td>
    </tr>
    <tr>
        <td>138</td>
        <td>rp</td>
        <td>Philippines</td>
        <td>300000</td>
        <td>298170</td>
        <td>1830</td>
        <td>100998376</td>
        <td>1.61</td>
        <td>24.27</td>
        <td>6.11</td>
        <td>2.09</td>
    </tr>
    <tr>
        <td>139</td>
        <td>pl</td>
        <td>Poland</td>
        <td>312685</td>
        <td>304255</td>
        <td>8430</td>
        <td>38562189</td>
        <td>0.09</td>
        <td>9.74</td>
        <td>10.19</td>
        <td>0.46</td>
    </tr>
    <tr>
        <td>163</td>
        <td>sp</td>
        <td>Spain</td>
        <td>505370</td>
        <td>498980</td>
        <td>6390</td>
        <td>48146134</td>
        <td>0.89</td>
        <td>9.64</td>
        <td>9.04</td>
        <td>8.31</td>
    </tr>
    <tr>
        <td>173</td>
        <td>th</td>
        <td>Thailand</td>
        <td>513120</td>
        <td>510890</td>
        <td>2230</td>
        <td>67976405</td>
        <td>0.34</td>
        <td>11.19</td>
        <td>7.8</td>
        <td>0.0</td>
    </tr>
    <tr>
        <td>182</td>
        <td>ug</td>
        <td>Uganda</td>
        <td>241038</td>
        <td>197100</td>
        <td>43938</td>
        <td>37101745</td>
        <td>3.24</td>
        <td>43.79</td>
        <td>10.69</td>
        <td>0.74</td>
    </tr>
    <tr>
        <td>185</td>
        <td>uk</td>
        <td>United Kingdom</td>
        <td>243610</td>
        <td>241930</td>
        <td>1680</td>
        <td>64088222</td>
        <td>0.54</td>
        <td>12.17</td>
        <td>9.35</td>
        <td>2.54</td>
    </tr>
    <tr>
        <td>192</td>
        <td>vm</td>
        <td>Vietnam</td>
        <td>331210</td>
        <td>310070</td>
        <td>21140</td>
        <td>94348835</td>
        <td>0.97</td>
        <td>15.96</td>
        <td>5.93</td>
        <td>0.3</td>
    </tr>
</table>




```python

```


```python

```


```python

```

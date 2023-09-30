# Weather Observation Station 6
Query the list of CITY names starting with vowels (i.e., a, e, i, o, or u) from STATION. Your result cannot contain duplicates.

---------------------------------------
**Solution**
```mysql
SELECT DISTINCT(city) 
FROM station 
WHERE city LIKE 'a%' OR city LIKE 'E%' OR city LIKE 'I%' OR city LIKE 'O%' OR city LIKE 'U%'
```

# Weather Observation Station 12
Query the list of CITY names from STATION that do not start with vowels and do not end with vowels. Your result cannot contain duplicates.  

---------------------------------------
**Solution**
```mysql
SELECT DISTINCT(city) 
FROM STATION
WHERE LOWER(SUBSTR(CITY,1,1)) NOT IN ('a','e','i','o','u') AND LOWER(SUBSTR(CITY,LENGTH(CITY),1)) NOT IN ('a','e','i','o','u');   
```

# Contest Leaderboard  
The total score of a hacker is the sum of their maximum scores for all of the challenges. Write a query to print the hacker_id, name, and total score of the hackers ordered by the descending score. If more than one hacker achieved the same total score, then sort the result by ascending hacker_id. Exclude all hackers with a total score of  from your result.

Sample Input

Hackers Table:   
![image](https://github.com/YaoSheng-Yu/HackerRank-SQL-practice/assets/144596901/4711d68c-6878-487a-a32e-4eb3112b6512)  


Submissions Table:   
![image](https://github.com/YaoSheng-Yu/HackerRank-SQL-practice/assets/144596901/6eb00e34-b398-4085-a3fc-6af9c42b0815)  

Sample Output:  

4071 Rose 191

74842 Lisa 174  

84072 Bonnie 100  

---------------------------------------
**Solution**
```mysql
SELECT ts.hacker_id, h.name, ts.total
FROM (
    SELECT hacker_id, SUM(top_score) AS total
    FROM (
        SELECT hacker_id, challenge_id, MAX(score) AS top_score
        FROM Submissions
        GROUP BY hacker_id, challenge_id
    ) AS max_submission
    GROUP BY hacker_id
    HAVING SUM(top_score) > 0
) AS ts
JOIN Hackers h ON ts.hacker_id = h.hacker_id
ORDER BY ts.total DESC, ts.hacker_id ASC;
```

# Type of Triangle
Write a query identifying the type of each record in the TRIANGLES table using its three side lengths. Output one of the following statements for each record in the table:

Equilateral: It's a triangle with  sides of equal length.  
Isosceles: It's a triangle with  sides of equal length.  
Scalene: It's a triangle with  sides of differing lengths.  
Not A Triangle: The given values of A, B, and C don't form a triangle.  

Sample InputL:  
![image](https://github.com/YaoSheng-Yu/HackerRank-SQL-practice/assets/144596901/d8be910d-d0f0-4ef0-b00a-6026832382d4)  

Sample Output:  
Isosceles  
Equilateral  
Scalene  
Not A Triangle  

---------------------------------------
**Solution**
```mysql
SELECT   
CASE   
    WHEN A + B <= C OR A + C <= B OR B + C <= A THEN 'Not A Triangle'  
    WHEN A = B AND B = C THEN 'Equilateral'  
    WHEN A = B OR B = C OR A = C THEN 'Isosceles'  
    ELSE 'Scalene'  
END  
FROM TRIANGLES;  
```

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

# Binary Tree Nodes  
You are given a table, BST, containing two columns: N and P, where N represents the value of a node in Binary Tree, and P is the parent of N.  

Write a query to find the node type of Binary Tree ordered by the value of the node. Output one of the following for each node:

Root: If node is root node.  
Leaf: If node is leaf node.  
Inner: If node is neither root nor leaf node.  

Sample Input:  
![image](https://github.com/YaoSheng-Yu/HackerRank-SQL-practice/assets/144596901/a56b06bc-53ac-4903-85f0-43836a54e822)  

Sample Output

1 Leaf  
2 Inner  
3 Leaf  
5 Root  
6 Leaf  
8 Inner  
9 Leaf  

---------------------------------------
**Solution**
```mysql
SELECT N,  
CASE  
    WHEN P IS NULL THEN 'Root'  
    WHEN N NOT IN (SELECT DISTINCT P FROM BST WHERE P IS NOT NULL) THEN 'Leaf'  
    ELSE 'Inner'  
END  
FROM BST  
ORDER BY N asc;  
```

# New Companies
Amber's conglomerate corporation just acquired some new companies. Each of the companies follows this hierarchy:   
Given the table schemas below, write a query to print the company_code, founder name, total number of lead managers, total number of senior managers, total number of managers, and total number of employees. Order your output by ascending company_code.  
![image](https://github.com/YaoSheng-Yu/HackerRank-SQL-practice/assets/144596901/e359f955-175a-4c41-9dea-0d9350a1d445)  

Note:  
The tables may contain duplicate records.  
The company_code is string, so the sorting should not be numeric. For example, if the company_codes are C_1, C_2, and C_10, then the ascending company_codes will be C_1, C_10, and C_2.  

Sample Input:  
Company Table:  
![image](https://github.com/YaoSheng-Yu/HackerRank-SQL-practice/assets/144596901/6c1b18c6-b35d-4487-8c68-a3c3cb456fca)  

Lead_Manager Table:  
![image](https://github.com/YaoSheng-Yu/HackerRank-SQL-practice/assets/144596901/8a3ce39c-3cbf-483f-bfe7-9489b7b0cca8)  

Senior_Manager Table:  
![image](https://github.com/YaoSheng-Yu/HackerRank-SQL-practice/assets/144596901/63513d15-1c9a-4835-8740-a8d179ca19a9)  

Manager Table:  
![image](https://github.com/YaoSheng-Yu/HackerRank-SQL-practice/assets/144596901/d59a3c43-ae1e-4cbb-92bf-79a84ec6c98f)  

Employee Table:  
![image](https://github.com/YaoSheng-Yu/HackerRank-SQL-practice/assets/144596901/252b49aa-563a-4292-b37f-0d628080e672)  

Sample Output  

C1 Monika 1 2 1 2  
C2 Samantha 1 1 2 2  

---------------------------------------
**Solution**
```mysql
SELECT   
    C.COMPANY_CODE,  
    C.founder,   
    COUNT(DISTINCT LM.LEAD_MANAGER_CODE),   
    COUNT(DISTINCT SM.SENIOR_MANAGER_CODE),   
    COUNT(DISTINCT M.MANAGER_CODE),   
    COUNT(DISTINCT E.EMPLOYEE_CODE)  
FROM COMPANY C  
LEFT JOIN LEAD_MANAGER LM ON LM.COMPANY_CODE = C.COMPANY_CODE  
LEFT JOIN SENIOR_MANAGER SM ON SM.COMPANY_CODE = C.COMPANY_CODE  
LEFT JOIN MANAGER M ON M.COMPANY_CODE = C.COMPANY_CODE  
LEFT JOIN EMPLOYEE E ON E.COMPANY_CODE = C.COMPANY_CODE  
GROUP BY C.COMPANY_CODE, C.founder  
ORDER BY C.COMPANY_CODE ASC;  
```
***Note*** When using GROUP BY in combination with aggregate functions, all the columns in the SELECT statement that aren't part of an aggregate function need to be included in the GROUP BY clause. This is to ensure determinism in the results. So you have to use GROUP BY C.COMPANY_CODE, C.founder rather than GROUP BY C.COMPANY_CODE

# The PADS
Generate the following two result sets:

Query an alphabetically ordered list of all names in OCCUPATIONS, immediately followed by the first letter of each profession as a parenthetical (i.e.: enclosed in parentheses). For example: AnActorName(A), ADoctorName(D), AProfessorName(P), and ASingerName(S).
Query the number of ocurrences of each occupation in OCCUPATIONS. Sort the occurrences in ascending order, and output them in the following format:

There are a total of [occupation_count] [occupation]s.
where [occupation_count] is the number of occurrences of an occupation in OCCUPATIONS and [occupation] is the lowercase occupation name. If more than one Occupation has the same [occupation_count], they should be ordered alphabetically.  

Sample Input  
![image](https://github.com/YaoSheng-Yu/HackerRank-SQL-practice/assets/144596901/60466814-0de0-4bed-b970-c91d093da98d)  

Sample Output

Ashely(P)  
Christeen(P)  
Jane(A)   
Jenny(D)  
Julia(A)  
Ketty(P)  
Maria(A)  
Meera(S)  
Priya(S)  
Samantha(D)  
There are a total of 2 doctors.  
There are a total of 2 singers.  
There are a total of 3 actors.  
There are a total of 3 professors.  

---------------------------------------
**Solution**
```mysql
SELECT output  
FROM (  
    SELECT CONCAT(Name, '(', SUBSTRING(Occupation, 1, 1), ')' ) AS output, 1 AS order_priority  
    FROM OCCUPATIONS   
    UNION ALL  
    SELECT CONCAT('There are a total of ', COUNT(Name), ' ', LOWER(Occupation), 's.' ), 2 AS order_priority  
    FROM OCCUPATIONS  
    GROUP BY Occupation  
) AS derived_table  
ORDER BY   
    order_priority,  
    output;  
```
***Note*** Adding an order_priority column to the derived table to help control the ordering. The first subquery's results have a priority of 1, and the second subquery's results have a priority of 2, ensuring that the individual names come before the counts.

# The Blunder

Samantha was tasked with calculating the average monthly salaries for all employees in the EMPLOYEES table, but did not realize her keyboard's  key was broken until after completing the calculation. She wants your help finding the difference between her miscalculation (using salaries with any zeros removed), and the actual average salary.

Write a query calculating the amount of error (i.e.: actual - miscalculated average monthly salaries), and round it up to the next integer.

Sample Input  
![image](https://github.com/YaoSheng-Yu/HackerRank-SQL-practice/assets/144596901/bd3007d6-79b1-475f-bc43-f629c842c65a)

Sample Output  
2061

![image](https://github.com/YaoSheng-Yu/HackerRank-SQL-practice/assets/144596901/d90cdd79-093e-4998-85b3-1b97818d48aa)

2159 - 98 = 2061  

---------------------------------------
**Solution**
```mysql
SELECT CEIL(AVG(salary) - AVG(CAST(REPLACE(CAST(salary AS CHAR), '0', '') AS UNSIGNED)))
FROM EMPLOYEES;
```

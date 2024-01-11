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

# The PADS (CHAR manipulation)
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

# The Blunder (char manipulation)

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

# Top Earners
We define an employee's total earnings to be their monthly salary * months  worked, and the maximum total earnings to be the maximum total earnings for any employee in the Employee table. Write a query to find the maximum total earnings for all employees as well as the total number of employees who have maximum total earnings. Then print these values as 2 space-separated integers.

Input Format  
![image](https://github.com/YaoSheng-Yu/HackerRank-SQL-practice/assets/144596901/7eeb13ce-845f-4a71-8b6a-8c548899c195)  

Sample Output

69952 1  

---------------------------------------
**Solution**
```mysql
SELECT CONCAT(MAX(earnings), '  ', COUNT(employee_id))
FROM (
    SELECT months * salary AS earnings, employee_id
    FROM Employee
) AS top_earnings
WHERE earnings = (
    SELECT MAX(months * salary)
    FROM Employee
);
```

# Weather Observation Station 15 
Query the Western Longitude (LONG_W) for the largest Northern Latitude (LAT_N) in STATION that is less than 137.2345. Round your answer to 4  decimal places.

---------------------------------------
**Solution**
```mysql
SELECT ROUND(LONG_W, 4)  
FROM STATION  
WHERE LAT_N = (  
    SELECT MAX(LAT_N)   
    FROM STATION   
    WHERE LAT_N < 137.2345  
);  
```

# Weather Observation Station 20 (median)
A median is defined as a number separating the higher half of a data set from the lower half. Query the median of the Northern Latitudes (LAT_N) from STATION and round your answer to 4 decimal places.  

---------------------------------------  
**Solution**  
```mysql  
SELECT round(AVG(LAT_N), 4)   
FROM (  
    SELECT LAT_N,  
           ROW_NUMBER() OVER (ORDER BY LAT_N) AS row_asc,  
           ROW_NUMBER() OVER (ORDER BY LAT_N DESC) AS row_desc  
    FROM STATION   
) AS subquery  
WHERE row_asc IN (row_desc, row_desc - 1, row_desc + 1);  
```  

# The Report （CASE WHEN）
Ketty gives Eve a task to generate a report containing three columns: Name, Grade and Mark. Ketty doesn't want the NAMES of those students who received a grade lower than 8. The report must be in descending order by grade -- i.e. higher grades are entered first. If there is more than one student with the same grade (8-10) assigned to them, order those particular students by their name alphabetically. Finally, if the grade is lower than 8, use "NULL" as their name and list them by their grades in descending order. If there is more than one student with the same grade (1-7) assigned to them, order those particular students by their marks in ascending order.

Sample input:  

![image](https://github.com/YaoSheng-Yu/HackerRank-SQL-practice/assets/144596901/650e3c56-5caf-4694-8944-7d509b08b2bd)  

![image](https://github.com/YaoSheng-Yu/HackerRank-SQL-practice/assets/144596901/db81d785-cc65-47a5-8ebd-ec72e0010501)  

Sample output:  
Maria 10 99  
Jane 9 81  
Julia 9 88   
Scarlet 8 78  
NULL 7 63  
NULL 7 68  

---------------------------------------  
**Solution**  
```mysql  
SELECT   
    CASE   
        WHEN g.grade >= 8 THEN s.name   
        ELSE NULL   
    END AS name,  
    g.grade,  
    s.marks  
FROM students s  
INNER JOIN Grades g   
    ON s.marks BETWEEN g.min_mark AND g.max_mark  
ORDER BY   
    g.grade DESC,  
    CASE  
        WHEN g.grade >= 8 THEN s.name  
    END ASC,  
    CASE  
        WHEN g.grade < 8 THEN s.marks  
    END ASC;  
```  

# Top Competitors  
Julia just finished conducting a coding contest, and she needs your help assembling the leaderboard! Write a query to print the respective hacker_id and name of hackers who achieved full scores for more than one challenge. Order your output in descending order by the total number of challenges in which the hacker earned a full score. If more than one hacker received full scores in same number of challenges, then sort them by ascending hacker_id.  

Sample Input

Hackers Table:  
![image](https://github.com/YaoSheng-Yu/HackerRank-SQL-practice/assets/144596901/07fc8c41-f0a9-46f0-b3ae-f892a6db8d0a)  

Difficulty Table:  
![image](https://github.com/YaoSheng-Yu/HackerRank-SQL-practice/assets/144596901/068b907c-721c-4c25-87cd-fb8d668852e1)  

Challenges Table:  
![image](https://github.com/YaoSheng-Yu/HackerRank-SQL-practice/assets/144596901/d380630f-b6bd-4cb8-a49e-62e993b41e85)  

Submissions Table:  
![image](https://github.com/YaoSheng-Yu/HackerRank-SQL-practice/assets/144596901/5f47061c-78e3-4f44-a74a-fb3038c465de)  

Sample Output

90411 Joe

---------------------------------------  
**Solution**  
```mysql  
SELECT h.hacker_id, h.name
FROM Submissions s
JOIN Challenges c ON s.challenge_id = c.challenge_id
JOIN Difficulty d ON d.difficulty_level = c.difficulty_level
JOIN Hackers h ON s.hacker_id = h.hacker_id
WHERE s.score = d.score
GROUP BY h.hacker_id, h.name
HAVING COUNT(s.challenge_id) > 1
ORDER BY COUNT(s.challenge_id) DESC, h.hacker_id; 
```  

# Ollivander's Inventory  
Harry Potter and his friends are at Ollivander's with Ron, finally replacing Charlie's old broken wand.

Hermione decides the best way to choose is by determining the **minimum number of gold galleons** needed to buy each non-evil wand of high power and age. Write a query to print the id, age, coins_needed, and power of the wands that Ron's interested in, sorted in order of descending power. If more than one wand has same power, sort the result in order of descending age. （least gold needed for one combination of age and power）

Sample Input  

Wands Table:  
![image](https://github.com/YaoSheng-Yu/HackerRank-SQL-practice/assets/144596901/1a069f05-5b8f-4273-b577-ffb534254a67)  

Wands_Property Table:  
![image](https://github.com/YaoSheng-Yu/HackerRank-SQL-practice/assets/144596901/a6b67537-b78d-4ac1-aed9-ac082aa23476)  


Sample Output

9 45 1647 10  
12 17 9897 10  
1 20 3688 8  
15 40 6018 7  
19 20 7651 6  
11 40 7587 5  
10 20 504 5  
18 40 3312 3  
20 17 5689 3  
5 45 6020 2  
14 40 5408 1  

---------------------------------------  
**Solution**  
```mysql  
SELECT id, age, coins_needed, power  
FROM (  
    SELECT   
        w.id,   
        p.age,   
        w.coins_needed,   
        w.power,  
        ROW_NUMBER() OVER(PARTITION BY w.power, p.age ORDER BY w.coins_needed ASC) as rnk  
    FROM Wands w  
    JOIN Wands_Property p ON w.code = p.code  
    WHERE p.is_evil = 0  
) as RankedWands  
WHERE rnk = 1  
ORDER BY power DESC, age DESC;   
SELECT id, age, coins_needed, power
```

**note**: PARTITION BY w.power, p.age: This breaks the data into partitions (or groups) based on the unique combinations of w.power and p.age. It's kinda similar to group by.  It's used with **window functions** to specify the set of rows over which the window function will operate. 

# Challenges
Julia asked her students to create some coding challenges. Write a query to print the hacker_id, name, and the total number of challenges created by each student. Sort your results by the total number of challenges in descending order. If more than one student created the same number of challenges, then sort the result by hacker_id. If more than one student created the same number of challenges and the count is less than the maximum number of challenges created, then exclude those students from the result.  

Sample Input  

Hackers Table:  
![image](https://github.com/YaoSheng-Yu/HackerRank-SQL-practice/assets/144596901/d18fc4fd-2dfd-413d-a8ff-4c60511ce99e)  


Challenges Table:  
![image](https://github.com/YaoSheng-Yu/HackerRank-SQL-practice/assets/144596901/c529d4fd-bbc2-4415-bcad-87169e696d8a)

Sample Output

21283 Angela 6  
88255 Patrick 5  
96196 Lisa 1  

---------------------------------------  
**Solution**  
```mysql  
WITH ChallengeCounts AS (
    SELECT hacker_id, COUNT(challenge_id) AS cnt
    FROM Challenges
    GROUP BY hacker_id
)

, MaxChallengeCount AS (
    SELECT MAX(cnt) AS max_cnt
    FROM ChallengeCounts
)

, DuplicateChallengeCounts AS (
    SELECT cnt
    FROM ChallengeCounts
    GROUP BY cnt
    HAVING COUNT(hacker_id) > 1
)

SELECT h.hacker_id, h.name, cc.cnt
FROM Hackers h
JOIN ChallengeCounts cc ON h.hacker_id = cc.hacker_id
WHERE cc.cnt = (SELECT max_cnt FROM MaxChallengeCount)
OR cc.cnt NOT IN (SELECT cnt FROM DuplicateChallengeCounts)
ORDER BY cc.cnt DESC, h.hacker_id;
```

# Page With No Likes [Facebook SQL Interview Question]
Assume you're given two tables containing data about Facebook Pages and their respective likes (as in "Like a Facebook Page").

Write a query to return the IDs of the Facebook pages that have zero likes. The output should be sorted in ascending order based on the page IDs.

Sample Input  

pages Table:  
![image](https://github.com/YaoSheng-Yu/HackerRank-datalemur-SQL-practice/assets/144596901/9aacab33-48d3-40f3-b90f-49d8935adceb)


page_likes Table:  
![image](https://github.com/YaoSheng-Yu/HackerRank-datalemur-SQL-practice/assets/144596901/b124bfcb-85c3-458b-80e3-bee21bc35b6a)


---------------------------------------  
**Solution 1**  
```PostgreSQL   
WITH like_count AS (
    SELECT page_id, COUNT(*) AS likes_num
    FROM page_likes
    GROUP BY page_id
)

SELECT p.page_id
FROM pages AS p
LEFT JOIN like_count AS l ON p.page_id = l.page_id
WHERE l.likes_num IS NULL;
```

**note**: Using left join to find null, because pages with no likes won't show up in like_counts table

**Solution 2**  
```PostgreSQL   
WITH like_count AS (
    SELECT page_id, COUNT(*) AS likes_num
    FROM page_likes
    GROUP BY page_id
)

SELECT p.page_id, COALESCE(l.likes_num, 0) AS likes_num
FROM pages AS p
LEFT JOIN like_count AS l ON p.page_id = l.page_id
WHERE COALESCE(l.likes_num, 0) = 0
ORDER BY p.page_id;

```

**note**:  show 0 instead of NULL for the number of likes, you can use the COALESCE function. COALESCE returns the first non-null value in a list. By using it, you can replace NULL with 0.

# Laptop vs. Mobile Viewership [New York Times SQL Interview Question]
Assume you're given the table on user viewership categorised by device type where the three types are laptop, tablet, and phone.

Write a query that calculates the total viewership for laptops and mobile devices where mobile is defined as the sum of tablet and phone viewership. Output the total viewership for laptops as laptop_reviews and the total viewership for mobile devices as mobile_views.

Sample Input  

viewership  Table:  
![image](https://github.com/YaoSheng-Yu/HackerRank-datalemur-SQL-practice/assets/144596901/7374946a-c7de-4b74-8009-aca0ae329efa)



---------------------------------------  
**Solution 1**  
```PostgreSQL   
SELECT 
  COUNT(*) FILTER (WHERE device_type = 'laptop') AS laptop_views,
  COUNT(*) FILTER (WHERE device_type IN ('tablet', 'phone'))  AS mobile_views 
FROM viewership;
```

**Solution 2**  
```PostgreSQL   
SELECT 
  SUM(CASE WHEN device_type = 'laptop' THEN 1 ELSE 0 END) AS laptop_views, 
  SUM(CASE WHEN device_type IN ('tablet', 'phone') THEN 1 ELSE 0 END) AS mobile_views 
FROM viewership;
```

# Duplicate Job Listings [Linkedin SQL Interview Question]
Assume you're given a table containing job postings from various companies on the LinkedIn platform. Write a query to retrieve the count of companies that have posted duplicate job listings.

Definition:
Duplicate job listings are defined as two job listings within the same company that share identical titles and descriptions.

Sample Input  

job_listings Table:  
![image](https://github.com/YaoSheng-Yu/HackerRank-datalemur-SQL-practice/assets/144596901/a68dd821-fea3-415a-bfa1-dc35cf7954d0)


---------------------------------------  
**Solution 1**  
```PostgreSQL   
WITH DuplicateJobListings AS (
    SELECT company_id
    FROM job_listings
    GROUP BY company_id, title, description
    HAVING COUNT(*) > 1
)

SELECT COUNT(DISTINCT company_id) AS duplicate_companies
FROM DuplicateJobListings;
```

**Solution 2**  
```PostgreSQL   
WITH RankedJobListings AS (
    SELECT company_id, title, description, 
           ROW_NUMBER() OVER (PARTITION BY company_id, title, description ORDER BY id) as rn
    FROM job_listings
)
SELECT COUNT(DISTINCT company_id)
FROM RankedJobListings
WHERE rn > 1
```
**note**: This approach assigns a row number within each partition of company_id, title, and description. Any row number greater than 1 indicates a duplicate.

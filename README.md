
***Contest Leaderboard***
The total score of a hacker is the sum of their maximum scores for all of the challenges. Write a query to print the hacker_id, name, and total score of the hackers ordered by the descending score. If more than one hacker achieved the same total score, then sort the result by ascending hacker_id. Exclude all hackers with a total score of  from your result.

Sample Input

Hackers Table: 
![image](https://github.com/YaoSheng-Yu/HackerRank-SQL-practice/assets/144596901/4711d68c-6878-487a-a32e-4eb3112b6512)


Submissions Table: 
![image](https://github.com/YaoSheng-Yu/HackerRank-SQL-practice/assets/144596901/6eb00e34-b398-4085-a3fc-6af9c42b0815)


Sample Output

4071 Rose 191
74842 Lisa 174
84072 Bonnie 100
4806 Angela 89
26071 Frank 85
80305 Kimberly 67
49438 Patrick 43
---------------------------------------
**Solution**
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
# SQL Practice  
----------------------------------------------------
-- DAY 1
----------------------------------------------------

-- Q1: Recyclable and Low Fat Products

SELECT product_id
FROM Products 
WHERE low_fats = 'Y' AND recyclable = 'Y';

-- Q2: Find Customer Referee

SELECT name
FROM Customer
WHERE referee_id != 2 OR referee_id IS NULL;

-- Q3: Big Countries

SELECT name, population, area
FROM World
WHERE area >= 3000000 OR population >= 25000000;


----------------------------------------------------
-- DAY 2
----------------------------------------------------

-- Q4: Article Views I

SELECT DISTINCT author_id AS id
FROM Views
WHERE author_id = viewer_id;

-- Q5: Invalid Tweets

SELECT tweet_id
FROM Tweets
WHERE LENGTH(content) > 15;

-- Q6: Employee Unique ID

SELECT unique_id, name
FROM Employees e
LEFT JOIN EmployeeUNI u
ON e.id = u.id;


----------------------------------------------------
-- DAY 3
----------------------------------------------------

-- Q7: Product Sales Analysis I

SELECT p.product_name, s.year, s.price
FROM Sales s
LEFT JOIN Product p
ON s.product_id = p.product_id;

-- Q8: Bank Account Summary II

SELECT u.name, SUM(t.amount) AS balance
FROM Users u
JOIN Transactions t
ON u.account_id = t.account_id
GROUP BY u.account_id;

-- Q9: Contest Participation %

SELECT contest_id,
ROUND(COUNT(user_id) * 100.0 / (SELECT COUNT(*) FROM Users), 2) AS percentage
FROM Register
GROUP BY contest_id;


----------------------------------------------------
-- DAY 4
----------------------------------------------------

-- Q10: Queries Quality

SELECT query_name,
ROUND(AVG(rating / position), 2) AS quality
FROM Queries
WHERE query_name IS NOT NULL
GROUP BY query_name;

-- Q11: Monthly Transactions

SELECT DATE_FORMAT(trans_date, '%Y-%m') AS month,
country,
COUNT(*) AS trans_count,
SUM(amount) AS total_amount
FROM Transactions
GROUP BY DATE_FORMAT(trans_date, '%Y-%m'), country;

-- Q12: Immediate Delivery %

SELECT ROUND(AVG(order_date = customer_pref_delivery_date) * 100, 2) AS immediate_percentage
FROM Delivery;

----------------------------------------------------
-- DAY 5
----------------------------------------------------

-- Q13: 197 Rising Temperature

SELECT w1.id
FROM Weather w1
JOIN Weather w2
ON DATEDIFF(w1.recordDate, w2.recordDate) = 1
WHERE w1.temperature > w2.temperature;

-- Q14: 1661 Average Time per Machine

SELECT machine_id,
       ROUND(AVG(end_time - start_time), 3) AS processing_time
FROM (
    SELECT machine_id,
           process_id,
           MAX(CASE WHEN activity_type = 'end' THEN timestamp END) AS end_time,
           MAX(CASE WHEN activity_type = 'start' THEN timestamp END) AS start_time
    FROM Activity
    GROUP BY machine_id, process_id
) t
GROUP BY machine_id;

-- Q15: 577 Employee Bonus

SELECT e.name, b.bonus
FROM Employee e
LEFT JOIN Bonus b
ON e.empId = b.empId
WHERE b.bonus < 1000 OR b.bonus IS NULL;

----------------------------------------------------
-- DAY 6 (Q16 - Q18)
----------------------------------------------------

-- Q16: 1280. Students and Examinations

SELECT 
    s.student_id,
    s.student_name,
    sub.subject_name,
    COUNT(e.subject_name) AS attended_exams
FROM Students s
CROSS JOIN Subjects sub
LEFT JOIN Examinations e
    ON s.student_id = e.student_id
    AND sub.subject_name = e.subject_name
GROUP BY 
    s.student_id,
    s.student_name,
    sub.subject_name
ORDER BY 
    s.student_id,
    sub.subject_name;

-- Q17: 1581. Customer Who Visited but Did Not Make Any Transactions

SELECT 
    v.customer_id,
    COUNT(v.visit_id) AS count_no_trans
FROM Visits v
LEFT JOIN Transactions t
    ON v.visit_id = t.visit_id
WHERE t.visit_id IS NULL
GROUP BY v.customer_id;

-- Q18: 1633. Percentage of Users Attended a Contest

SELECT 
    contest_id,
    ROUND(
        COUNT(DISTINCT user_id) * 100.0 /
        (SELECT COUNT(*) FROM Users),
    2) AS percentage
FROM Register
GROUP BY contest_id
ORDER BY percentage DESC, contest_id;

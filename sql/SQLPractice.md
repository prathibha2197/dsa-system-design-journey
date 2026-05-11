\-- SQL Practice (LeetCode 50)



\-- Q1: Recyclable and Low Fat Products

SELECT product\_id

FROM Products

WHERE low\_fats = 'Y' AND recyclable = 'Y';



\-- Q2: Find Customer Who Never Orders

SELECT name AS Customers

FROM Customers

WHERE id NOT IN (

&#x20;   SELECT customerId FROM Orders

);



\-- Q3: Combine Two Tables

SELECT firstName, lastName, city, state

FROM Person p

LEFT JOIN Address a

ON p.personId = a.personId;



\-- Q4: Find Customer Referee

SELECT name

FROM Customer

WHERE referee\_id != 2 OR referee\_id IS NULL;



\-- Q5: Big Countries

SELECT name, population, area

FROM World

WHERE area >= 3000000 OR population >= 25000000;



\-- Q6: Article Views I

SELECT DISTINCT author\_id AS id

FROM Views

WHERE author\_id = viewer\_id

ORDER BY id;


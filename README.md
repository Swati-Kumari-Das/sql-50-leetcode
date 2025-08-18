# SQL 50 - LeetCode Solutions

---

## 1. Recyclable and Low Fat Products (1757)

**Problem:**  
Find the `product_id` of products that are both **low fat** and **recyclable**.

**Solution:**

SELECT product_id
FROM Products
WHERE low_fats = 'Y'
  AND recyclable = 'Y';



## 2. Find Customer Referee (584)
**Problem:**  
Find the names of customers who are **not referred by customer with id = 2**.


**Solution:**


SELECT name 
FROM Customer 
WHERE referee_id != 2 
   OR referee_id IS NULL;


## 3. Big Countries

**Solution:** 

select name,population,area from world where area >=3000000 ||  population>=25000000;

## 4. Article Views I
**Solution:** 

select distinct author_id as id from views where author_id=viewer_id order by author_id;

## 5. Invalid Tweets
**Solution:** 

select tweet_id from Tweets where length(content)>15;

## 6. Replace Employee ID With The Unique Identifier

**Solution:** 
SELECT u.unique_id, e.name
FROM Employees e
LEFT JOIN EmployeeUNI u
ON e.id = u.id;

## 7. Product Sales Analysis I


**Solution:** 
SELECT product_name, s.year, s.price from Sales s 
JOIN Product p 
ON p.product_id = s.product_id

## 8. Customer Who Visited but Did Not Make Any Transactions

**Solution:** 

SELECT v.customer_id, COUNT(*) AS count_no_trans
FROM Visits v
LEFT JOIN Transactions t ON v.visit_id = t.visit_id
WHERE t.transaction_id IS NULL
GROUP BY v.customer_id;


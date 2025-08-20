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

SELECT v.customer_id, COUNT(v.visit_id) AS count_no_trans
FROM Visits v
LEFT JOIN Transactions t ON v.visit_id = t.visit_id
WHERE t.transaction_id IS NULL
GROUP BY v.customer_id;

## 9.  Rising Temperature

**Solution:** 

 SELECT W1.id
 FROM Weather W1, Weather W2 
 WHERE DATEDIFF(W1.recordDate, W2.recordDate) = 1
 AND W1.temperature > W2.temperature;


**Solution:** 

SELECT W1.id
FROM Weather W1
JOIN Weather W2 
  ON  SUBDATE(W1.recordDate,1) =  W2.recordDate
WHERE W1.temperature > W2.temperature;

## 10.  Average Time of Process per Machine

**Solution:** 

select a1.machine_id,round(avg(a2.timestamp - a1.timestamp),3) as processing_time from activity a1
join activity a2
on a1.machine_id = a2.machine_id
and a1.process_id= a2.process_id
-- and a1.timestamp< a2.timestamp
and a1.activity_type='start'
and a2.activity_type='end'
group by a1.machine_id



## 11. Employee Bonus

**Solution:** 

select e.name, b.bonus
from employee e
left join bonus b
 on e.empId = b.empId
where b.bonus < 1000 or b.bonus is null;


## 12.  Students and Examinations

**Solution:** 

```sql

select s.student_id, s.student_name, sub.subject_name,
count(e.subject_name) as attended_exams
from Students s
cross join Subjects sub
left join Examinations e
on s.student_id = e.student_id
and e.subject_name = sub.subject_name
group by s.student_id, s.student_name, sub.subject_name
order by s.student_id, s.student_name ;


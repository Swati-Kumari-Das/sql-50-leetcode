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

```sql
select a1.machine_id,round(avg(a2.timestamp - a1.timestamp),3) as processing_time from activity a1
join activity a2
on a1.machine_id = a2.machine_id
and a1.process_id= a2.process_id
-- and a1.timestamp< a2.timestamp
and a1.activity_type='start'
and a2.activity_type='end'
group by a1.machine_id

```

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

```

## 13. Managers with at Least 5 Direct Reports

**Solution:** 
```sql

select e1.name
from employee e1
inner join employee e2
on e1.id= e2.managerId
group by e2.managerid
having count(e2.managerid)>=5

```


## 14. Confirmation Rate

**Solution:** 
```sql
select s.user_id,
-- round(avg(if(c.action='confirmed',1,0)),2) as confirmation_rate
ifnull(round(sum(c.action='confirmed')/count(*),2),0.00) as confirmation_rate
from Signups s
left join Confirmations c
on s.user_id =c.user_id
group by s.user_id
```

## 15. Not Boring Movies

**Solution:** 
```sql

Select * from Cinema where id%2!=0 And description!="boring" order by rating desc;
```
## 16. Average Selling Price

**Solution:** 
```sql

select p.product_id,
ifnull(round(sum(p.price*u.units)/sum(u.units),2),0) as average_price
from prices p
left join UnitsSold u
on p.product_id=u.product_id
and u.purchase_date between p.start_date and p.end_date
group by p.product_id
```

## 17. Project Employees I

**Solution:** 
```sql
SELECT p.project_id,
       ROUND(AVG(e.experience_years), 2) AS average_years
FROM Project p
LEFT JOIN Employee e
       ON p.employee_id = e.employee_id
GROUP BY p.project_id;

```

## 18.  Percentage of Users Attended a Contest


**Solution:** 
```sql
select contest_id ,
round(count(distinct user_id)*100 / (select count(user_id) from users),2) as percentage
from Register
group by contest_id
order by percentage desc, contest_id asc;
```

## 19.  Queries Quality and Percentage


**Solution:** 

```sql

select 
query_name ,
round(avg(rating/position),2) as quality,
round(avg(if(rating <3,1,0)*100),2) as poor_query_percentage
from queries
where query_name is not null
group by query_name
```

## 20. Monthly Transactions I

**Solution:** 

```sql
select 
date_format(trans_date,"%Y-%m") as month,
country ,
count(id) as trans_count,
sum(if(state='approved',1,0)) as approved_count,
sum(amount) as trans_total_amount,
sum(if(state='approved',amount,0)) as approved_total_amount
from transactions
group by month,country

```

## 21.  Immediate Food Delivery II

**Solution:** 

```sql

select 
round(sum(if(min_order_date=min_customer_pref_delivery_date,1,0)*100)/count(min_order_date),2)
as immediate_percentage
from
(select delivery_id,
customer_id,
min(order_date) as min_order_date,
min(customer_pref_delivery_date) as min_customer_pref_delivery_date
from delivery
group by customer_id) as new_table

```

## 22. Game Play Analysis IV


**Solution:** 

```sql

select 
round(sum(player_login)/count(distinct player_id),2)
as fraction
from
(select player_id,
datediff(event_date,min(event_date) over(partition by player_id)) =1 as player_login
from activity) as new_table


#A PARTITION By clause is used to partition rows of table into groups
#it is always used inside OVER() clause


```

## 23. User Activity for the Past 30 Days I


**Solution:** 

```sql

select teacher_id ,count(distinct subject_id) as cnt from Teacher group by teacher_id;
```

## 24.  User Activity for the Past 30 Days I
**Solution:** 

```sql
select activity_date as day ,
count(distinct user_id) as active_users
from Activity
where activity_date between date_sub('2019-07-27' , interval 29 day) and '2019-07-27'
group by day
```

## 25. Product Sales Analysis III
**Solution:** 

```sql
select 
product_id,
year  as first_year, quantity, price
from sales
where ((product_id, year) IN (select product_id, min(year) from sales
group by product_id));
```
## 26. Classes With at Least 5 Students

**Solution:** 

```sql
SELECT
    class
FROM
    Courses
-- Group the rows by class to count students in each class
GROUP BY
    class
-- Filter the groups to only include classes with 5 or more students
HAVING
    COUNT(student) >= 5;
```

## 27. Find Followers Count
**Solution:** 

```sql
select distinct(user_id) , count(follower_id) as followers_count from Followers group by user_id order by user_id;
```

## 28. Biggest Single Number
**Solution:** 

```sql
SELECT MAX(num) AS num
FROM (
    SELECT num
    FROM MyNumbers
    GROUP BY num
    HAVING COUNT(num) = 1
) AS unique_numbers;

```
## 29. Customers Who Bought All Products
**Solution:** 

```sql
select customer_id from customer 
group by customer_id 
having count(distinct product_key)=(select count(product_key) from product)
```

## 30.  The Number of Employees Which Report to Each Employee
**Solution:** 

```sql
SELECT
    E1.employee_id,
    E1.name,
    COUNT(E2.employee_id) AS reports_count,
    ROUND(AVG(E2.age)) AS average_age
FROM
    Employees E1
JOIN
    Employees E2
    ON E2.reports_to = E1.employee_id
GROUP BY
    E1.employee_id
ORDER BY
    E1.employee_id;

```

## 31.  Primary Department for Each Employee
**Solution1:** 

```sql

select employee_id, department_id
from employee
where primary_flag='Y'
group by employee_id
Union
select employee_id, department_id
from employee
group by employee_id
having count(employee_id)=1
 ```
**Solution2:** 

```sql
  
SELECT employee_id, department_id
FROM Employee
WHERE primary_flag = 'Y'
   OR employee_id IN (
       SELECT employee_id
       FROM Employee
       GROUP BY employee_id
       HAVING COUNT(*) = 1
   );



```

## 32.  Triangle Judgement
**Solution:** 

```sql
  
select x,y,z,
case when 
x+y>z and y+z >x and x+z>y then 'Yes' else 'No' 
end as triangle
from triangle
```

## 33. Consecutive Numbers

**Solution:** 
```sql

  select l1.num as ConsecutiveNums 
from Logs l1,logs l2, logs l3
where l1.id-l2.id=1
and l2.id-l3.id=1
and l1.num=l2.num
and l2.num=l3.num
and l1.num=l3.num
group by l1.num
```

## 34.  Product Price at a Given Date

**Solution:** 
```sql

select product_id, new_price as price 
from products
where (product_id, change_date) IN 
(
    select product_id, max(change_date)
    from products
    where change_date<='2019-08-16'
    group by product_id
)
union
select product_id, 10 as price 
from products
where (product_id) not IN 
(
    select product_id
    from products
    where change_date<='2019-08-16'
    group by product_id
)


```

## 35.   Last Person to Fit in the Bus

**Solution:** 
```sql

select q1.person_name 
from Queue q1 
inner join 
Queue q2 
on q1.turn>=q2.turn
group by q1.turn
having sum(q2.weight)<=1000
order by q1.turn desc limit 1;

```

## 36.  Count Salary Categories

**Solution:** 
```sql
select "Low Salary" as category,
sum(if(income<20000,1,0)) as accounts_count
from accounts
union
select "Average Salary" as category,
sum(if(income between 20000 and 50000,1,0)) as accounts_count
from accounts
union
select "High Salary" as category,
sum(if(income>50000,1,0)) as accounts_count
from accounts

```

## 37. Employees Whose Manager Left the Company

**Solution:** 
```sql
SELECT employee_id
FROM Employees
WHERE salary < 30000
AND manager_id NOT IN (
  SELECT employee_id FROM Employees
)
ORDER BY employee_id;

```
## 38.  Exchange Seats

**Solution:** 
```sql
select 
case 
when id=(select max(id) from seat) and id%2=1 then id
when id%2=1 then id+1 else id-1 end as id,
student 
from seat
order by id
```

## 39. Movie Rating
**Solution:** 
```sql

(select u.name as results 
from users u 
left join MovieRating mr
on u.user_id=mr.user_id
group by u.user_id
order by count(mr.user_id) desc,
u.name limit 1)
union all
(select m.title as results 
from movies m 
left join MovieRating mr
on m.movie_id=mr.movie_id
where extract(year_month from created_at)=202002
group by m.movie_id
order by avg(mr.rating) desc,
m.title limit 1)

```

## 40. Restaurant Growth
**Solution:** 
```sql
select visited_on,
(
    select sum(amount)
    from customer
    where visited_on 
    between date_sub(c.visited_on,interval 6 day)
    and c.visited_on
)as amount,
round((
    select sum(amount)/7
    from customer
    where visited_on 
    between date_sub(c.visited_on,interval 6 day)
    and c.visited_on
),2)as average_amount
from customer c
where visited_on >=(
     select date_add(min(visited_on), interval 6 day)
     from customer 
)
group by visited_on
order by visited_on
```

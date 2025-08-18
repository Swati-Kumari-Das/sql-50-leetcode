# SQL 50 - LeetCode Solutions

---

## 1. Recyclable and Low Fat Products (1757)

**Problem:**  
Find the `product_id` of products that are both **low fat** and **recyclable**.

**Solution:**
```sql
SELECT product_id
FROM Products
WHERE low_fats = 'Y'
  AND recyclable = 'Y';

---

## 2. Find Customer Referee (584)

**Solution:**
```sql
SELECT name 
FROM Customer 
WHERE referee_id != 2 
   OR referee_id IS NULL;


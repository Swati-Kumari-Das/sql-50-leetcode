# SQL 50 - LeetCode Solutions

## 1. Recyclable and Low Fat Products
**Problem:** Find the `product_id` of products that are both low fat and recyclable.

**Solution:**
```sql
SELECT product_id 
FROM Products 
WHERE low_fats = 'Y' 
  AND recyclable = 'Y';

##2 Find Customer Referee
   **Solution:**
```sql
Select name from Customer where referee_id!=2 or referee_id is null;

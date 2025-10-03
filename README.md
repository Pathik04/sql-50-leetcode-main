# SQL 50 From Leetcode Studyplan
Solutions for [SQL 50 Study Plan](https://leetcode.com/studyplan/top-sql-50/) on LeetCode

---


[1757 - Recyclable and Low Fat Products](https://leetcode.com/problems/recyclable-and-low-fat-products/)
```sql
SELECT product_id
FROM Products
WHERE low_fats = 'Y'
AND recyclable = 'Y'
```
---

### SQL Query Explanation

* `SELECT product_id FROM Products;` → fetch the `product_id` column from the `Products` table.
* Filter rows → `low_fats = 'Y'` **AND** `recyclable = 'Y'`.
* **Important Notes:**

  * Both conditions must be true for a row to be included.
  * Use quotes `'Y'` for string/char comparisons.

---



[584 - Find Customer Referee](https://leetcode.com/problems/find-customer-referee)
```sql
SELECT name 
FROM Customer 
WHERE referee_id != 2 OR referee_id IS null
```
---

### SQL Query Explanation

* `SELECT name FROM Customer;` → fetch the `name` column from the `Customer` table.
* Filter rows → `referee_id` ≠ 2 **OR** `referee_id` IS NULL.
* **Important Notes:**

  * `NULL` comparisons → use `IS NULL`.
  * `!=` or `<>` ❌ does NOT match `NULL`.
  * To exclude `NULL` → `AND referee_id IS NOT NULL`.

---
[595 - Big Countries](https://leetcode.com/problems/big-countries/)
```sql
SELECT name, population, area
FROM WORLD
WHERE area >= 3000000
OR population >= 25000000
```
---

### SQL Query Explanation

* `SELECT name, population, area FROM WORLD;` → fetch the `name`, `population`, and `area` columns from the `WORLD` table.
* Filter rows → include rows where **area ≥ 3,000,000** **OR** **population ≥ 25,000,000**.
* **Important Notes:**

  * `OR` means a row is included if **either** condition is true.
  * Large numbers can be written with or without commas depending on SQL dialect.

---


[1148 - Article Views I](https://leetcode.com/problems/article-views-i)
```sql
SELECT DISTINCT author_id as id
FROM Views
WHERE viewer_id >= 1
AND author_id = viewer_id
ORDER BY author_id
```
---

### SQL Query Explanation

* `SELECT DISTINCT author_id AS id FROM Views;` → fetch unique `author_id` values and rename the column as `id`.
* Filter rows → only include rows where `viewer_id >= 1` **AND** `author_id = viewer_id`.
* `ORDER BY author_id;` → sort the results in ascending order by `author_id`.
* **Important Notes:**

  * `DISTINCT` ensures duplicate `author_id` values are removed.
  * Both conditions in `WHERE` must be true for a row to be included.

---

[1683 - Invalid Tweets](https://leetcode.com/problems/invalid-tweets/)
```sql
SELECT tweet_id
FROM Tweets
WHERE length(content) > 15
```
---

### SQL Query Explanation

* `SELECT tweet_id FROM Tweets;` → fetch the `tweet_id` column from the `Tweets` table.
* Filter rows → only include rows where the length of `content` is greater than 15 characters.
* **Important Notes:**

  * `length()` (or `LENGTH()` depending on SQL dialect) returns the number of characters in a string.
  * Only tweets with more than 15 characters in `content` will be included.

---

[1378 - Replace Employee ID With The Unique Identifier](https://leetcode.com/problems/replace-employee-id-with-the-unique-identifier)
```sql
SELECT unique_id, name
FROM Employees e
LEFT JOIN EmployeeUNI eu
ON e.id = eu.id
```
---

### SQL Query Explanation

* `SELECT unique_id, name FROM Employees e LEFT JOIN EmployeeUNI eu ON e.id = eu.id;`
  → Fetch `unique_id` and `name` by joining `Employees` (`e`) with `EmployeeUNI` (`eu`) on the `id` column.
* **Join Type:**

  * `LEFT JOIN` → include **all rows from `Employees`**, even if there is **no matching row** in `EmployeeUNI`.
* **Important Notes:**

  * If an employee has no match in `EmployeeUNI`, the `unique_id` will be `NULL`.
  * Aliases (`e` and `eu`) make the query easier to read and write.

---

[1068 - Product Sales Analysis I](https://leetcode.com/problems/product-sales-analysis-i/)
```sql
SELECT product_name, year, price
FROM Sales s
LEFT JOIN Product p
ON s.product_id = p.product_id
```
---

### SQL Query Explanation

* `SELECT product_name, year, price FROM Sales s LEFT JOIN Product p ON s.product_id = p.product_id;`
  → Fetch `product_name`, `year`, and `price` by joining `Sales` (`s`) with `Product` (`p`) on `product_id`.
* **Join Type:**

  * `LEFT JOIN` → include **all rows from `Sales`**, even if there is **no matching row** in `Product`.
* **Important Notes:**

  * If a sale has no matching product, `product_name` will be `NULL`.
  * Table aliases (`s` and `p`) simplify referencing columns.

---


[1581 - Customer Who Visited but Did Not Make Any Transactions](https://leetcode.com/problems/customer-who-visited-but-did-not-make-any-transactions/)
```sql
SELECT customer_id, COUNT(*) as count_no_trans
FROM Visits 
WHERE visit_id NOT IN (SELECT DISTINCT visit_id FROM Transactions)
GROUP BY customer_id
```
---

### SQL Query Explanation

* `SELECT customer_id, COUNT(*) AS count_no_trans FROM Visits;`
  → Fetch the `customer_id` and count of visits **without transactions**.

* **Filter rows:**

  * `WHERE visit_id NOT IN (SELECT DISTINCT visit_id FROM Transactions)`
    → Only include visits that **do not appear** in the `Transactions` table.

* **Grouping:**

  * `GROUP BY customer_id` → Aggregate the count for each customer separately.

* **Important Notes:**

  * `COUNT(*)` counts the number of rows per customer.
  * `NOT IN` excludes all visits that have corresponding transactions.
  * `DISTINCT` ensures no duplicate `visit_id` in the subquery.

---

[197 - Rising Temperature](https://leetcode.com/problems/rising-temperature/) 
```sql
SELECT w1.id 
FROM Weather w1, Weather w2
WHERE DATEDIFF(w1.recordDate, w2.recordDate) = 1
AND w1.temperature > w2.temperature

-- OR
SELECT w1.id
FROM Weather w1, Weather w2
WHERE w1.temperature > w2.temperature
AND SUBDATE(w1.recordDate, 1) = w2.recordDate
```
---

### SQL Query Explanation

* **Query Purpose:**
  Find the `id` of days where the temperature was higher than the previous day.

* **Query:**

  * `SUBDATE(w1.recordDate, 1) = w2.recordDate` → Match the previous day.
  * `w1.temperature > w2.temperature` → Include days where temperature increased from the previous day.

* **Important Notes:**

  * Self-join (`w1`, `w2`) compares rows within the same table.
  * This approach is more efficient and readable than using `DATEDIFF`.

---


[1661 - Average Time of Process per Machine](https://leetcode.com/problems/average-time-of-process-per-machine/)
```sql
SELECT machine_id, ROUND(AVG(end - start), 3) AS processing_time
FROM 
(SELECT machine_id, process_id, 
    MAX(CASE WHEN activity_type = 'start' THEN timestamp END) AS start,
    MAX(CASE WHEN activity_type = 'end' THEN timestamp END) AS end
 FROM Activity 
  GROUP BY machine_id, process_id) AS subq
GROUP BY machine_id
```
---

### SQL Query Explanation

* **Query Purpose:**
  Calculate the **average processing time** for each machine.

* **Logic:**

  * The subquery groups data by `machine_id` and `process_id`.
  * `MAX(CASE WHEN activity_type = 'start' THEN timestamp END)` → Finds the start time of each process.
  * `MAX(CASE WHEN activity_type = 'end' THEN timestamp END)` → Finds the end time of each process.
  * The difference `(end - start)` gives the processing time for each process.
  * `ROUND(AVG(end - start), 3)` → Computes the **average processing time** per machine and rounds it to 3 decimal places.

* **Important Notes:**

  * Subquery is necessary to calculate start and end timestamps per process.
  * Outer query aggregates the results per machine.
  * `ROUND()` ensures consistent precision in the output.

---

[577 - Employee Bonus](https://leetcode.com/problems/employee-bonus/solutions/)
```sql
SELECT name, bonus
FROM Employee e
LEFT JOIN Bonus b
ON e.empId = b.empId
WHERE bonus < 1000
OR bonus IS NULL
```
---

### SQL Query Explanation

* **Query Purpose:**
  Retrieve the `name` and `bonus` of employees who have a bonus less than 1000 or no bonus recorded.

* **Logic:**

  * `LEFT JOIN` connects `Employee` (`e`) with `Bonus` (`b`) on `empId`.
  * `bonus < 1000 OR bonus IS NULL` → Include employees with small bonuses or **no bonus**.
  * Using `LEFT JOIN` ensures employees without a bonus still appear in the results.

* **Important Notes:**

  * `IS NULL` is necessary to check for employees without a bonus.
  * This query combines **filtering** and **joining** to handle both existing and missing bonus data.

---

[1280 - Students and Examinations](https://leetcode.com/problems/students-and-examinations/)
```sql
SELECT a.student_id, a.student_name, b.subject_name, COUNT(c.subject_name) AS attended_exams
FROM Students a
JOIN Subjects b
LEFT JOIN Examinations c
ON a.student_id = c.student_id
AND b.subject_name = c.subject_name
GROUP BY 1, 3
ORDER BY 1, 3 
```
---

### SQL Query Explanation

* **Query Purpose:**
  Show how many exams each student has attended for each subject.

* **Logic:**

  * `JOIN Subjects b` → Combine every student with all subjects.
  * `LEFT JOIN Examinations c` → Match exams attended by students for each subject (if any).
  * `COUNT(c.subject_name)` → Count how many times a student attended that subject’s exam.
  * `GROUP BY 1, 3` → Group results by student (`1` = first column) and subject (`3` = third column).
  * `ORDER BY 1, 3` → Sort results by student ID and subject name.

* **Important Notes:**

  * Using `LEFT JOIN` ensures subjects with **no exams attended** still appear, with count = 0.
  * `GROUP BY` with column positions (`1, 3`) is shorthand for column names.

---

[570. Managers with at Least 5 Direct Reports](https://leetcode.com/problems/managers-with-at-least-5-direct-reports)
```sql
SELECT name 
FROM Employee 
WHERE id IN
  (SELECT managerId 
   FROM Employee 
   GROUP BY managerId 
   HAVING COUNT(*) >= 5
  )

-- OR
SELECT a.name
FROM Employee a
JOIN Employee b
WHERE a.id = b.managerId
GROUP BY b.managerId
HAVING COUNT(*) >= 5
```
---

### SQL Query Explanation

* **Query Purpose:**
  Find the names of employees who are managers of at least 5 employees.

* **Logic (using subquery version):**

  * Inner query → `SELECT managerId FROM Employee GROUP BY managerId HAVING COUNT(*) >= 5`

    * Finds all `managerId` values that manage **5 or more employees**.
  * Outer query → `SELECT name FROM Employee WHERE id IN ( ... )`

    * Retrieves the names of those managers.

* **Important Notes:**

  * `HAVING COUNT(*) >= 5` ensures only managers with at least 5 direct reports are included.
  * This version is simple and clear, making it easier to read than the join-based alternative.

---


[1934. Confirmation Rate](https://leetcode.com/problems/confirmation-rate/)
```sql
SELECT 
  s.user_id, 
  ROUND(
    COALESCE(
      SUM(
        CASE WHEN ACTION = 'confirmed' THEN 1 END
      ) / COUNT(*), 0),2) 
  AS confirmation_rate 
FROM Signups s 
LEFT JOIN Confirmations c 
ON s.user_id = c.user_id 
GROUP BY s.user_id;  
```
---

### SQL Query Explanation

* **Query Purpose:**
  Calculate the **confirmation rate** of each user based on their signups and confirmations.

* **Logic:**

  * `LEFT JOIN Confirmations c ON s.user_id = c.user_id` → Include all users from `Signups`, even if they have no confirmations.
  * `CASE WHEN action = 'confirmed' THEN 1 END` → Count only confirmations marked as `'confirmed'`.
  * `SUM(...) / COUNT(*)` → Ratio of confirmed actions to total actions per user.
  * `COALESCE(..., 0)` → If no confirmations exist, return `0` instead of `NULL`.
  * `ROUND(..., 2)` → Round the confirmation rate to 2 decimal places.

* **Important Notes:**

  * Using `LEFT JOIN` ensures users without confirmations still appear with a rate of `0`.
  * The result shows `user_id` along with their computed `confirmation_rate`.

---

[620. Not Boring Movies](https://leetcode.com/problems/not-boring-movies)
```sql
-- odd id, "boring", rating desc
SELECT *
FROM Cinema
WHERE id % 2 <> 0 
AND description <> "boring"
ORDER BY rating DESC
```
---

### SQL Query Explanation

* **Query Purpose:**
  Retrieve all movies with odd IDs that are **not described as "boring"**, sorted by rating in descending order.

* **Logic:**

  * `id % 2 <> 0` → Select only rows with odd `id` values.
  * `description <> "boring"` → Exclude movies where the description is exactly `"boring"`.
  * `ORDER BY rating DESC` → Sort results by `rating`, highest first.

* **Important Notes:**

  * `%` operator is used to check odd/even IDs.
  * `<>` is the SQL standard for “not equal.”
  * Sorting ensures top-rated results appear first.

---

[1251. Average Selling Price](https://leetcode.com/problems/average-selling-price/)
```sql
-- avg(selling), round 2
SELECT p.product_id, 
  ROUND(SUM(price * units) / SUM(units), 2) AS average_price
FROM Prices p
LEFT JOIN UnitsSold s
ON p.product_id = s.product_id
AND purchase_date BETWEEN start_date AND end_date
GROUP BY p.product_id
```
---

### SQL Query Explanation

* **Query Purpose:**
  Calculate the **average selling price** for each product, rounded to 2 decimal places.

* **Logic:**

  * `LEFT JOIN UnitsSold s ON p.product_id = s.product_id AND purchase_date BETWEEN start_date AND end_date`
    → Match each product’s sales within its valid price date range.
  * `SUM(price * units)` → Total revenue from all units sold.
  * `SUM(units)` → Total number of units sold.
  * `SUM(price * units) / SUM(units)` → Weighted average selling price.
  * `ROUND(..., 2)` → Round result to 2 decimal places.

* **Important Notes:**

  * `LEFT JOIN` ensures all products are included, even if no units were sold.
  * Using weighted average avoids errors when prices change over time.

---

[1075. Project Employees I](https://leetcode.com/problems/project-employees-i)
```sql
-- avg(exp_yr), round 2, by project
SELECT project_id, ROUND(AVG(experience_years), 2) average_years
FROM Project p 
LEFT JOIN Employee e
ON p.employee_id = e.employee_id
GROUP BY project_id
```
---

### SQL Query Explanation

* **Query Purpose:**
  Calculate the **average employee experience (in years)** for each project, rounded to 2 decimal places.

* **Logic:**

  * `LEFT JOIN Employee e ON p.employee_id = e.employee_id` → Match each project with its employees.
  * `AVG(experience_years)` → Compute the average experience of employees in a project.
  * `ROUND(..., 2)` → Round the result to 2 decimal places.
  * `GROUP BY project_id` → Perform the calculation separately for each project.

* **Important Notes:**

  * `LEFT JOIN` ensures all projects are included, even if no employees are linked.
  * The result shows one row per `project_id` with its average employee experience.

---

[1633. Percentage of Users Attended a Contest](https://leetcode.com/problems/percentage-of-users-attended-a-contest)
```sql
-- % desc, contest_id asc, round 2
SELECT r.contest_id,
       ROUND(COUNT(DISTINCT r.user_id) * 100 / (SELECT COUNT(DISTINCT user_id) FROM Users), 2) AS percentage
FROM Register r
GROUP BY r.contest_id
ORDER BY percentage DESC, r.contest_id ASC;
```
---

### SQL Query Explanation

* **Query Purpose:**
  Find the percentage of users registered in each contest, rounded to 2 decimal places.

* **Logic:**

  * `COUNT(DISTINCT r.user_id)` → Count unique users registered in a contest.
  * `(SELECT COUNT(DISTINCT user_id) FROM Users)` → Total number of unique users.
  * `COUNT(...) * 100 / (total users)` → Calculate registration percentage per contest.
  * `ROUND(..., 2)` → Round the percentage to 2 decimal places.
  * `GROUP BY r.contest_id` → Compute the percentage for each contest.
  * `ORDER BY percentage DESC, r.contest_id ASC` → Sort by highest percentage first, then by contest ID in ascending order.

* **Important Notes:**

  * Using `DISTINCT` avoids counting duplicate registrations.
  * The result shows participation rates for each contest as a percentage of all users.

---

[1211 Queries Quality and Percentage](https://leetcode.com/problems/queries-quality-and-percentage)

```sql
--quality - avg(rating/position), poor query % - %(rating < 3), round 2
SELECT query_name, 
    ROUND(AVG(rating/position), 2) AS quality, 
    ROUND(SUM(IF(rating < 3, 1, 0)) * 100/ COUNT(rating), 2) AS poor_query_percentage
FROM Queries
GROUP BY query_name

-- OR
SELECT query_name, 
    ROUND(AVG(rating/position), 2) AS quality, 
    ROUND(SUM(
        CASE WHEN rating < 3 THEN 1 ELSE 0 END
    ) * 100/ COUNT(rating), 2) AS poor_query_percentage
FROM Queries
GROUP BY query_name
```
---

### SQL Query Explanation

* **Query Purpose:**
  Evaluate each query’s **quality score** and calculate the percentage of **poor queries** (ratings below 3).

* **Logic:**

  * `AVG(rating/position)` → Average quality score (rating adjusted by position).
  * `ROUND(..., 2)` → Round quality to 2 decimal places.
  * `SUM(CASE WHEN rating < 3 THEN 1 ELSE 0 END)` → Count poor queries (rating < 3).
  * `COUNT(rating)` → Total queries for that query name.
  * `(poor / total) * 100` → Percentage of poor queries.
  * `GROUP BY query_name` → Compute results per query name.

* **Important Notes:**

  * `CASE WHEN ... END` is preferred over `IF(...)` for portability across SQL databases.
  * Both queries are functionally the same, but the `CASE` version is **more standard SQL**.

---

[1193. Monthly Transactions I](https://leetcode.com/problems/monthly-transactions-i/)
```sql
-- month, country, count(trans), total(amt), count(approved_trans), total(amt)
SELECT DATE_FORMAT(trans_date, '%Y-%m') month, country, 
        COUNT(state) trans_count, 
        SUM(IF(state = 'approved', 1, 0)) approved_count, 
        SUM(amount) trans_total_amount,
        SUM(IF(state = 'approved', amount, 0)) approved_total_amount
FROM Transactions
GROUP BY 1, 2

-- OR
SELECT DATE_FORMAT(trans_date, '%Y-%m') month, country, 
        COUNT(state) trans_count, 
        SUM(CASE WHEN state = 'approved' THEN 1 ELSE 0 END) approved_count, 
        SUM(amount) trans_total_amount,
        SUM(CASE WHEN state = 'approved' THEN amount ELSE 0 END) approved_total_amount
FROM Transactions
GROUP BY 1, 2
```
---

### SQL Query Explanation

* **Query Purpose:**
  Generate monthly transaction reports by country, including total counts, amounts, and approved transaction details.

* **Logic:**

  * `DATE_FORMAT(trans_date, '%Y-%m')` → Extracts year-month from transaction date.
  * `COUNT(state)` → Total number of transactions.
  * `SUM(CASE WHEN state = 'approved' THEN 1 ELSE 0 END)` → Number of approved transactions.
  * `SUM(amount)` → Total transaction amount.
  * `SUM(CASE WHEN state = 'approved' THEN amount ELSE 0 END)` → Total amount of approved transactions.
  * `GROUP BY 1, 2` → Group results by month and country.

* **Important Notes:**

  * Both versions are functionally identical.
  * `CASE WHEN ... END` is the preferred SQL standard over `IF(...)` for better portability.
  * Output shows, for each `(month, country)`, the overall transactions and the subset that were approved.

---


[1174. Immediate Food Delivery II](https://leetcode.com/problems/immediate-food-delivery-ii/)
```sql
SELECT
    ROUND((COUNT(CASE WHEN d.order_date = d.customer_pref_delivery_date THEN 1 END) / COUNT(*)) * 100, 2)  immediate_percentage
FROM Delivery d
WHERE d.order_date = (
    SELECT
    MIN(order_date)
    FROM Delivery
    WHERE customer_id = d.customer_id
    );

-- OR
SELECT ROUND(AVG(temp.order_date=temp.customer_pref_delivery_date) * 100, 2) immediate_percentage
FROM (
    SELECT *, RANK() OVER(partition by customer_id ORDER BY order_date) od
    FROM Delivery) temp
WHERE temp.od = 1
```
---

### SQL Query Explanation

* **Query Purpose:**
  Calculate the percentage of customers whose **first order** was delivered on their **preferred delivery date**.

* **Logic (first query):**

  * For each customer, find their earliest order (`MIN(order_date)`).
  * Compare `order_date` with `customer_pref_delivery_date`.
  * `COUNT(CASE WHEN ... THEN 1 END)` → Count first orders delivered on the preferred date.
  * Divide by total first orders, multiply by 100, and round to 2 decimals.

* **Logic (second query):**

  * Use `RANK() OVER (PARTITION BY customer_id ORDER BY order_date)` to mark each customer’s first order.
  * Compare `order_date` with `customer_pref_delivery_date`.
  * `AVG(condition)` → MySQL treats `TRUE` as `1` and `FALSE` as `0`, so averaging gives the percentage.
  * Multiply by 100 and round to 2 decimals.

* **Important Notes:**

  * Both queries return the same result.
  * The **window function version (second query)** is more elegant and usually easier to maintain.
  * The **subquery version (first query)** is more universally supported across SQL engines.

---

[550. Game Play Analysis IV](https://leetcode.com/problems/game-play-analysis-iv/)
```sql
WITH login_date AS (SELECT player_id, MIN(event_date) AS first_login
FROM Activity
GROUP BY player_id),

recent_login AS (
SELECT *, DATE_ADD(first_login, INTERVAL 1 DAY) AS next_day
FROM login_date)

SELECT ROUND((SELECT COUNT(DISTINCT(player_id))
FROM Activity
WHERE (player_id, event_date) IN 
(SELECT player_id, next_day FROM recent_login)) / (SELECT COUNT(DISTINCT player_id) FROM Activity), 2) AS fraction
```
---

### SQL Query Explanation

* **Query Purpose:**
  Calculate the fraction of players who logged in **on the day after their first login**.

* **Logic:**

  1. **CTE `login_date`** → Find each player’s first login date.
  2. **CTE `recent_login`** → Add 1 day to each player’s first login to get the `next_day`.
  3. **Main query:**

     * Count distinct `player_id`s who logged in on `next_day`.
     * Divide by total distinct `player_id`s to get the fraction.
     * `ROUND(..., 2)` → Round the fraction to 2 decimal places.

* **Important Notes:**

  * Uses **Common Table Expressions (CTEs)** for clarity.
  * `(player_id, event_date) IN (...)` matches players with their specific next-day login.
  * The result represents the **retention fraction** of players returning the day after their first login.

---

[2356. Number of Unique Subjects Taught by Each Teacher](https://leetcode.com/problems/number-of-unique-subjects-taught-by-each-teacher)
```sql
SELECT teacher_id, COUNT(DISTINCT subject_id) cnt
FROM Teacher
GROUP BY teacher_id
```
---

### SQL Query Explanation

* **Query Purpose:**
  Count the number of **distinct subjects** each teacher handles.

* **Logic:**

  * `COUNT(DISTINCT subject_id)` → Count unique subjects per teacher.
  * `GROUP BY teacher_id` → Aggregate results by each teacher.

* **Important Notes:**

  * Ensures that even if a teacher appears multiple times for the same subject, it is counted only once.
  * The result shows each `teacher_id` along with the number of subjects they teach.

---

[1141. User Activity for the Past 30 Days I](https://leetcode.com/problems/user-activity-for-the-past-30-days-i/)
```sql
SELECT activity_date as day, COUNT(DISTINCT user_id) AS active_users
FROM Activity
WHERE activity_date BETWEEN DATE_SUB('2019-07-27', INTERVAL 29 DAY) AND '2019-07-27'
GROUP BY activity_date
```
---

### SQL Query Explanation

* **Query Purpose:**
  Count the number of **active users per day** over the last 30 days ending on `2019-07-27`.

* **Logic:**

  * `activity_date BETWEEN DATE_SUB('2019-07-27', INTERVAL 29 DAY) AND '2019-07-27'` → Select data for the 30-day period.
  * `COUNT(DISTINCT user_id)` → Count unique users active each day.
  * `GROUP BY activity_date` → Aggregate counts for each day separately.

* **Important Notes:**

  * Using `DISTINCT` ensures users are counted only once per day, even if they performed multiple activities.
  * The result shows daily active users (DAU) for the specified period.

---

[1070. Product Sales Analysis III
](https://leetcode.com/problems/product-sales-analysis-iii/)
```sql
SELECT s.product_id, s.year AS first_year, s.quantity, s.price
FROM Sales s
JOIN (
  SELECT product_id, MIN(year) AS year 
  FROM sales 
  GROUP BY product_id
  ) p
ON s.product_id = p.product_id
AND s.year = p.year

-- OR
WITH first_year_sales AS (
  SELECT s.product_id, MIN(s.year) as first_year
  FROM Sales s
  INNER JOIN Product p
  ON s.product_id = p.product_id
  GROUP BY s.product_id)
SELECT f.product_id, f.first_year, s.quantity, s.price
FROM first_year_sales f
JOIN Sales s 
ON f.product_id = s.product_id
AND f.first_year = s.year
```
---

### SQL Query Explanation

* **Query Purpose:**
  Retrieve sales details (quantity and price) for the **first year each product was sold**.

* **Logic (first query using subquery):**

  * Inner query → Find the **earliest year** (`MIN(year)`) each product was sold.
  * Outer query → Join with `Sales` to get `quantity` and `price` for that first year.

* **Logic (second query using CTE):**

  * `WITH first_year_sales AS (...)` → Create a CTE to store the first year for each product.
  * Join CTE with `Sales` to fetch the corresponding `quantity` and `price`.

* **Important Notes:**

  * Both approaches return the same result.
  * Using a **CTE** makes the query more readable and easier to maintain.
  * `JOIN` ensures only sales from the **first year** are included.

---

[596. Classes More Than 5 Students](https://leetcode.com/problems/classes-more-than-5-students/)
```sql
SELECT class
FROM Courses
GROUP BY class
HAVING COUNT(student) >= 5
```
---

### SQL Query Explanation

* **Query Purpose:**
  Find all classes that have **at least 5 students** enrolled.

* **Logic:**

  * `GROUP BY class` → Aggregate data for each class.
  * `HAVING COUNT(student) >= 5` → Include only classes with 5 or more students.

* **Important Notes:**

  * `HAVING` is used instead of `WHERE` because it filters **aggregated results**.
  * The result shows only the class names meeting the minimum student count requirement.

---

[1729. Find Followers Count](https://leetcode.com/problems/find-followers-count/)
```sql
SELECT user_id, COUNT(DISTINCT follower_id) AS followers_count
FROM Followers
GROUP BY user_id
ORDER BY user_id ASC
```
---

### SQL Query Explanation

* **Query Purpose:**
  Count the number of **distinct followers** for each user.

* **Logic:**

  * `COUNT(DISTINCT follower_id)` → Counts unique followers per user.
  * `GROUP BY user_id` → Aggregate the count for each user.
  * `ORDER BY user_id ASC` → Sort results by `user_id` in ascending order.

* **Important Notes:**

  * Using `DISTINCT` ensures that duplicate follower entries are not counted multiple times.
  * The result shows each `user_id` along with the number of unique followers they have.

---

[619. Biggest Single Number](https://leetcode.com/problems/biggest-single-number/)
```sql
SELECT COALESCE(
  (SELECT num
  FROM MyNumbers
  GROUP BY num
  HAVING COUNT(num) = 1
  ORDER BY num DESC
  LIMIT 1), null) 
  AS num
```
---

### SQL Query Explanation

* **Query Purpose:**
  Find the **largest number that appears only once** in the `MyNumbers` table.

* **Logic:**

  * `GROUP BY num` → Aggregate identical numbers.
  * `HAVING COUNT(num) = 1` → Keep only numbers that occur **exactly once**.
  * `ORDER BY num DESC LIMIT 1` → Select the **largest unique number**.
  * `COALESCE(..., null)` → Return `NULL` if no such number exists.

* **Important Notes:**

  * Ensures that if all numbers are repeated, the query safely returns `NULL`.
  * The result gives a single value: the largest unique number or `NULL`.

---

[1045. Customers Who Bought All Products](https://leetcode.com/problems/customers-who-bought-all-products/)
```sql
SELECT customer_id
FROM Customer
GROUP BY customer_id
HAVING COUNT(DISTINCT product_key) = (
  SELECT COUNT(product_key)
  FROM Product
)
```
---

### SQL Query Explanation

* **Query Purpose:**
  Find customers who have purchased **all products** available in the `Product` table.

* **Logic:**

  * `GROUP BY customer_id` → Aggregate purchases per customer.
  * `COUNT(DISTINCT product_key)` → Count unique products purchased by each customer.
  * `HAVING COUNT(DISTINCT product_key) = (SELECT COUNT(product_key) FROM Product)` → Include only customers who purchased **every product**.

* **Important Notes:**

  * `DISTINCT` ensures multiple purchases of the same product are counted only once.
  * The subquery `(SELECT COUNT(product_key) FROM Product)` returns the **total number of products**.
  * The result shows `customer_id`s of customers who bought all products.

---

[1731. The Number of Employees Which Report to Each Employee
](https://leetcode.com/problems/the-number-of-employees-which-report-to-each-employee/)

```sql
SELECT e1.employee_id, e1.name, COUNT(e2.employee_id) reports_count, ROUND(AVG(e2.age)) average_age 
FROM Employees e1, Employees e2
WHERE e1.employee_id = e2.reports_to
GROUP BY e1.employee_id
HAVING reports_count > 0
ORDER BY e1.employee_id
```
---

### SQL Query Explanation

* **Query Purpose:**
  For each manager, calculate the **number of direct reports** and their **average age**.

* **Logic:**

  * Self-join `Employees e1, Employees e2` → `e1` is the manager, `e2` are their reports.
  * `WHERE e1.employee_id = e2.reports_to` → Match employees to their manager.
  * `COUNT(e2.employee_id) reports_count` → Number of direct reports per manager.
  * `ROUND(AVG(e2.age)) average_age` → Average age of the reports, rounded.
  * `GROUP BY e1.employee_id` → Aggregate results per manager.
  * `HAVING reports_count > 0` → Include only managers who have at least one report.
  * `ORDER BY e1.employee_id` → Sort results by manager ID.

* **Important Notes:**

  * Self-join is necessary to relate employees with their managers.
  * `HAVING` filters based on **aggregated values**.

---

[1789. Primary Department for Each Employee
](https://leetcode.com/problems/primary-department-for-each-employee/?envType=study-plan-v2&envId=top-sql-50)
```sql
SELECT employee_id, department_id
FROM Employee 
WHERE primary_flag = 'Y'
UNION
SELECT employee_id, department_id
FROM Employee
GROUP BY employee_id
HAVING COUNT(employee_id)=1

-- OR
SELECT employee_id,department_id
FROM Employee
WHERE primary_flag = 'Y' OR employee_id IN
    (SELECT employee_id
     FROM employee
     GROUP BY employee_id
     HAVING COUNT(department_id) = 1
    )
```
---

### SQL Query Explanation

* **Query Purpose:**
  Retrieve each employee’s `employee_id` and `department_id` based on **primary assignment** or if they belong to **only one department**.

* **Logic (first query using `UNION`):**

  * First part → Select employees with `primary_flag = 'Y'`.
  * Second part → Select employees who are assigned to only **one department** (`COUNT(employee_id) = 1`).
  * `UNION` → Combine both sets of employees without duplicates.

* **Logic (second query using `IN`):**

  * Select employees where `primary_flag = 'Y'` **or** employee appears in a subquery counting only those assigned to a single department.

* **Important Notes:**

  * Both approaches give the same result.
  * `UNION` automatically removes duplicates, while `IN` checks inclusion in the subquery result.
  * Use the `IN` version for simplicity and readability.

---

[610. Triangle Judgement](https://leetcode.com/problems/triangle-judgement/)

```sql
SELECT x, y, z, 
CASE WHEN x + y > z AND x + z > y AND y + z > x THEN 'Yes'
ELSE 'No' END AS triangle
FROM Triangle
```
---

### SQL Query Explanation

* **Query Purpose:**
  Determine if the three sides `(x, y, z)` of a triangle satisfy the **triangle inequality**.

* **Logic:**

  * `CASE WHEN x + y > z AND x + z > y AND y + z > x THEN 'Yes' ELSE 'No' END AS triangle`

    * Checks the triangle inequality: the sum of any two sides must be greater than the third.
    * Returns `'Yes'` if the sides can form a triangle, `'No'` otherwise.

* **Important Notes:**

  * `CASE WHEN ... END` is used to produce conditional output in SQL.
  * The result shows the original sides along with a column indicating whether they form a valid triangle.

---


[180. Consecutive Numbers
](https://leetcode.com/problems/consecutive-numbers/)
```sql
WITH cte AS (
  SELECT id, num, 
    LEAD(num) OVER (ORDER BY id) AS next, 
    LAG(num) OVER (ORDER BY id) AS prev
  FROM Logs
) 
SELECT DISTINCT(num) AS ConsecutiveNums
FROM cte
WHERE num = next AND num = prev
```
---

### SQL Query Explanation

* **Query Purpose:**
  Identify numbers in the `Logs` table that appear **consecutively at least three times**.

* **Logic:**

  * **CTE (`cte`)** → Use window functions to get the **next** and **previous** values for each row:

    * `LEAD(num) OVER (ORDER BY id)` → next number
    * `LAG(num) OVER (ORDER BY id)` → previous number
  * Main query → Select numbers where `num = next AND num = prev` → Numbers that repeat **before and after**.
  * `DISTINCT(num)` → Remove duplicates in the result.

* **Important Notes:**

  * Window functions (`LEAD`, `LAG`) are essential to check consecutive rows without self-joining.
  * The query returns all numbers that occur in **at least three consecutive rows**.

---


[1164. Product Price at a Given Date](https://leetcode.com/problems/product-price-at-a-given-date)
```sql
SELECT product_id, new_price AS price
FROM products
WHERE (product_id, change_date) IN
(   
    SELECT product_id, MAX(change_date)
    FROM products
    WHERE change_date <= '2019-08-16'
    GROUP BY product_id
)
UNION

SELECT product_id, 10 AS price
FROM products
WHEN product_id NOT IN
(
  SELECT product_id
  FROM products
  WHERE change_date <= '2019-08-16'
)
```
---

### SQL Query Explanation

* **Query Purpose:**
  Retrieve the **latest price of each product** as of `'2019-08-16'`, and assign a default price (`10`) if no prior price exists.

* **Logic (first part):**

  * Subquery → For each `product_id`, find the **maximum `change_date`** on or before `'2019-08-16'`.
  * Outer query → Select `product_id` and the corresponding `new_price`.

* **Logic (second part):**

  * Include products that **do not have any price change before `'2019-08-16'`**.
  * Assign a **default price** of 10 to these products.

* **Important Notes:**

  * `UNION` combines both sets of products without duplicates.
  * Ensures every product has a price as of the given date, either from history or default.
  * The query can be corrected by replacing `WHEN` with `WHERE` in the second part for proper syntax.

---


[1978. Employees Whose Manager Left the Company](https://leetcode.com/problems/employees-whose-manager-left-the-company)
```sql
SELECT employee_id
FROM Employees
WHERE manager_id NOT IN (
    SELECT employee_id 
    FROM Employees
)
AND salary < 30000
ORDER BY employee_id
```
---

### SQL Query Explanation

* **Query Purpose:**
  Find employees whose **manager does not exist in the Employees table** and whose **salary is less than 30,000**.

* **Logic:**

  * `manager_id NOT IN (SELECT employee_id FROM Employees)` → Identify employees whose manager ID is not listed among existing employees.
  * `salary < 30000` → Filter for employees with salary below 30,000.
  * `ORDER BY employee_id` → Sort results by employee ID in ascending order.

* **Important Notes:**

  * Ensures that only employees with **missing or invalid managers** and **low salary** are included.
  * `NOT IN` ignores employees whose `manager_id` exists, focusing on orphaned entries.

---

[185. Department Top Three Salaries](https://leetcode.com/problems/department-top-three-salaries)
```sql
WITH RankedSalaries AS 
(SELECT 
    e.Id AS employee_id,
    e.name AS employee,
    e.salary,
    e.departmentId,
    DENSE_RANK() OVER (PARTITION BY e.departmentId ORDER BY e.salary DESC) AS salary_rank 
FROM Employee e)
SELECT d.name AS Department,
r.employee,
r.salary
FROM Department d
JOIN RankedSalaries r ON r.departmentId = d.id
WHERE r.salary_rank <=3;
```
---

### SQL Query Explanation

* **Query Purpose:**
  Retrieve the **top 3 highest-paid employees** in each department.

* **Logic:**

  * **CTE `RankedSalaries`** → Assign a **dense rank** to employees in each department based on salary (highest first).

    * `DENSE_RANK() OVER (PARTITION BY departmentId ORDER BY salary DESC)` → Rank salaries within each department.
  * Join `RankedSalaries` with `Department` to get department names.
  * Filter `WHERE salary_rank <= 3` → Select only the top 3 salaries per department.

* **Important Notes:**

  * `DENSE_RANK()` ensures ties get the same rank and the next rank is not skipped.
  * The result shows department name, employee name, and their salary for top earners.

---


[1667. Fix Names in a Table](https://leetcode.com/problems/fix-names-in-a-table)
```sql
SELECT user_id, CONCAT(UPPER(LEFT(name, 1)), LOWER(RIGHT(name, LENGTH(name)-1))) AS name
FROM Users
ORDER BY user_id
```
---

### SQL Query Explanation

* **Query Purpose:**
  Format each user’s name so that the **first letter is uppercase** and the rest are **lowercase**.

* **Logic:**

  * `UPPER(LEFT(name, 1))` → Take the first character and convert it to uppercase.
  * `LOWER(RIGHT(name, LENGTH(name)-1))` → Take the rest of the name and convert to lowercase.
  * `CONCAT(...)` → Combine the uppercase first letter with the lowercase remainder.
  * `ORDER BY user_id` → Sort results by `user_id`.

* **Important Notes:**

  * Ensures consistent capitalization of names.
  * Works even if the original `name` is in all caps or all lowercase.

---

[1527. Patients With a Condition](https://leetcode.com/problems/patients-with-a-condition)
```sql
SELECT patient_id, patient_name, conditions 
FROM patients 
WHERE conditions LIKE '% DIAB1%' 
OR conditions LIKE 'DIAB1%'
```
---

### SQL Query Explanation

* **Query Purpose:**
  Retrieve patients who have the condition **`DIAB1`** listed in their `conditions` column.

* **Logic:**

  * `conditions LIKE '% DIAB1%'` → Matches `DIAB1` appearing **anywhere after a space** in the conditions string.
  * `conditions LIKE 'DIAB1%'` → Matches `DIAB1` appearing **at the beginning** of the string.
  * `OR` → Includes patients satisfying either condition.

* **Important Notes:**

  * `%` is a wildcard representing **any sequence of characters**.
  * This ensures partial matches and avoids missing `DIAB1` not at the start.
  * The result shows `patient_id`, `patient_name`, and their `conditions`.

---

[196. Delete Duplicate Emails](https://leetcode.com/problems/delete-duplicate-emails)
```sql
DELETE p
FROM Person p, Person q
WHERE p.id > q.id
AND q.Email = p.Email
```
---

### SQL Query Explanation

* **Query Purpose:**
  Remove **duplicate records** from the `Person` table based on the `Email` column, keeping only the record with the **smallest `id`**.

* **Logic:**

  * `FROM Person p, Person q` → Self-join to compare each pair of records.
  * `p.id > q.id AND q.Email = p.Email` → Identify duplicates where `p` has a higher `id` (later entry) and the same email as `q`.
  * `DELETE p` → Delete the duplicate record `p`, keeping `q` (the first occurrence).

* **Important Notes:**

  * Ensures that only one record per email remains.
  * Self-join is used to compare rows within the same table efficiently.

---


[176. Second Highest Salary](https://leetcode.com/problems/second-highest-salary)
```sql
SELECT
(SELECT DISTINCT Salary 
FROM Employee
ORDER BY Salary DESC
LIMIT 1 OFFSET 1)
AS SecondHighestSalary

-- HINT: subquery is used to return null if there is no SecondHighestSalary
```
---

### SQL Query Explanation

* **Query Purpose:**
  Find the **second highest salary** in the `Employee` table.

* **Logic:**

  * Subquery → `SELECT DISTINCT Salary FROM Employee ORDER BY Salary DESC LIMIT 1 OFFSET 1`

    * `DISTINCT Salary` → Ensure unique salaries are considered.
    * `ORDER BY Salary DESC` → Sort salaries from highest to lowest.
    * `LIMIT 1 OFFSET 1` → Skip the highest salary and select the next one (second highest).
  * Outer query → Returns the result as `SecondHighestSalary`.

* **Important Notes:**

  * If there is **no second highest salary** (e.g., only one unique salary exists), the query returns `NULL`.
  * Using `DISTINCT` avoids counting duplicate salaries as separate entries.

---


 
[1517. Find Users With Valid E-Mails](https://leetcode.com/problems/find-users-with-valid-e-mails)
```sql
SELECT *
FROM Users
WHERE mail REGEXP '^[A-Za-z][A-Za-z0-9_\.\-]*@leetcode\\.com$'
```
---

### SQL Query Explanation

* **Query Purpose:**
  Select all users whose **email address** is valid and belongs to the `leetcode.com` domain.

* **Logic:**

  * `REGEXP` → Use a regular expression to match patterns in the `mail` column.
  * `'^[A-Za-z][A-Za-z0-9_\.\-]*@leetcode\\.com$'` → Pattern breakdown:

    * `^` → Start of the string.
    * `[A-Za-z]` → First character must be a letter.
    * `[A-Za-z0-9_\.\-]*` → Followed by letters, numbers, underscores, dots, or hyphens.
    * `@leetcode\\.com` → Must end with `@leetcode.com` (`\\.` escapes the dot).
    * `$` → End of the string.

* **Important Notes:**

  * Ensures only **valid LeetCode email addresses** are selected.
  * Regular expressions provide precise pattern matching for email validation.

---

[1204. Last Person to Fit in the Bus](https://leetcode.com/problems/last-person-to-fit-in-the-bus/)
```sql
-- 1000 kg limit
-- name of last person

WITH CTE AS (
    SELECT person_name, weight, turn, SUM(weight) 
    OVER(ORDER BY turn) AS total_weight
    FROM Queue
)
SELECT person_name
FROM cte
WHERE total_weight <=1000
ORDER BY total_weight DESC
LIMIT 1;
```
---

### SQL Query Explanation

* **Query Purpose:**
  Find the **name of the last person** who can enter a queue without exceeding a **1000 kg weight limit**.

* **Logic:**

  * **CTE (`CTE`)** → Calculate the **cumulative weight** of people in the queue:

    * `SUM(weight) OVER (ORDER BY turn) AS total_weight` → Running total based on queue order (`turn`).
  * Main query → Filter rows where `total_weight <= 1000`.
  * `ORDER BY total_weight DESC LIMIT 1` → Select the **last person** whose inclusion keeps the total weight within the limit.

* **Important Notes:**

  * Window function `SUM() OVER` allows calculation of cumulative sums **without aggregation**.
  * Ensures that the person returned is the **furthest in the queue** while respecting the weight restriction.

---

[1907. Count Salary Categories](https://leetcode.com/problems/count-salary-categories/)
```sql

SELECT 'Low Salary' AS category, SUM(IF(income<20000,1,0)) AS accounts_count 
FROM Accounts
UNION
SELECT 'Average Salary' AS category, SUM(IF(income>=20000 AND income<=50000,1,0)) AS accounts_count 
FROM Accounts
UNION
SELECT 'High Salary' AS category, SUM(IF(income>50000,1,0)) AS accounts_count 
FROM Accounts
```
---

### SQL Query Explanation

* **Query Purpose:**
  Categorize accounts based on **income levels** and count how many accounts fall into each category.

* **Logic:**

  * First query → `'Low Salary'` → Count accounts with `income < 20000`.
  * Second query → `'Average Salary'` → Count accounts with `income` between 20000 and 50000.
  * Third query → `'High Salary'` → Count accounts with `income > 50000`.
  * `SUM(IF(condition, 1, 0))` → Counts rows that satisfy the condition.
  * `UNION` → Combine all three categories into a single result set.

* **Important Notes:**

  * Each row represents a **salary category** and the **number of accounts** in that category.
  * `UNION` automatically removes duplicates; use `UNION ALL` if duplicates are possible and should be retained.

---

[626. Exchange Seats](https://leetcode.com/problems/exchange-seats/)
```sql
-- id, student
-- swap every two consecutives
-- num(students): odd? no swap for last one

SELECT id, 
CASE WHEN MOD(id,2)=0 THEN (LAG(student) OVER (ORDER BY id))
ELSE (LEAD(student, 1, student) OVER (ORDER BY id))
END AS 'Student'
FROM Seat
```
---

### SQL Query Explanation

* **Query Purpose:**
  Swap **every two consecutive students** in the seating order based on `id`.

  * If the number of students is **odd**, the last student remains in place.

* **Logic:**

  * `LAG(student) OVER (ORDER BY id)` → For even `id`s, get the previous student to swap.
  * `LEAD(student, 1, student) OVER (ORDER BY id)` → For odd `id`s, get the next student; if none, keep the same student (handles odd total).
  * `CASE WHEN MOD(id,2)=0 THEN ... ELSE ... END` → Apply swap logic based on whether `id` is even or odd.

* **Important Notes:**

  * Window functions `LAG` and `LEAD` allow accessing neighboring rows **without self-joins**.
  * The query effectively swaps students in pairs while keeping the last student unchanged if the total count is odd.

---

[1327. List the Products Ordered in a Period](https://leetcode.com/problems/list-the-products-ordered-in-a-period/)
```sql
-- name, amt
-- >= 100 units, feb 2020

SELECT p.product_name, SUM(o.unit) AS unit
FROM Products p
LEFT JOIN Orders o
ON p.product_id = o.product_id
WHERE DATE_FORMAT(order_date, '%Y-%m') = '2020-02'
GROUP BY p.product_name
HAVING SUM(o.unit) >= 100
```
---

### SQL Query Explanation

* **Query Purpose:**
  Retrieve products whose **total units ordered in February 2020** are at least 100.

* **Logic:**

  * `LEFT JOIN Orders o ON p.product_id = o.product_id` → Include all products and their orders.
  * `WHERE DATE_FORMAT(order_date, '%Y-%m') = '2020-02'` → Filter orders to February 2020.
  * `SUM(o.unit)` → Total units ordered per product.
  * `GROUP BY p.product_name` → Aggregate by product name.
  * `HAVING SUM(o.unit) >= 100` → Only include products with 100 or more units sold.

* **Important Notes:**

  * `LEFT JOIN` ensures products with no orders in February are included, but they will not satisfy the `HAVING` condition.
  * `DATE_FORMAT` is used to extract the year-month portion from `order_date`.

---

[1484. Group Sold Products By The Date](https://leetcode.com/problems/group-sold-products-by-the-date/)
```sql
SELECT sell_date, 
COUNT(DISTINCT product) AS num_sold,
GROUP_CONCAT(DISTINCT product) AS 'products' 
FROM Activities
GROUP BY sell_date
ORDER BY sell_date
```
---

### SQL Query Explanation

* **Query Purpose:**
  Summarize sales activity by date, including the **number of distinct products sold** and a **list of those products**.

* **Logic:**

  * `COUNT(DISTINCT product)` → Count the unique products sold each day.
  * `GROUP_CONCAT(DISTINCT product)` → Concatenate the names of sold products into a single string per day.
  * `GROUP BY sell_date` → Aggregate results by sale date.
  * `ORDER BY sell_date` → Sort the output chronologically.

* **Important Notes:**

  * `DISTINCT` ensures repeated products on the same day are counted only once.
  * The result shows daily sales summary with product count and names.

---


[1341. Movie Rating](https://leetcode.com/problems/movie-rating/)

```sql
(SELECT name AS results
FROM Users u
LEFT JOIN MovieRating mr
ON u.user_id = mr.user_id
GROUP BY name
ORDER BY COUNT(rating) DESC, name ASC
LIMIT 1)

UNION ALL

(SELECT title
FROM Movies m
LEFT JOIN MovieRating mr
ON m.movie_id = mr.movie_id
WHERE DATE_FORMAT(created_at, '%Y-%m') = '2020-02'
GROUP BY title
ORDER BY AVG(rating) DESC, title ASC
LIMIT 1 
)
```
---

### SQL Query Explanation

* **Query Purpose:**
  Retrieve:

  1. The **user with the most movie ratings**.
  2. The **highest-rated movie** in February 2020.

* **Logic (first query):**

  * `LEFT JOIN MovieRating mr ON u.user_id = mr.user_id` → Connect users with their ratings.
  * `GROUP BY name` → Aggregate ratings by user.
  * `ORDER BY COUNT(rating) DESC, name ASC LIMIT 1` → Select the user with the most ratings; tie-break by name.

* **Logic (second query):**

  * `LEFT JOIN MovieRating mr ON m.movie_id = mr.movie_id` → Connect movies with ratings.
  * `WHERE DATE_FORMAT(created_at, '%Y-%m') = '2020-02'` → Filter ratings for February 2020.
  * `GROUP BY title` → Aggregate ratings by movie title.
  * `ORDER BY AVG(rating) DESC, title ASC LIMIT 1` → Select the movie with the highest average rating; tie-break by title.

* **`UNION ALL`** → Combine both results into a single result set with **two rows**: one for the top user, one for the top movie.

* **Important Notes:**

  * `LEFT JOIN` ensures users or movies without ratings are included but may not appear in top results.
  * `UNION ALL` preserves both rows even if values are identical.

---


[1321. Restaurant Growth](https://leetcode.com/problems/restaurant-growth/)
```sql
-- pay: last 7 days (today inclusive) - avg.amt (round, 2)

SELECT visited_on, amount, ROUND(amount/7, 2) AS average_amount
FROM (
    SELECT DISTINCT visited_on,
    SUM(amount) OVER(ORDER BY visited_on RANGE BETWEEN INTERVAL 6 DAY PRECEDING AND CURRENT ROW) AS amount,
    MIN(visited_on) OVER() day_1
    FROM Customer
) t
WHERE visited_on >= day_1+6;
```
---

### SQL Query Explanation

* **Query Purpose:**
  Calculate the **7-day rolling average amount** for each day in the dataset, considering the **last 7 days** (today inclusive).

* **Logic:**

  * **Window function:**

    * `SUM(amount) OVER(ORDER BY visited_on RANGE BETWEEN INTERVAL 6 DAY PRECEDING AND CURRENT ROW)` → Calculate the sum of `amount` over the current day and previous 6 days.
  * `ROUND(amount/7, 2)` → Compute the 7-day average and round to 2 decimal places.
  * `MIN(visited_on) OVER() day_1` → Identify the first day in the dataset to determine the starting point for calculation.
  * `WHERE visited_on >= day_1 + 6` → Include only days where a full 7-day window exists.

* **Important Notes:**

  * This approach uses **window functions** to calculate moving averages without self-joins.
  * Ensures that averages are computed only when **enough days (7)** are available.

---


[602. Friend Requests II: Who Has the Most Friends](https://leetcode.com/problems/friend-requests-ii-who-has-the-most-friends)

```sql
-- `union` selects only unique vals, so we use `union all` here

WITH CTE AS (
    SELECT requester_id AS id FROM RequestAccepted
    UNION ALL
    SELECT accepter_id AS id FROM RequestAccepted
)

SELECT id, COUNT(id) AS num
FROM CTE
GROUP BY id
ORDER BY num DESC
LIMIT 1
```
---

### SQL Query Explanation

* **Query Purpose:**
  Identify the user who is involved in the **most requests**, either as a requester or accepter.

* **Logic:**

  * **CTE (`CTE`)** → Combine all `requester_id`s and `accepter_id`s into a single column `id`:

    * `UNION ALL` ensures **all occurrences** are counted, unlike `UNION` which removes duplicates.
  * `COUNT(id)` → Count total requests for each user.
  * `GROUP BY id` → Aggregate counts per user.
  * `ORDER BY num DESC LIMIT 1` → Select the user with the highest request count.

* **Important Notes:**

  * Using `UNION ALL` is crucial to accurately count **all appearances**, not just unique ones.
  * The result shows the `id` of the most active user and their total involvement.

---

[585. Investments in 2016](https://leetcode.com/problems/investments-in-2016)
```sql
SELECT
    ROUND(SUM(tiv_2016),2) AS tiv_2016
FROM insurance
WHERE tiv_2015 IN (SELECT tiv_2015 FROM insurance GROUP BY tiv_2015 HAVING COUNT(*) > 1)
AND (lat,lon) IN (SELECT lat,lon FROM insurance GROUP BY lat,lon HAVING COUNT(*) = 1)
```
---

### SQL Query Explanation

* **Query Purpose:**
  Calculate the **total `tiv_2016`** for records that meet two conditions:

  1. `tiv_2015` value is **duplicated** (appears more than once).
  2. `(lat, lon)` combination is **unique** (appears exactly once).

* **Logic:**

  * `tiv_2015 IN (SELECT tiv_2015 ... HAVING COUNT(*) > 1)` → Include only rows with **duplicate `tiv_2015` values**.
  * `(lat, lon) IN (SELECT lat, lon ... HAVING COUNT(*) = 1)` → Include only rows with **unique coordinates**.
  * `SUM(tiv_2016)` → Sum the `tiv_2016` values of the filtered rows.
  * `ROUND(..., 2)` → Round the total to 2 decimal places.

* **Important Notes:**

  * Combines filtering by **duplicate and unique criteria** in the same query.
  * Useful for identifying and aggregating specific subsets of insurance data based on value and location uniqueness.

---
DONE SQL !!
---
---

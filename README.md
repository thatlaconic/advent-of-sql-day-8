# [The Great North Pole Bureaucracy Bust of 2024](https://adventofsql.com/challenges/8)

## Description
Santa's Workshop facing a bureaucratic crisis after expanding to a bloated organizational size of 100,000 employees. Jingle, the Systems Administrator Elf, alerts Santa to the inefficiency, revealing absurd layers of middle management and redundant departments, such as 17 groups dedicated to optimizing hot cocoa without making any. Mrs. Claus discovers that the organization has become so entangled that no one is making toys anymore.

Santa decides to take action, vowing to unravel the mess using a database-driven approach to map the entire structure and eliminate inefficiencies. Dubbed "Operation Database Delight," this effort aims to restore the Workshop to its former productive glory. The tale ends with Santa diving into the data, ready to expose unnecessary positions with the help of recursive queries, reflecting on the irony of his situation.

## Challenge
[Download Challenge data](https://github.com/thatlaconic/advent-of-sql-day-8/blob/main/advent_of_sql_day_8.sql)

We want to find out how many managers the most over-managed employee has (levels deep).

To do this, you're going to need to go through all the employees and find out who their manager is, and who their manger is, and who their manger is... you see where this is going

* A report needs to be produce to calculate this management depth for all employees
* Order it by the number of levels in descending order.
  
## Dataset
This dataset contains 1 tables with 4 columns and 100000 rows. 
### Using PostgreSQL
**input**
```sql
SELECT *
FROM staff ;
```
**output**

![](https://github.com/thatlaconic/advent-of-sql-day-8/blob/main/staff.PNG)

### Solution
[Download Solution Code](https://github.com/thatlaconic/advent-of-sql-day-8/blob/main/advent_answer_day8.sql)

**input**
```sql
WITH RECURSIVE HierarchyCTE AS (
    SELECT 
        staff_id,
        manager_id,
        staff_name,
        1 AS level
    FROM staff
    WHERE manager_id IS NULL
    UNION ALL
    SELECT 
        t.staff_id,
        t.manager_id,
        t.staff_name,
        h.level + 1
    FROM  staff t
    JOIN HierarchyCTE h ON t.manager_id = h.staff_id
)
SELECT 
   staff_id,
   staff_name,
   manager_id,
   Level
FROM HierarchyCTE
ORDER BY level DESC, staff_id DESC;

```
**output**

![](https://github.com/thatlaconic/advent-of-sql-day-8/blob/main/d8.PNG)




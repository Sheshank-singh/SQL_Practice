# Second Highest Salary

## Problem

Find the **second-highest distinct salary** from the `Employee` table. If a second-highest salary does not exist, return `NULL`.

## Solution 1 — `DISTINCT + ORDER BY + LIMIT + OFFSET`

```sql
SELECT (
    SELECT DISTINCT salary
    FROM Employee
    ORDER BY salary DESC
    LIMIT 1 OFFSET 1
) AS SecondHighestSalary;
```

## Solution Concept

Think of the problem as:

```text
All salaries
    ↓
Remove duplicates
    ↓
Sort highest → lowest
    ↓
Skip the highest salary
    ↓
Take the next salary
```

Example:

```text
300
300
200
100
```

After `DISTINCT`:

```text
300
200
100
```

After `ORDER BY salary DESC`:

```text
300
200
100
```

`OFFSET 1` skips `300`, and `LIMIT 1` returns `200`.

If there is no second salary, the scalar subquery returns `NULL`.

---

## New Concepts Learned

### 1. `DISTINCT`

Removes duplicate values.

```sql
SELECT DISTINCT salary
FROM Employee;
```

Example:

```text
300
300
200
100
```

becomes:

```text
300
200
100
```

---

### 2. `ORDER BY ... DESC`

Sorts values from highest to lowest.

```sql
ORDER BY salary DESC
```

* `ASC` → ascending
* `DESC` → descending

---

### 3. `LIMIT`

Controls how many rows are returned.

```sql
LIMIT 1
```

means return at most **one row**.

---

### 4. `OFFSET`

Skips a specified number of rows.

```sql
OFFSET 1
```

means skip the first row.

Therefore:

```sql
LIMIT 1 OFFSET 1
```

means:

> Skip the highest salary and return the next one.

---

### 5. Scalar Subquery

A query inside another query that returns a **single value**.

```sql
SELECT (
    SELECT DISTINCT salary
    FROM Employee
    ORDER BY salary DESC
    LIMIT 1 OFFSET 1
) AS SecondHighestSalary;
```

The inner query finds the second-highest salary, and the outer query gives it the required column name.

---

## Key Assessment Pattern

When you see:

> **Nth highest distinct value**

Think:

```text
DISTINCT
   ↓
ORDER BY DESC
   ↓
LIMIT / OFFSET
```

For this problem:

```text
2nd highest
→ OFFSET 1
→ LIMIT 1
```

### Important

`OFFSET` starts counting from **0**:

```text
OFFSET 0 → highest
OFFSET 1 → second highest
OFFSET 2 → third highest
```

---

## Alternative Solution

Another common approach is:

```sql
SELECT MAX(salary) AS SecondHighestSalary
FROM Employee
WHERE salary < (
    SELECT MAX(salary)
    FROM Employee
);
```

This works because the second-highest salary is the **maximum salary that is less than the highest salary**.

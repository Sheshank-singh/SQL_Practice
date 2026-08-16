# Nth Highest Salary

## Problem

Find the **Nth highest distinct salary** from the `Employee` table. If fewer than `N` distinct salaries exist, return `NULL`.

---

## Solution

```sql
CREATE FUNCTION getNthHighestSalary(N INT) RETURNS INT
BEGIN
    SET N = N - 1;

    RETURN (
        SELECT DISTINCT salary
        FROM Employee
        ORDER BY salary DESC
        LIMIT 1 OFFSET N
    );
END
```

---

## Solution Concept

The idea is to convert the **Nth highest** problem into an `OFFSET` problem.

```text
Nth highest
    ↓
Remove duplicate salaries
    ↓
Sort highest → lowest
    ↓
Skip N - 1 salaries
    ↓
Return 1 salary
```

For example, if:

```text
N = 3
```

and salaries are:

```text
500
500
400
300
200
```

After `DISTINCT`:

```text
500
400
300
200
```

We need to skip:

```text
N - 1 = 2
```

salaries:

```text
500 ← skip
400 ← skip
300 ← return
```

Therefore:

```sql
LIMIT 1 OFFSET 2
```

returns `300`.

---

## New Concepts Learned

### 1. `CREATE FUNCTION`

Creates a reusable SQL function.

```sql
CREATE FUNCTION getNthHighestSalary(N INT)
RETURNS INT
```

* `getNthHighestSalary` → function name
* `N INT` → input parameter
* `RETURNS INT` → function returns an integer

---

### 2. Function Parameter

```sql
N INT
```

`N` represents which highest salary we want.

```text
getNthHighestSalary(2) → 2nd highest
getNthHighestSalary(3) → 3rd highest
getNthHighestSalary(5) → 5th highest
```

---

### 3. `BEGIN ... END`

Defines the body of the function.

```sql
BEGIN
    SET N = N - 1;
    RETURN (...);
END
```

Statements such as `SET` and `RETURN` must be inside the `BEGIN ... END` block.

---

### 4. `SET`

Used to modify a variable or parameter.

```sql
SET N = N - 1;
```

This converts the ranking into the required offset:

```text
N = 1 → OFFSET 0
N = 2 → OFFSET 1
N = 3 → OFFSET 2
```

---

### 5. `DISTINCT`

Removes duplicate salaries.

```sql
SELECT DISTINCT salary
FROM Employee;
```

This is necessary because the problem asks for the **Nth highest distinct salary**.

---

### 6. `ORDER BY ... DESC`

Sorts salaries from highest to lowest.

```sql
ORDER BY salary DESC
```

---

### 7. `LIMIT`

Controls the number of rows returned.

```sql
LIMIT 1
```

means return only one salary.

---

### 8. `OFFSET`

Skips rows before returning the result.

```sql
LIMIT 1 OFFSET N
```

After `SET N = N - 1`, this effectively skips the salaries before the required rank.

---

### 9. `RETURN`

Returns the result from the function.

```sql
RETURN (
    SELECT ...
);
```

If fewer than `N` distinct salaries exist, the query produces `NULL`.

---

## Key Pattern

For an **Nth highest distinct value**:

```text
DISTINCT
   ↓
ORDER BY DESC
   ↓
OFFSET N - 1
   ↓
LIMIT 1
```

### Important Formula

```text
OFFSET = N - 1
```

Examples:

```text
2nd highest → OFFSET 1
3rd highest → OFFSET 2
4th highest → OFFSET 3
Nth highest → OFFSET N - 1
```

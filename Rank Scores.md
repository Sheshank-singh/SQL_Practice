# Rank Scores

## Problem

Rank all scores from **highest to lowest**.

Rules:

* Same scores receive the same rank.
* After a tie, ranking continues with the **next consecutive integer**.
* There should be **no gaps** between ranks.
* Return the result ordered by score in descending order.

---

## Solution

```sql
SELECT
    score,
    DENSE_RANK() OVER (
        ORDER BY score DESC
    ) AS `rank`
FROM Scores
ORDER BY score DESC;
```

---

## Solution Concept

The key requirement is:

> **Same score → same rank, and no gaps after ties.**

Therefore, use:

```text
DENSE_RANK()
```

Example:

```text
Score     Rank
------    ----
4.00        1
4.00        1
3.85        2
3.65        3
3.65        3
3.50        4
```

---

## New Concepts Learned

### 1. Window Function

A window function performs a calculation across multiple rows **without combining/removing the original rows**.

Example:

```sql
DENSE_RANK() OVER (...)
```

Unlike `GROUP BY`, the original rows are preserved.

---

### 2. `DENSE_RANK()`

Assigns the same rank to tied values and **does not leave gaps**.

```sql
DENSE_RANK() OVER (
    ORDER BY score DESC
)
```

Highest score gets rank `1`.

---

### 3. `OVER()`

Defines how the window function should calculate its result.

```sql
DENSE_RANK() OVER (
    ORDER BY score DESC
)
```

Here, `ORDER BY score DESC` tells SQL to rank the highest score first.

---

### 4. `RANK()` vs `DENSE_RANK()` vs `ROW_NUMBER()`

For:

```text
100
100
90
80
```

| Function       | Result     |
| -------------- | ---------- |
| `ROW_NUMBER()` | 1, 2, 3, 4 |
| `RANK()`       | 1, 1, 3, 4 |
| `DENSE_RANK()` | 1, 1, 2, 3 |

### Remember

```text
ROW_NUMBER()
→ Every row gets a unique number

RANK()
→ Ties share rank + gaps exist

DENSE_RANK()
→ Ties share rank + no gaps
```

---

## Important Assessment Pattern

When the question says:

> **Same values should have the same rank and there should be no gaps**

Think immediately:

```text
DENSE_RANK()
```

When it says:

> **Same values should have the same rank but gaps are allowed**

Use:

```text
RANK()
```

When every row needs a unique sequential number:

```text
ROW_NUMBER()
```

---

## Important `ORDER BY` Concept

There are two `ORDER BY` clauses in the solution:

```sql
DENSE_RANK() OVER (
    ORDER BY score DESC
)
```

This determines **how the rank is calculated**.

```sql
ORDER BY score DESC;
```

This determines **how the final result is displayed**.

They have different purposes.

---

## Key Takeaway

For ranking problems:

```text
Tie + no gaps
      ↓
DENSE_RANK()

Tie + gaps
      ↓
RANK()

Unique row number
      ↓
ROW_NUMBER()
```

This is an important foundation for advanced SQL problems such as **top-N per group, highest salary per department, and ranking within departments**.

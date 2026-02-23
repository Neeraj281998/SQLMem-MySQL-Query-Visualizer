# 🗄 SQLMem — MySQL Query Visualizer

<div align="center">

![SQLMem Banner](https://img.shields.io/badge/SQLMem-v2.0-00d4ff?style=for-the-badge&logo=mysql&logoColor=white)
![HTML](https://img.shields.io/badge/HTML-Single%20File-orange?style=for-the-badge&logo=html5&logoColor=white)
![CSS](https://img.shields.io/badge/CSS-Vanilla-blue?style=for-the-badge&logo=css3&logoColor=white)
![JavaScript](https://img.shields.io/badge/JavaScript-Vanilla-yellow?style=for-the-badge&logo=javascript&logoColor=black)
![License](https://img.shields.io/badge/License-MIT-green?style=for-the-badge)

**An interactive, animated MySQL query visualizer built for education.**  
Watch your SQL queries execute step-by-step with live table animations, FK relationship diagrams, JOIN beam effects, and a real SQL execution order explainer.
<!-- 
[🚀 Live Demo](#) · [📖 Documentation](#table-of-contents) · [🐛 Report Bug](#) · [✨ Request Feature](#)
-->
</div>

---

## 📸 Preview

```
┌─────────────────────────────────────────────────────────────────────────┐
│  🗄 SQLMem  v2 — MySQL Visualizer   SCHEMA | DML | SELECT/WHERE | JOIN  │
├──────────────┬──────────────────────────────────────┬───────────────────┤
│  SQL EDITOR  │                                      │  RESULT           │
│              │   ┌─departments──┐                   │                   │
│  CREATE TABLE│   │ 🔑 id   INT  │◄─────────────────│  id  name  salary │
│  employees...│   │ · name  VAR  │  employees.dept_id│  1   Alice  90000 │
│              │   │ · budget DEC │                   │  2   Bob    75000 │
│  ▶ EXECUTE   │   └──────────────┘                   │  ✓ 5 rows         │
│              │   ┌─employees────┐                   ├───────────────────┤
│  PRESETS     │   │ 🔑 id   INT  │                   │  EXEC ORDER       │
│  🏢 Company  │   │ · name  VAR  │                   │  1. FROM  ✓       │
│  🛒 E-Comm   │   │ 🔗 dept_id   │                   │  2. WHERE ●       │
│  🎓 School   │   │ · salary DEC │                   │  3. SELECT...     │
└──────────────┴──────────────────────────────────────┴───────────────────┘
```

---

## Table of Contents

- [Features](#-features)
- [Getting Started](#-getting-started)
- [Tabs & Panels](#-tabs--panels)
  - [Schema Tab](#1-schema-tab)
  - [DML Tab](#2-dml-tab)
  - [SELECT / WHERE Tab](#3-select--where-tab)
  - [JOIN Tab](#4-join-tab)
  - [GROUP BY Tab](#5-group-by-tab)
  - [Exec Order](#6-exec-order-visualizer)
- [Example Queries](#-example-queries)
  - [SELECT & Filtering](#select--filtering)
  - [DML Operations](#dml-operations)
  - [JOIN Queries](#join-queries)
  - [Aggregations](#aggregations)
- [Preset Schemas](#-preset-schemas)
- [Architecture](#-architecture)

---

## ✨ Features

| Feature | Description |
|---|---|
| 📐 **Schema Visualizer** | Renders `CREATE TABLE` statements as draggable cards with animated FK arrows |
| 🔗 **FK Relationship Arrows** | Dashed animated SVG lines connecting foreign key → primary key columns |
| ✏️ **DML Animations** | Row-by-row INSERT / UPDATE / DELETE / TRUNCATE with live preview highlights |
| 🔍 **SELECT / WHERE** | Animated row filtering with LIKE, IN, BETWEEN, IS NULL, ORDER BY, LIMIT, OFFSET |
| ⚡ **JOIN Animator** | INNER / LEFT / RIGHT / FULL JOIN with scanning beam, moving dot, and Venn diagram |
| 📊 **GROUP BY + Aggregates** | SUM, AVG, COUNT, MIN, MAX with animated proportional bar charts per group |
| ⚙️ **Execution Order Explainer** | Side-by-side "you write vs MySQL executes" order with live step highlighting |
| 🎯 **NULL Handling** | Visual NULL propagation — NULLs shown in red italic throughout all operations |
| 💻 **Zero Dependencies** | Single HTML file — no npm, no build step, no server needed |

---

## 🚀 Getting Started

### Option 1 — Download & Open

```bash
# Clone the repository
git clone https://github.com/yourusername/sqlmem.git

# Open directly in browser — no server needed
open sqlmem.html
# or on Windows:
start sqlmem.html
```

### Option 2 — Use a Preset

1. Open `sqlmem.html` in any modern browser
2. Click **🏢 Company** (or any preset) in the left panel
3. The schema renders instantly with tables, FK arrows, and data previews
4. Switch to the **JOIN** tab and click **⚡ ANIMATE JOIN** to see it in action

### Browser Compatibility

| Browser | Supported |
|---|---|
| Chrome 90+ | ✅ |
| Firefox 88+ | ✅ |
| Safari 14+ | ✅ |
| Edge 90+ | ✅ |

---

## 🗂 Tabs & Panels

### 1. Schema Tab

Write raw `CREATE TABLE` and `INSERT INTO` SQL. The parser supports:

- `PRIMARY KEY` column constraints
- `FOREIGN KEY (col) REFERENCES table(col)` relationships
- Column types: `INT`, `VARCHAR(n)`, `DECIMAL`, `BOOLEAN`, `DATE`, etc.
- Multi-row `INSERT INTO ... VALUES (row1), (row2), ...`
- `NULL` values in INSERT data

**Example input:**

```sql
CREATE TABLE departments (
  id   INT PRIMARY KEY,
  name VARCHAR(50),
  budget DECIMAL
);

CREATE TABLE employees (
  id         INT PRIMARY KEY,
  name       VARCHAR(50),
  dept_id    INT,
  salary     DECIMAL,
  FOREIGN KEY (dept_id) REFERENCES departments(id)
);

INSERT INTO departments VALUES
  (1, 'Engineering', 500000),
  (2, 'Marketing',   200000),
  (3, 'Sales',       300000);

INSERT INTO employees VALUES
  (1, 'Alice', 1, 90000),
  (2, 'Bob',   1, 75000),
  (3, 'Carol', 2, 70000),
  (4, 'Dave',  NULL, 65000);
```

**What you'll see:**
- Two draggable table cards rendered on the canvas
- An animated dashed arrow from `employees.dept_id → departments.id`
- Data preview rows visible inside each card (click the row-count badge to toggle)
- Dave's `dept_id` shown as `NULL` in red italic

---

### 2. DML Tab

Animate data manipulation operations on any loaded table.

#### INSERT

```
Operation : INSERT
Table     : employees
Values    : 10,'Grace',1,95000
            11,'Hank',3,72000
```

Each row **flashes green** and appears live in the card preview with an animated entry effect.

#### UPDATE

```
Operation : UPDATE
Table     : employees
SET       : salary=99000
WHERE     : id=1
```

The scanner highlights every row one-by-one. Matching rows flash **yellow** with the updated value applied in place.

#### DELETE

```
Operation : DELETE
Table     : employees
WHERE     : salary < 70000
```

Matching rows show a **red strikethrough** animation before being removed. Row count decrements in real time.

#### TRUNCATE

```
Operation : TRUNCATE
Table     : managers
```

All rows disappear instantly. A comparison card appears explaining:

> **TRUNCATE** drops all rows without a WHERE clause — non-rollbackable, resets AUTO_INCREMENT, much faster than DELETE.  
> **DELETE** supports WHERE filtering, fires triggers, and is transaction-safe.

---

### 3. SELECT / WHERE Tab

Build and animate a full SELECT query visually.

| Control | Description |
|---|---|
| **Table** | Source table to query |
| **SELECT Columns** | `*` or comma-separated column names |
| **WHERE Clause** | Free-text condition (see filter types below) |
| **Filter Type Buttons** | Inject LIKE / IN / BETWEEN / IS NULL / IS NOT NULL templates |
| **ORDER BY** | Column name + ASC or DESC direction |
| **LIMIT / OFFSET** | Pagination controls |

**Filter examples:**

```sql
-- Simple comparison
salary > 70000

-- BETWEEN range
salary BETWEEN 65000 AND 85000

-- IN list
dept_id IN (1, 2, 3)

-- String pattern
name LIKE 'A%'

-- NULL check
dept_id IS NULL

-- Combined
salary > 60000 AND dept_id IS NOT NULL
```

**Animation flow:**

```
FROM  →  [loads table rows into canvas]
WHERE →  [scans each row, filtered rows get strikethrough]
SELECT → [picks requested columns]
ORDER  → [reorders result set]
LIMIT  → [clips to N rows from offset]
```

The result panel shows a stats card:

```
┌────────┬──────────┬──────────┐
│  6     │    2     │    4     │
│ total  │ filtered │ returned │
└────────┴──────────┴──────────┘
```

---

### 4. JOIN Tab

Animate a JOIN between any two loaded tables.

| Control | Description |
|---|---|
| **JOIN TYPE** | INNER / LEFT / RIGHT / FULL |
| **FROM Table** | The driving (left) table |
| **JOIN Table** | The joined (right) table |
| **ON Condition** | e.g. `employees.dept_id = departments.id` (auto-detected from FK) |

**Visual effects during animation:**

- Left table card glows **orange** (source)
- Right table card glows **green** (target)
- An animated beam with a **moving dot** travels between the join columns
- Each row in the left table scans right table row-by-row
- Matching rows flash green on both sides
- Result streams into the right panel row-by-row with a Venn diagram

**JOIN type behaviors:**

| Type | Included rows | NULL side |
|---|---|---|
| `INNER` | Only matched rows | — |
| `LEFT` | All left + matched right | Right columns = NULL for unmatched |
| `RIGHT` | Matched left + all right | Left columns = NULL for unmatched |
| `FULL` | Everything | NULLs on whichever side has no match |

**ON condition auto-detection:**  
If the two selected tables have a defined `FOREIGN KEY` relationship, the ON condition fills in automatically. You can always override it manually.

---

### 5. GROUP BY Tab

Animate aggregation queries with a live bar chart.

| Control | Description |
|---|---|
| **Table** | Source table |
| **GROUP BY** | Column to group on (or none for a grand total) |
| **Function** | COUNT / SUM / AVG / MIN / MAX |
| **Column** | Target column for SUM/AVG/MIN/MAX |
| **HAVING** | Post-aggregation filter (e.g. `COUNT(*) > 1`) |
| **Show NULL handling** | Badges on groups that had NULLs excluded |

**Example — salary distribution by department:**

```
Table     : employees
GROUP BY  : dept_id
Function  : AVG
Column    : salary
HAVING    : (empty)
```

Result animates as proportional bars:

```
dept 1  ████████████████  82,500
dept 2  █████████████     67,500
dept 3  ████████████████  80,000
NULL    ██████            65,000
```

**NULL handling note:**  
`AVG`, `SUM`, `MIN`, `MAX` silently skip NULL values — just like real MySQL. Enable **Show NULL handling** to see a warning badge on groups where NULLs were excluded from the calculation.

---

### 6. Exec Order Visualizer

Click **⚙ EXEC ORDER** in the header at any time.

**The #1 conceptual mistake in SQL:** Most beginners think SQL executes in the order you write it. It doesn't.

```
You write:          MySQL executes:
─────────────       ─────────────────
1. SELECT     →     1. FROM
2. FROM       →     2. WHERE
3. WHERE      →     3. GROUP BY
4. GROUP BY   →     4. HAVING
5. HAVING     →     5. SELECT  ← (aliases defined here!)
6. ORDER BY   →     6. DISTINCT
7. LIMIT      →     7. ORDER BY
                    8. LIMIT / OFFSET
```

**Common mistakes this explains:**

- ❌ `WHERE total > 0` — fails if `total` is a SELECT alias (WHERE runs before SELECT)
- ✅ `HAVING total > 0` — works because HAVING runs after SELECT
- ❌ `WHERE COUNT(*) > 1` — COUNT is an aggregate, can't use in WHERE
- ✅ `HAVING COUNT(*) > 1` — correct placement
- `ORDER BY` can use SELECT aliases because it runs after SELECT

The right panel **EXEC ORDER** tab also animates live during query execution — each step lights up as active (shimmer effect), turns green when done, and remaining steps stay dimmed.

---

## 💡 Example Queries

### SELECT & Filtering

**Top 3 earners:**
```
Tab      : SELECT/WHERE
Table    : employees
Columns  : name, salary
ORDER BY : salary  [DESC]
LIMIT    : 3
```

**Employees without a department:**
```
Tab   : SELECT/WHERE
Table : employees
WHERE : dept_id IS NULL
```

**Employees in specific departments:**
```
Tab   : SELECT/WHERE
Table : employees
WHERE : dept_id IN (1,3)
```

**Mid-range salaries:**
```
Tab   : SELECT/WHERE
Table : employees
WHERE : salary BETWEEN 70000 AND 85000
```

**Name search:**
```
Tab   : SELECT/WHERE
Table : employees
WHERE : name LIKE '%e%'
```

**Pagination — page 2 of employees by salary:**
```
Tab      : SELECT/WHERE
Table    : employees
Columns  : name, salary
ORDER BY : salary [DESC]
LIMIT    : 2
OFFSET   : 2
```

---

### DML Operations

**Add two new employees:**
```
Tab       : DML
Operation : INSERT
Table     : employees
Values    : 10,'Grace',1,2,95000
            11,'Hank',3,NULL,72000
```

**Give everyone a raise:**
```
Tab       : DML
Operation : UPDATE
Table     : employees
SET       : salary=salary+5000
WHERE     : (empty — updates all rows)
```

**Promote one employee:**
```
Tab       : DML
Operation : UPDATE
Table     : employees
SET       : salary=100000
WHERE     : name='Alice'
```

**Fire the lowest earner:**
```
Tab       : DML
Operation : DELETE
Table     : employees
WHERE     : salary < 65000
```

**Clear a junction table:**
```
Tab       : DML
Operation : TRUNCATE
Table     : managers
```

---

### JOIN Queries

**Who works in which department? (INNER):**
```
Tab       : JOIN
Type      : INNER
FROM      : employees
JOIN      : departments
ON        : employees.dept_id = departments.id
```
→ 5 rows — Dave (NULL dept) excluded

**All employees including unassigned (LEFT):**
```
Tab       : JOIN
Type      : LEFT
FROM      : employees
JOIN      : departments
ON        : employees.dept_id = departments.id
```
→ 6 rows — Dave appears with `departments.*` = NULL

**All departments including empty ones (RIGHT):**
```
Tab       : JOIN
Type      : RIGHT
FROM      : employees
JOIN      : departments
ON        : employees.dept_id = departments.id
```
→ Departments with no employees show `employees.*` = NULL

**Everything — unmatched on both sides (FULL):**
```
Tab       : JOIN
Type      : FULL
FROM      : employees
JOIN      : departments
ON        : employees.dept_id = departments.id
```
→ Maximum row count — Dave + any empty departments all appear

---

### Aggregations

**Headcount per department:**
```
Tab      : GROUP BY
Table    : employees
GROUP BY : dept_id
Function : COUNT
```

**Total salary spend per department:**
```
Tab      : GROUP BY
Table    : employees
GROUP BY : dept_id
Function : SUM
Column   : salary
```

**Average salary — only departments with 2+ people:**
```
Tab      : GROUP BY
Table    : employees
GROUP BY : dept_id
Function : AVG
Column   : salary
HAVING   : COUNT(*) > 1
```

**Highest score per course (School preset):**
```
Tab      : GROUP BY
Table    : enrollments
GROUP BY : course_id
Function : MAX
Column   : score
```

**Grand total of all salaries (no GROUP BY):**
```
Tab      : GROUP BY
Table    : employees
GROUP BY : (none)
Function : SUM
Column   : salary
```

**NULL exclusion demo:**
```
Tab             : GROUP BY
Table           : employees
GROUP BY        : dept_id
Function        : AVG
Column          : salary
☑ Show NULL handling  (check this box)
```
→ The NULL group (Dave) shows a warning badge — his salary IS included but his `dept_id` being NULL makes him his own group

---

## 📦 Preset Schemas

Three schemas are built in and load with one click:

### 🏢 Company

```
departments  ←── employees  ←── managers
(id, name,       (id, name,       (id,
 budget)          dept_id FK,      employee_id FK,
                  manager_id,      level)
                  salary)
```

Best for: JOIN animations, GROUP BY salary, DML demos

### 🛒 E-Commerce

```
users  ←── orders  ←── order_items  ──→  products
                         (junction)
```

Best for: Multi-table JOINs, chained FK arrows, aggregate totals per user

### 🎓 School

```
students  ←── enrollments  ──→  courses
               (junction with score)
```

Best for: Many-to-many relationships, GROUP BY with MIN/MAX scores, HAVING filters

---

## 🏗 Architecture

SQLMem is a **single HTML file** (~1,500 lines) with no external dependencies.

```
sqlmem.html
├── <style>          CSS — layout, card styles, animations (lines 1–500)
├── <body>           HTML structure — panels, tabs, canvas, SVG layer
└── <script>
    ├── State        tables{}, relations[], selected flags
    ├── Parser       parseSQL() → CREATE TABLE + INSERT support
    │   ├── splitOutsideParens()   comma-split respecting VARCHAR(50)
    │   ├── extractRowStrings()    paren-depth row extraction
    │   └── parseValues()         typed value parser (NULL, strings, numbers)
    ├── Renderer     buildTableCard(), renderConnections(), SVG FK lines
    ├── Draggable    makeDraggable() — mousedown/mousemove/mouseup
    ├── JOIN         animateJoin() — async row scanning + beam + venn
    ├── SELECT       animateSelect() — WHERE eval + ORDER + LIMIT
    ├── DML          animateDML() — INSERT / UPDATE / DELETE / TRUNCATE
    ├── AGG          animateAgg() — GROUP BY + aggregate + HAVING + bars
    ├── ExecOrder    buildExecOrderUI() + highlightExecStep() + modal
    └── Utils        sleep(), log(), evalWhere(), setStatus()
```

### Key Design Decisions

**Self-contained parser** — No SQL parsing library. A lightweight regex + character-scan parser handles the subset of MySQL DDL and DML needed for visualization. It correctly handles:
- Commas inside `VARCHAR(50)` and `DECIMAL(10,2)` via `splitOutsideParens()`
- Multi-row INSERT values via `extractRowStrings()` (paren-depth tracking)
- NULL, quoted strings, integers, and decimals via `parseValues()`

**Async animations** — All operations use `async/await` with a `sleep(ms)` helper to create frame-by-frame scanning effects without blocking the UI thread.

**Live SVG layer** — FK arrows and JOIN beams are drawn on an absolutely-positioned `<svg>` that sits above the canvas. They redraw on every drag event via `renderConnections()`.

---



### Guidelines

- Keep it a **single HTML file** — no build tools, no npm
- Test all 3 presets after any parser changes
- Animations should use `async/await` + `sleep()` — not `setTimeout` callbacks
- New features should add a tab or extend an existing one — don't bloat the header

---



## 🙏 Acknowledgements

<!-- - Inspired by [JavaMem](https://github.com/yourusername/javamem) — Java memory visualizer -->
- Font: [JetBrains Mono](https://www.jetbrains.com/legalnotices/font/) + [Syne](https://fonts.google.com/specimen/Syne)
- Color palette inspired by terminal dark themes

---

<div align="center">

**Built with ❤️ for SQL learners everywhere**

If SQLMem helped you understand a JOIN or finally get GROUP BY — leave a ⭐

</div>

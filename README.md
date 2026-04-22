# 💉 SQL Injection Demo — College Mini Project

> An interactive, browser-based demonstration of SQL Injection attacks built as a single self-contained HTML file. No server, no setup, no dependencies required.

---

## 📋 Table of Contents

1. [Project Overview](#project-overview)
2. [Technologies Used](#technologies-used)
3. [How It Was Built — Step by Step](#how-it-was-built)
4. [Project Structure](#project-structure)
5. [How to Use the Demo](#how-to-use-the-demo)
6. [Attack Types Explained](#attack-types-explained)
7. [What Each Section Does](#what-each-section-does)
8. [Prevention Techniques Covered](#prevention-techniques-covered)
9. [Disclaimer](#disclaimer)

---

## Project Overview

This project visually demonstrates how **SQL Injection (SQLi)** works on a simulated e-commerce website called *ShopZone*. It shows how an attacker can manipulate database queries through unsanitized input fields to expose all product records, or even leak a hidden users/admin table.

The entire project runs in a **single HTML file** — open it in any browser and it works instantly.

---

## Technologies Used

| Technology | Purpose |
|---|---|
| HTML5 | Page structure and layout |
| CSS3 | Styling, animations, cyberpunk theme |
| Vanilla JavaScript | Database simulation and attack logic |
| Google Fonts | Custom typography (Orbitron, Share Tech Mono, Exo 2) |

No backend, no framework, no npm install needed.

---

## How It Was Built

### Step 1 — Concept & Design Direction

The first decision was the **visual theme**. Since this is a cybersecurity project, a **dark cyberpunk / hacker aesthetic** was chosen with:
- A near-black background (`#050a0f`)
- Cyan (`#00e5ff`) as the primary accent color
- Red (`#ff2d55`) to highlight attacks and danger
- Monospace fonts to feel like a terminal/code environment
- Animated CSS grid lines and scanline overlay for atmosphere

### Step 2 — Page Layout

The page was divided into **5 clearly numbered sections** to match a college project format:

```
01 → What is SQL Injection?   (Theory)
02 → Live Demonstration       (Interactive)
03 → Query Breakdown          (Visual)
04 → Prevention Techniques    (Solutions)
05 → Project Summary          (Conclusion)
```

A sticky header with a glitch animation on the title and a pulsing "Educational Purpose Only" badge was added at the top.

### Step 3 — Simulating the Database in JavaScript

Since there is no real backend, a JavaScript object was used to simulate a database with two tables:

```javascript
const db = {
  products: [
    { id: 1, name: "Laptop", category: "Electronics", price: 54999, stock: 12 },
    // ...7 more products
  ],
  users: [
    { id: 1, username: "admin", password_hash: "5f4dcc3b...", role: "administrator" },
    // ...
  ]
};
```

This accurately represents how a real relational database stores data in separate tables.

### Step 4 — Defining the Four Attack Scenarios

Each attack was defined as an object containing:
- The malicious **input string**
- The **resulting SQL query** (with HTML color coding for clarity)
- An **explanation** of why it works
- A **JavaScript function** simulating what the database would return

```javascript
const attacks = [
  { label: "Normal Search",  input: "Laptop",        action: () => db.products.filter(p => p.name === "Laptop") },
  { label: "Classic OR 1=1", input: "' OR '1'='1",   action: () => db.products },          // returns ALL rows
  { label: "Comment Bypass", input: "' OR 1=1 --",   action: () => db.products },          // returns ALL rows
  { label: "UNION Attack",   input: "' UNION SELECT", action: () => [...db.users mapped] } // leaks users table
];
```

### Step 5 — Building the Simulated Website Panel

A fake e-commerce site ("ShopZone") was built inside the left panel with:
- A branded green header navigation bar
- A search input field that visually turns red when injected
- A results area that renders a dynamic HTML table

The search input and the Execute Attack button are wired to the same JavaScript function, which picks the currently selected attack, runs its `action()` function, and renders the results as a table.

### Step 6 — SQL Query Display with Syntax Highlighting

The generated SQL query is displayed with **manual CSS-based syntax highlighting** using `<span>` tags:

```html
<span class="sql-kw">SELECT</span> * 
<span class="sql-kw">FROM</span> <span class="sql-table">products</span>
<span class="sql-kw">WHERE</span> name = <span class="sql-inject">' OR '1'='1'</span>;
```

Each class applies a different color:
- `.sql-kw` — Blue (SQL keywords like SELECT, FROM, WHERE)
- `.sql-str` — Orange (string values)
- `.sql-inject` — Red + underline (the injected malicious part)
- `.sql-table` — Teal (table names)

### Step 7 — Query Flow Visualization

A horizontal flow diagram was built using CSS flexbox to visually show:

```
[ USER INPUT ] → [ BACKEND CODE ] → [ SQL QUERY ] → [ DATABASE RESULT ]
```

Each box updates dynamically when an attack is selected. For attack scenarios, the SQL Query and Result boxes turn red to indicate danger.

### Step 8 — Prevention Section

Six prevention cards were built, each with:
- A colored top border accent
- A title (e.g., "PREPARED STATEMENTS")
- A plain-language explanation
- A real code snippet showing the secure implementation

### Step 9 — Animations & Polish

CSS animations added throughout:
- **Glitch effect** on the main title (shifts left/right with color splitting)
- **Pulse animation** on the warning badge
- **Flash animation** when an attack banner appears
- **Hover transitions** on all interactive buttons
- **Focus glow** on the search input

---

## Project Structure

Since this is a single-file project, all code lives in `sql_injection_demo.html`:

```
sql_injection_demo.html
│
├── <head>
│   ├── Google Fonts import
│   └── <style> — All CSS (theme variables, layout, animations)
│
└── <body>
    ├── <header>            — Title, badge, subtitle
    ├── .container
    │   ├── Section 01      — Theory cards (4 cards)
    │   ├── Section 02      — Live demo (website panel + attack panel)
    │   ├── Section 03      — Query flow visualization
    │   ├── Section 04      — Prevention grid (6 cards)
    │   └── Section 05      — Summary table
    ├── <footer>
    └── <script>            — Database simulation + attack logic
```

---

## How to Use the Demo

### Opening the File

1. Download `sql_injection_demo.html`
2. Double-click it — it opens directly in any web browser (Chrome, Firefox, Edge, Safari)
3. No internet connection required after the page loads (fonts load from Google Fonts on first open)

### Running the Demo (Step by Step)

**Normal Search (Baseline):**
1. Click the **"Normal Search"** button in the attack selector panel (top-left option)
2. The search field shows `Laptop`
3. Click **"EXECUTE ATTACK"** (or press the green SEARCH button on the site)
4. Only 1 product (Laptop) is returned — this is the expected, secure behaviour

**Classic OR 1=1 Attack:**
1. Click **"Classic OR 1=1"** in the attack selector
2. The input field changes to `' OR '1'='1` and turns red
3. The SQL query panel updates to show the injected query
4. Click **"EXECUTE ATTACK"**
5. All 8 products are now displayed — the filter was bypassed completely

**Comment Bypass Attack:**
1. Click **"Comment Bypass"**
2. Input becomes `' OR 1=1 --`
3. Note how `--` appears greyed out in the SQL panel (it's a comment, ignored by the database)
4. Click **"EXECUTE ATTACK"** — same result, all products exposed

**UNION Attack (Most Dangerous):**
1. Click **"UNION Attack"**
2. Input becomes `' UNION SELECT --`
3. See how the SQL query now pulls from the `users` table instead of products
4. Click **"EXECUTE ATTACK"**
5. The results table now shows usernames, roles, and password hashes — a full data breach scenario

---

## Attack Types Explained

### 1. Normal Search
```sql
SELECT * FROM products WHERE name = 'Laptop';
```
Standard query — only rows where name equals "Laptop" are returned.

### 2. Classic OR 1=1
```sql
SELECT * FROM products WHERE name = '' OR '1'='1';
```
The attacker closes the string with `'` and adds `OR '1'='1'` — a condition that is always true. The WHERE clause becomes meaningless and every row is returned.

### 3. Comment Bypass (`--`)
```sql
SELECT * FROM products WHERE name = '' OR 1=1 --'
```
The `--` characters start a SQL comment, making the database ignore everything after it (including the closing quote that was expected). Same result as OR 1=1 but avoids syntax errors.

### 4. UNION Attack
```sql
SELECT * FROM products WHERE name = ''
UNION SELECT id, username, password_hash, role, NULL FROM users --
```
`UNION` combines the result of two SELECT statements. The attacker queries a completely different table (`users`) and its data appears in the output alongside (or instead of) the products.

---

## What Each Section Does

| Section | Content |
|---|---|
| **01 — What is SQL Injection?** | Four theory cards: Definition, How It Happens, Impact, Prevalence |
| **02 — Live Demonstration** | Interactive ShopZone website (left) + attack selector with SQL display (right) |
| **03 — Query Breakdown** | Flowchart showing input → backend → SQL → result, plus vulnerable PHP code example |
| **04 — Prevention Techniques** | Six cards: Prepared Statements, Stored Procedures, Input Validation, Least Privilege, ORM, WAF |
| **05 — Project Summary** | Topic, severity (CVSS 9.8), OWASP rank, and written conclusion |

---

## Prevention Techniques Covered

1. **Prepared Statements / Parameterized Queries** — Input is passed as a separate parameter, never embedded in the query string
2. **Stored Procedures** — SQL logic lives in the database, not the application
3. **Input Validation** — Whitelist allowed characters, reject `' -- ; /*` and similar
4. **Least Privilege** — App database users should only have SELECT/INSERT on required tables
5. **ORM / Query Builders** — Libraries like Eloquent, SQLAlchemy auto-escape inputs
6. **Web Application Firewall (WAF)** — Detects and blocks SQLi patterns at the network level

---

## Disclaimer

> ⚠️ This project is created **strictly for educational purposes** as part of a college mini project on web application security. All database interactions are **simulated entirely in JavaScript** — no real database, no real server, and no real vulnerabilities are present. Do not use SQL injection techniques on any system you do not own or have explicit permission to test. Unauthorized access to computer systems is illegal.

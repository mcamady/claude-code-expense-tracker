**Spec Document – Spendly Database Layer**

## **1. Overview**

Replace the stub in `database/db.py` with a working SQLite implementation.
This establishes the **data layer foundation** for the Spendly application.

All future features depend on this being correctly implemented:

* Authentication
* Profile
* Expense tracking

---

## **2. Dependencies**

* None (this is the first step)

---

## **3. Routes**

* No new routes
* Existing routes in `app.py` remain unchanged

---

## **4. Database Schema**

### **A. users Table**

| Column        | Type    | Constraints                |
| ------------- | ------- | -------------------------- |
| id            | INTEGER | Primary key, autoincrement |
| name          | TEXT    | Not null                   |
| email         | TEXT    | Unique, not null           |
| password_hash | TEXT    | Not null                   |
| created_at    | TEXT    | Default datetime('now')    |

---

### **B. expenses Table**

| Column      | Type    | Constraints                      |
| ----------- | ------- | -------------------------------- |
| id          | INTEGER | Primary key, autoincrement       |
| user_id     | INTEGER | Foreign key → users.id, not null |
| amount      | REAL    | Not null                         |
| category    | TEXT    | Not null                         |
| date        | TEXT    | Not null (YYYY-MM-DD format)     |
| description | TEXT    | Nullable                         |
| created_at  | TEXT    | Default datetime('now')          |

---

## **5. Functions to Implement (`database/db.py`)**

### **A. get_db()**

* Opens connection to:

  * `spendly.db` OR `expense_tracker.db`
* Set:

  * `row_factory = sqlite3.Row`
  * `PRAGMA foreign_keys = ON`
* Returns connection

---

### **B. init_db()**

* Creates tables using `CREATE TABLE IF NOT EXISTS`
* Safe for multiple calls
* Ensures schema readiness

---

### **C. seed_db()**

* Check if `users` table has data

  * If yes → exit (avoid duplication)

* Insert demo user:

  * Name: Demo User
  * Email: [demo@spendly.com](mailto:demo@spendly.com)
  * Password: demo123 (hashed)

* Insert:

  * 8 sample expenses
  * Linked to demo user
  * Multiple categories
  * Dates within current month
  * At least one expense per category

---

## **6. Changes to `app.py`**

* Import:

  * `get_db`
  * `init_db`
  * `seed_db`

* On startup:

  * Call inside `app.app_context()`:

    * `init_db()`
    * `seed_db()`

* Ensure DB is ready before routes execute

---

## **7. Files to Change**

* `database/db.py`
* `app.py`

---

## **8. Files to Create**

* None

---

## **9. Dependencies**

* No new packages

Use:

* `sqlite3` (standard library)
* `werkzeug.security`

---

## **10. Categories (Fixed List)**

Use exactly:

* Food
* Transport
* Bills
* Health
* Entertainment
* Shopping
* Other

---

## **11. Rules for Implementation**

* ❌ No ORM (no SQLAlchemy)
* ✅ Use parameterized queries only
* ❌ No string formatting in SQL
* ✅ Enable foreign keys on every connection
* ✅ Store `amount` as REAL (float)
* ✅ Hash passwords using:

  ```python
  from werkzeug.security import generate_password_hash
  ```
* ✅ Prevent duplicate seed inserts
* ✅ Use YYYY-MM-DD date format

---

## **12. Expected Behavior**

### **get_db()**

* Returns working DB connection
* Dictionary-like row access
* Foreign key enforcement enabled

### **init_db()**

* Creates tables safely
* No failure on repeated runs

### **seed_db()**

* Inserts demo data once only
* No duplication

### **Database Enforces**

* Unique email
* Valid foreign keys

---

## **13. Error Handling**

* Duplicate email → fails (UNIQUE constraint)
* Invalid user_id → fails (foreign key constraint)
* Invalid queries → clear errors

---

## **14. Definition of Done**

* DB file created on startup
* Tables exist with correct schema
* Demo user created (hashed password)
* 8 sample expenses inserted
* No duplicate seed data
* App runs without errors
* Foreign keys enforced
* All queries parameterized



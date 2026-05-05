# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Setup and Development Commands
1. **Install dependencies**:
   ```
   pip install -r requirements.txt
   ```
2. **Run the development server** (defaults to `http://127.0.0.1:5001`):
   ```
   flask run --port=5001
   ```
3. **Run the test suite**:
   ```
   pytest -v
   ```
4. **Run a single test file** (e.g., `test_users.py`):
   ```
   pytest -v test_users.py
   ```

## High‑Level Architecture
- **`app.py`** – Flask application entry point; registers routes and starts the server.
- **`database/db.py`** – Intended to contain SQLite helper functions (`get_db`, `init_db`, `seed_db`). Currently a placeholder for students to implement.
- **Templates (`templates/` directory)** – Jinja2 HTML files for each UI page (`landing.html`, `register.html`, `login.html`, `terms.html`, `privacy.html`).
- **Static assets (`static/` directory)** – CSS and JavaScript files shared by the templates.

### Data Flow
```
Client HTTP request → Flask route (app.py) → (future) business logic → Database helpers (db.py) → SQLite DB → Response (HTML template)
```

## Project Structure
```
expense-tracker/
├─ app.py                # Flask app and route definitions
├─ database/
│   └─ db.py            # DB helper stub (students fill in)
├─ static/
│   └─ css/style.css    # Stylesheet
├─ templates/
│   ├─ base.html        # Base layout
│   ├─ landing.html
│   ├─ login.html
│   ├─ register.html
│   ├─ terms.html
│   └─ privacy.html
├─ requirements.txt      # Python dependencies (Flask, Werkzeug, pytest, pytest‑flask)
└─ CLAUDE.md            # ✅ This file
```

## Conventions
- Keep route handlers thin; move business logic to separate modules when the project grows.
- Use Jinja inheritance (`{% extends "base.html" %}`) for shared layout.
- All DB interactions should go through the helper functions defined in `database/db.py`.
- Run the app in debug mode only during development (`app.run(debug=True, ...)`).

## Security Notes
- Validate and sanitize any user‑provided data before using it in SQL statements.
- Enable Flask’s built‑in CSRF protection when forms are added.
- Store secrets (e.g., session keys) outside the repo or via environment variables.

## Typical Development Workflow
1. Install deps → `pip install -r requirements.txt`
2. Implement missing pieces (e.g., `db.py`).
3. Run the server → `flask run --port=5001`
4. Open `http://127.0.0.1:5001` in a browser.
5. Write/adjust tests → `pytest -v`
6. Commit changes following the project’s commit style.
```

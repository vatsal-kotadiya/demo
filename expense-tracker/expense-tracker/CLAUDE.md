# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Commands

```bash
# Install dependencies
pip install -r requirements.txt

# Run the dev server (port 5001)
python app.py

# Run all tests
pytest

# Run a single test file
pytest tests/test_auth.py

# Run a single test by name
pytest -k "test_login"
```

## Architecture

**Spendly** is a Flask expense-tracking app structured as a step-by-step student project. Most backend logic is intentionally left unimplemented — stubs in `app.py` mark each step.

### Request flow

All routes are defined in `app.py`. Templates extend `templates/base.html` (navbar + footer shell), with page content in `{% block content %}`. There is no blueprint or app-factory pattern — a single `Flask(__name__)` instance handles everything.

### Database layer

`database/db.py` is the only database file. It is expected to expose three functions:
- `get_db()` — returns a SQLite connection with `row_factory = sqlite3.Row` and `PRAGMA foreign_keys = ON`
- `init_db()` — creates tables with `CREATE TABLE IF NOT EXISTS`
- `seed_db()` — inserts sample rows for development

There is no ORM. All queries are raw SQL via the connection returned by `get_db()`.

### Auth

Werkzeug is used for password hashing (`werkzeug.security`). Session management is expected to use Flask's built-in `session` (cookie-based, requires `app.secret_key`).

### Frontend

- `static/css/style.css` — single stylesheet; uses CSS custom properties defined in `:root` (colors, fonts, radii). Font families: `DM Serif Display` (headings) and `DM Sans` (body).
- `static/js/main.js` — placeholder; JS is added incrementally as features are built.
- Currency is Indian Rupees (₹); format amounts accordingly.

### Implementation steps (as designed)

1. Database setup (`database/db.py`)
2. Register (form + insert user)
3. Login / Logout (session)
4. Profile page
5–6. Expense dashboard / listing
7. Add expense
8. Edit expense
9. Delete expense

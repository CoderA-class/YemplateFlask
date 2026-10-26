# Flask + SQL Server Starter

A reusable Flask starter with an application factory, blueprints/routes, services, and a central SQL Server connection helper.

## 1. Requirements

- Python 3.10+
- Microsoft SQL Server reachable from this machine
- Microsoft ODBC Driver 18 for SQL Server (Driver 17 is also supported if installed)

## 2. Setup (Windows PowerShell)

```powershell
py -m venv .venv
.\.venv\Scripts\Activate.ps1
python -m pip install --upgrade pip
pip install -r requirements.txt
Copy-Item .env.example .env
```

Edit `.env` and set your database server, database name, and authentication details. Never commit `.env`.

For Windows Authentication, set `DB_TRUSTED_CONNECTION=true`; you can then leave username/password blank. Ensure your installed ODBC driver matches `DB_DRIVER`.

## 3. Run

```powershell
python run.py
```

Open http://127.0.0.1:5000

## 4. Structure

- `app/__init__.py`: application factory and blueprint registration.
- `app/config.py`: environment-based settings.
- `app/routes/`: request handling only; keep business logic out of routes.
- `app/services/`: business logic and database operations.
- `app/database/`: connection creation and database health check.
- `app/templates/`: Jinja templates.
- `app/static/`: CSS, JavaScript, images.
- `tests/`: starter tests.

## 5. Add a feature

1. Create `app/routes/reports.py` (or another feature route module).
2. Create `app/services/reports_service.py` for business logic.
3. Add a blueprint in `app/__init__.py`.
4. Put templates under `app/templates/` and assets under `app/static/`.

## 6. SQL Server notes

The connection helper uses `pyodbc` and parameterized queries. Use `?` placeholders for values and never concatenate user input into SQL. Each service should close its cursor and connection. For multi-step writes, use a transaction and explicitly commit; rollback on errors.

`DB_TRUST_SERVER_CERTIFICATE=yes` is convenient for local development. In production, configure a trusted SQL Server certificate and set this to `no`.

## 7. Health check

Visit `/health` to check whether the Flask app is running and whether SQL Server can be reached. The route returns a generic error rather than exposing credentials or raw connection details.

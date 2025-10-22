# Contra o Bullying - Deploy notes for Render

Quick steps to deploy this Flask app to Render (copy-paste):

1) Commit & push your branch to GitHub:

   git add -A
   git commit -m "Prepare for Render: requirements/procfile/runtime, fix templates and DB init"
   git push origin main

2) On Render: create a new Web Service and connect your GitHub repo/branch.
   - Environment: Python
   - Branch: main (or your branch)
   - Build command: leave default (Render runs `pip install -r requirements.txt`)
   - Start command: Render reads the `Procfile` automatically. Confirm `Procfile` contains:

       web: gunicorn app:app --bind 0.0.0.0:$PORT

   - Runtime: Render will use `runtime.txt` (this repo has `python-3.10.12`)

3) If pip install fails on Render because of a missing wheel or build dependency, Render logs will show the failing package. Often adding `wheel` to `requirements.txt` resolves many build issues (already included).

4) Local troubleshooting tips (Windows):
   - If `pip install` fails with a permission error ([WinError 5] Access denied), try:
     - Ensure no Python processes (including a running dev server) are running.
     - Use per-user install: `python -m pip install --user -r requirements.txt` (safe, no admin required)
     - Or run PowerShell as Administrator and install system-wide.

5) Database & startup:
   - The app creates the SQLite DB/tables on import. `init_db()` is invoked safely on import so `gunicorn app:app` will create required tables if missing.

6) Admin login (local):
   - Use password `DanielGuilherme` to sign in via the admin button (this is a simple hard-coded admin check). Consider replacing with proper auth before production.

7) After deploy:
   - Open the Render service URL; check `/`, `/estatisticas`, `/admin` (you'll need to login for admin).

If you want, I can also:
- Add a short `deploy-to-render.md` with screenshots and exact UI steps.
- Run a final smoke-test (start gunicorn locally and curl /) after you confirm packages are installed.


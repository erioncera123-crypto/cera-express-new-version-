# CERA EXPRESS LLC — website deployment

This is an actual Flask website codebase, not a published website. To put it online, provision a Python web host with an HTTPS domain, persistent storage, and secure secret variables.

1. Deploy the folder using the included Procfile (`gunicorn --bind 0.0.0.0:${PORT:-8000} --workers 2 app:app`).
2. Install `requirements.txt`.
3. Create `ADMIN_PASSWORD_HASH` with `python -c "from werkzeug.security import generate_password_hash; import getpass; print(generate_password_hash(getpass.getpass('Admin password: ')))"`.
4. Set `SECRET_KEY` to a random 32+ byte secret and `HTTPS_ONLY=1` behind HTTPS.
5. Mount a persistent private storage volume and set `DATABASE_PATH` to a writable path on it. Back it up routinely.
6. Add a custom domain and restrict access to authorized staff. Review security before using real payroll data.
7. GoDaddy Microsoft 365 email is **not connected**. For production use, integrate Microsoft Graph with approved OAuth credentials and admin consent, rather than assuming SMTP password authentication will work. The current SMTP pathway is only a configurable legacy option, not a configured Outlook integration.

Driver rates default to 50%. The separate Change driver percentage form modifies future imports; saved weeks retain the stored rate. Import preview is mandatory; verify toll date range and unmatched transponders. Sending an email requires explicit selection and approval.

**Version 4: editable driver percentages (default 50%), saved per payroll week.** See DEPLOYMENT.md for website publishing instructions.

# CERA EXPRESS LLC — Weekly Payroll Web App (starter)

A password-protected Flask web app with preloaded driver IDs and five known transponders, weekly gross/toll entry, 50/50 pay calculations, saved weekly records, CSV export and individual PDF statements.

## Run locally

1. Install Python 3.11+.
2. In this folder: `python -m pip install -r requirements.txt`
3. Generate a password hash: `python -c "from werkzeug.security import generate_password_hash; print(generate_password_hash('REPLACE_WITH_A_STRONG_PASSWORD'))"`
4. Set environment variables `ADMIN_PASSWORD_HASH` to the printed hash and `SECRET_KEY` to a long random string (for example, generated with `python -c "import secrets; print(secrets.token_hex(32))"`). Do **not** put your plaintext password in the source code.
5. Run `python app.py` and open `http://127.0.0.1:5000` on your computer.

## Deployment

Deploy to a Python-capable host using `gunicorn app:app`. Set `HTTPS_ONLY=1` when HTTPS is configured, and set the password hash, secret key and `DATABASE_PATH` as environment variables. Use a **persistent private volume** for the SQLite database; ephemeral hosting will lose payroll history. Back up the database regularly. Do not make the site public without HTTPS, secure password management, access controls, backup and security review. For multiple users or production scale, migrate to a managed database and individual user accounts.

## Current limitations

- Settlement PDF upload extracts text and highlights lines containing known driver IDs; it does not yet infer gross amounts.
- Excel upload highlights rows matching known transponder numbers; it does not yet infer toll amount/date columns.
- Both file types need real sample reports to configure safe, accurate import rules. Nothing from uploads is posted automatically.
- Weekly date is a label selected by the operator. Verify the toll report and settlement cover the same date range.
- PDF statement links work for saved driver/week entries only.
- No bank payments, ACH, tax calculations, withholding or accounting integrations.
- This is a working starter application, not a fully hosted production payroll service.

## Driver settlement email delivery (new)

- Enter each driver's email address using **Add or update driver**. For existing drivers, re-enter their ID and transponder when updating the email.
- Save and verify weekly payroll; choose **Review & send settlement emails**. Check the desired recipients and explicitly approve before sending.
- Each email includes that driver's individual PDF, not the full payroll. Sent statements are logged with a fingerprint of the week's amounts to avoid accidental duplicates. Changed payroll can be sent as a new version.
- Configure server environment variables `SMTP_HOST`, `SMTP_PORT` (default `587`), `SMTP_USER`, `SMTP_PASSWORD`, and `SMTP_FROM`. Use `SMTP_SSL=1` and port 465 only if your provider requires implicit TLS. The default is STARTTLS on port 587. Use an approved SMTP account or app password; never put credentials in source files.
- SMTP send success means the mail server accepted the message, **not** proof it reached the driver's inbox. Check your provider's delivery logs for bounces.
- No email is sent automatically or until you select recipients and approve. This starter uses one SMTP connection per driver; consider a production email provider, queue and rate limits before large batches.
- Before hosting with real personal/payroll information, obtain a security review, backups, proper per-user authentication and delivery safeguards.

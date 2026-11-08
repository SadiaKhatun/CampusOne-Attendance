# CampusOne update — November 2026

## Dynamic QR attendance
- Teacher starts a session and opens a separate Live QR interface.
- The interface displays course name, teacher name, session ID, and session start/end time.
- QR token is refreshed every 5 seconds and each token expires after 15 seconds.
- Students can open the scanner from Available Sessions or from the session notification.
- The device camera is used only to read the QR; no student photo is captured or uploaded.
- Server validates login, course enrollment, token expiry and active session. Duplicate Present records are blocked.
- Late attendance requests remain available after the session; teachers can approve or cancel.

## Password management
- Student/Teacher registration stores a unique email address; older accounts can add one from My Profile.
- Login page includes a Forgot Password popup and sends a 30-minute one-time reset link through configured SMTP.
- My Profile includes Old Password, New Password and Confirm New Password fields.

## Database safety
- Existing attendance/session/user data is preserved through additive SQLite migrations.
- `attendance_qr_tokens` stores hashed, short-lived QR tokens; `password_reset_tokens` stores hashed one-time reset-token references.
- Legacy OTP/photo endpoints are disabled; old database columns are retained only for compatibility.

## Which frontend is updated?
The Flask-served interface is in `templates/index.html`, `static/app.js`, and `static/style.css`, with API/database logic in `app.py`. The separate `react_frontend` folder remains a UI learning preview and is not the database-connected Render interface.

See `DYNAMIC_QR_AND_PASSWORD_GUIDE_BANGLA.md` for setup, SMTP and browser camera requirements.

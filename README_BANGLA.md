# CampusOne — Updated Demo Project

## New features
- Student dashboard displays all classes scheduled for the current day.
- Students can open Class Routine and filter their schedule by Sunday through Saturday.
- Admin can select multiple class days for one routine entry.
- The “All days except Friday” option schedules the class on Sunday, Monday, Tuesday, Wednesday, Thursday and Saturday.
- Semester selection is limited to 1st through 8th semester.
- Existing SQLite data is preserved; schema changes are additive.

## Run on Windows
1. Extract the ZIP archive.
2. Run `run_windows.bat`.
3. Open the local address shown in the terminal.

Demo accounts:
- Admin: `ADMIN001` / `admin123`
- Student: `S001` / `student123`
- Teacher: `T001` / `teacher123`

This is a Flask + SQLite demo. Test all workflows locally before using real institutional data.


## Live deployment status
এই package-এ Flask app deploy করার starter files যোগ করা হয়েছে। React dashboard preview এখনো Flask API-এর সঙ্গে সম্পূর্ণ যুক্ত নয়। বিস্তারিত `LIVE_DEPLOYMENT_GUIDE_BANGLA.md` দেখুন।


## Attendance mode update — Dynamic QR
- OpenCV, face recognition, classroom photo upload বা photo-based attendance নেই।
- Teacher session চালু করে Open Live QR খুললে QR প্রতি ৫ সেকেন্ডে refresh হয়।
- Student Attendance page বা session notification থেকে Mark Attendance চাপলে device camera QR scanner খোলে।
- Camera কেবল QR scan করে; কোনো ছবি capture বা upload করে না।
- Server-side short-lived token, active session, course enrollment এবং duplicate attendance যাচাই হয়।
- QR scan সফল হলে একই session-এর জন্য Student Present হয়; QR refresh হলেও Present record বদলায় না।
- Camera permission-এর জন্য HTTPS অথবা localhost প্রয়োজন; updated Chrome/Edge ব্যবহার করুন।

## Password security
- Account creation-এ registered email address দিতে হয়; পুরোনো account-এর email My Profile থেকে যোগ করতে হবে।
- Login page-এর Forgot password popup থেকে email reset link চাওয়া যায়। Reset link ৩০ মিনিট বৈধ এবং একবার ব্যবহারযোগ্য।
- Email পাঠাতে `.env.example` কপি করে `.env` বানিয়ে SMTP settings configure করতে হবে।
- My Profile থেকে Old Password, New Password ও Confirm New Password দিয়ে password পরিবর্তন করা যায়। New password ৮–৬৪ characters।
- বিস্তারিত `DYNAMIC_QR_AND_PASSWORD_GUIDE_BANGLA.md` ফাইলে আছে।

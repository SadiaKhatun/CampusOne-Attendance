# CampusOne Smart Attendance Update (বাংলা)

এই সংস্করণে SQLite-এর পুরোনো তথ্য না মুছে additive migration করা হয়েছে। Dynamic QR token এবং password reset-এর জন্য প্রয়োজনীয় নতুন table/column প্রথমবার app চালু হলে তৈরি হবে।

## Dynamic QR Attendance
1. Teacher Course থেকে session চালু করবেন।
2. Attendance page-এর **Open Live QR** অথবা session তৈরির পর **Open Live QR Code** চাপলে বড় QR interface খুলবে।
3. Interface-এ Course Name, Teacher Name, Session ID, Session Start ও End Time দেখা যাবে। QR প্রায় প্রতি ৫ সেকেন্ডে নতুন করে তৈরি হবে; প্রতিটি token server-side ১৫ সেকেন্ডে expire হবে।
4. Student Available Sessions থেকে **Mark Attendance · Scan QR** চাপলে device camera scanner খুলবে। Student session notification খুলেও **Scan QR · Mark Attendance** চাপতে পারবে।
5. Server token expiry, login, course enrollment এবং active session যাচাই করে Present record তৈরি করবে। একই student একই session-এ দ্বিতীয়বার Present হতে পারবে না।
6. QR refresh হলেও আগের Present record অক্ষত থাকবে। Expired session-এর জন্য Late Request পাঠানো যায়; Teacher approve করলে Present, cancel করলে Absent হবে।

Camera শুধুমাত্র QR পড়তে ব্যবহৃত হয়। কোনো student photo capture বা upload করা হয় না; OpenCV/face recognition ব্যবহার করা হয় না। Browser camera permission-এর জন্য HTTPS অথবা localhost প্রয়োজন। Updated Chrome/Edge recommended.

## Password Management
- নতুন Student/Teacher account তৈরি করার সময় unique registered email দিতে হবে।
- পুরোনো account-এর email My Profile থেকে add/update করতে হবে। Admin account-এর email-ও Admin My Profile থেকে সেট করতে হবে।
- Login page-এর Forgot password popup থেকে email দিলে ৩০ মিনিট মেয়াদি একবার ব্যবহারযোগ্য reset link পাঠানো হয়।
- My Profile-এ Old Password, New Password, Confirm New Password দিয়ে password পরিবর্তন করা যায়। নতুন password ৮–৬৪ characters হতে হবে।
- Email delivery-এর জন্য `.env.example` কপি করে `.env` বানিয়ে SMTP settings দিতে হবে। Gmail ব্যবহার করলে Google App Password ব্যবহার করতে হবে।

## Windows Setup
1. ZIP extract করো।
2. `.env.example` কপি করে `.env` বানাও এবং SMTP/secret settings বসাও।
3. `run_windows.bat` চালাও। Pip install-এ network timeout হলে launcher এখন ১০০ সেকেন্ড timeout ও ১০ retry ব্যবহার করবে।
4. Browser-এ `http://127.0.0.1:5000` খোলো।
5. Teacher session start করে Live QR page খোলা রাখবেন। Student-এর ফোনে HTTPS deployment থাকলে camera scan সবচেয়ে ভালো কাজ করবে।

## Proxy Attendance Limitation
Rotating short-lived QR screenshot reuse-এর সময় কমায়, কিন্তু token expire হওয়ার আগে QR screenshot অন্যকে পাঠানো হলে proxy attendance সম্পূর্ণভাবে ঠেকানো যায় না। আরও শক্ত প্রতিরোধের জন্য classroom Wi-Fi/proximity validation বা teacher-side verification যোগ করা যেতে পারে।

## API summary
- `GET /api/teacher/session/<session_id>/qr` — teacher-only rotating QR image and session details.
- `POST /api/mark-attendance-qr` — student-only QR token validation and Present marking.
- `POST /api/forgot-password` — registered email-এ reset link request.
- `GET /reset-password?token=...` and `POST /api/reset-password` — one-time password reset.
- `POST /api/change-password` — logged-in user's old/new/confirm password update.
- `POST /api/late-attendance-requests` and teacher review endpoints — late attendance workflow.

Production deployment-এর আগে HTTPS, secure `CAMPUSONE_SECRET`, SMTP, database backup এবং permission review নিশ্চিত করতে হবে।

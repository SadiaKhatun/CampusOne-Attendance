# CampusOne: Dynamic QR Attendance ও Password Management

## Student QR Attendance
- Student Available Sessions থেকে Mark Attendance চাপলে QR scanner চালু হয়।
- Student Dashboard-এর session notification খুলেও Scan QR · Mark Attendance ব্যবহার করতে পারে।
- Camera কেবল QR পড়তে ব্যবহৃত হয়; কোনো ছবি তোলা বা আপলোড হয় না।
- QR token প্রতি ৫ সেকেন্ডে refresh হয় এবং server-side ১৫ সেকেন্ডে expire হয়।
- Server student login, course enrollment, active session এবং token expiry যাচাই করে।
- একই student একই session-এ একবারই Present হতে পারে। QR refresh হলেও Present record অক্ষত থাকে।
- Expired session-এর জন্য Late Request পাঠানো যায়; Teacher approve করলে Present, cancel করলে Absent হয়।

## Teacher Live QR
- Teacher Start Session করার পর Attendance page থেকে Open Live QR, অথবা session creation confirmation থেকে Open Live QR Code চাপবে।
- বড় QR interface-এ Course, Teacher, Session ID, Start ও End time দেখা যায়।
- QR স্বয়ংক্রিয়ভাবে refresh হয়; teacher-কে browser tab খোলা রাখতে হবে।

## Camera/browser requirement
- Browser camera access-এর জন্য HTTPS domain অথবা localhost প্রয়োজন। LAN IP দিয়ে plain HTTP খুললে browser camera permission block করতে পারে।
- QR scanning built-in BarcodeDetector ব্যবহার করে; বর্তমানে updated Chrome/Edge browser recommended.
- Camera শুধু scan করে, কোনো photo capture বা upload করে না।

## Forgot Password
- Account তৈরি করার সময় registered email দিতে হবে। পুরোনো account-এর email My Profile থেকে যোগ করতে হবে।
- Login page-এর Forgot password? popup থেকে email দিলে reset link পাঠানো হবে।
- Reset link ৩০ মিনিট বৈধ এবং একবার ব্যবহারযোগ্য।
- Email পাঠাতে `.env`-এ SMTP settings configure করতে হবে। Gmail হলে Google App Password ব্যবহার করতে হবে; normal Gmail password নয়।
- Admin account-এর email-ও Admin My Profile থেকে সেট করতে হবে।

## Change Password
- My Profile-এ Old Password, New Password, Confirm New Password দিয়ে password পরিবর্তন করা যায়।
- New password ৮–৬৪ characters হতে হবে এবং পুরোনো password সঠিক হতে হবে।

## Setup
1. `.env.example` কপি করে `.env` বানাও।
2. `CAMPUSONE_SECRET`-এ random secret বসাও।
3. `SMTP_HOST`, `SMTP_PORT`, `SMTP_USER`, `SMTP_PASSWORD`, `SMTP_FROM`, `PUBLIC_BASE_URL` পূরণ করো।
4. `run_windows.bat` চালাও; pip installation timeout/retry বাড়ানো হয়েছে।
5. Student/Teacher account-এর registered email ঠিক আছে কি না Profile থেকে যাচাই করো।

## Proxy attendance limitation
Short-lived rotating QR screenshot reuse-এর সময় কমায়, কিন্তু কেউ QR screenshot-টি expiry-এর ১৫ সেকেন্ডের মধ্যে অন্যকে পাঠালে সম্পূর্ণভাবে ঠেকানো যায় না। উচ্চ নিশ্চয়তার জন্য পরে classroom network/proximity validation বা teacher-side visual confirmation যোগ করা যেতে পারে।

Vulnerability: SQL Injection (SQLi)  
Severity: Critical  
Platform: PortSwigger Web Security Academy  
Lab: [SQL Injection vulnerability allowing login bypass](https://portswigger.net/web-security/sql-injection/lab-login-bypass)

---

## Summary
A login form passes user-supplied input directly into a SQL query 
without sanitization. By injecting SQL syntax into the username field, 
an attacker can manipulate the query logic to authenticate as any user 
— including administrator — without knowing their password.

---

## How I exploited it

1. Navigated to the login page and opened Burp Suite Repeater
2. Entered the following payload in the username field:

   `' OR 1=1--`

3. The password was 123 its not the correct password
4. The injected payload closes the SQL string, adds a condition 
   that is always true (1=1), and comments out the password 
   check entirely
5. The server returned the first account in the database — 
   the administrator account
6. Successfully logged in as administrator with no password

---

## Why this works
The backend SQL query likely looks like this:

SELECT * FROM users WHERE username='input' AND password='input'

After injection it becomes:

SELECT * FROM users WHERE username='' OR 1=1--' AND password=''

OR 1=1 is always true, so the WHERE clause returns all users. 
The database returns the first row which is typically the 
administrator account.

## Real World Impact
Full administrative account takeover with no credentials required. 
An attacker gains complete control of the application — access to 
all user data, admin functions, and potentially the underlying 
database. This is a critical severity vulnerability that violates 
POPIA in South Africa, exposing the business to legal liability 
and heavy fines.

---

## How to fix it
- Use parameterized queries / prepared statements — never 
  concatenate user input into SQL queries
- Implement an ORM (Object Relational Mapper)
- Apply input validation and allowlisting
- Never expose detailed database errors to the user

---

## Screenshots
<img width="1366" height="768" alt="Screenshot (117)" src="https://github.com/user-attachments/assets/b295c52c-d9a1-43e0-8bd3-54a2d677c837" />
<img width="1366" height="768" alt="Screenshot (116)" src="https://github.com/user-attachments/assets/b5d95f5c-0b61-489f-95ea-6cf001adc649" />


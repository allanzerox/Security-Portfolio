Vulnerability: Cross-Site Scripting (XSS) — Stored  
Severity: High  
Platform: PortSwigger Web Security Academy  
Lab: [Stored XSS into HTML context with nothing encoded](https://portswigger.net/web-security/cross-site-scripting/stored/lab-html-context-nothing-encoded)

---

## Summary
A blog comment section stores and reflects user input directly into 
the HTML page without any sanitization or encoding. An attacker can 
inject malicious HTML or JavaScript that executes in the browser of 
every user who visits the page — including administrators.

---

## How I exploited it

1. Navigated to a blog post with a comment section
2. Intercepted the comment POST request in Burp Suite
3. Injected the following payload into the comment field:

   `<script>alert('virgin')</script>`

4. The code was stored in the database 
5. Every user visiting the blog post now loads the injected content
6. To demonstrate JavaScript execution, the payload can be 
   escalated to:

   `<script>alert(document.cookie)</script>`

   Which would steal session cookies from any visitor

---

## Why Stored XSS is more dangerous than Reflected XSS
- Reflected XSS requires tricking a victim into clicking a 
  malicious link
- Stored XSS executes automatically for every user who visits 
  the page — no interaction needed
- One payload can affect thousands of users simultaneously
- Administrators visiting the page can have their session 
  hijacked, giving attackers full control of the application

---

## Real World Impact
- Session hijacking — steal cookies and take over any user account
- Admin account takeover — if an admin visits the page their 
  session is compromised
- Defacement — inject content that damages brand reputation
- Phishing — inject fake login forms to steal credentials
- Malware distribution — redirect users to malicious sites


---

## How to fix it
- Encode all user supplied output before rendering in HTML
- Implement a Content Security Policy (CSP)
- Use allowlisting for any HTML input fields
- Never trust user supplied data — validate and sanitize 
  server side before storing

---

## Screenshots
<img width="1366" height="768" alt="Screenshot (120)" src="https://github.com/user-attachments/assets/bf0a7120-4805-4c0a-acf8-c75e3741e1e5" />

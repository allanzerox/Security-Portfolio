Vulnerability: Broken Authentication

Severity: High

What I found:
The password reset flow submits the username as a parameter in the POST request when setting a new password. This parameter is not validated against the reset token.

How I exploited it:
Intercepted the POST request in Burp Suite Repeater, changed the username parameter from my account to "carlos", submitted the request. Successfully changed carlos's password and logged in as him.

Real world impact:
An attacker can take over any account on the platform without knowing their password — including admin accounts.

How to fix it:
Tie the password reset token to the username server-side. Never trust client-supplied username during password reset.

Screenshot:
<img width="1366" height="768" alt="Screenshot (112)" src="https://github.com/user-attachments/assets/6ef91c69-1425-4d97-8071-13b2989e5d98" />
<img width="1366" height="768" alt="Screenshot (113)" src="https://github.com/user-attachments/assets/f4323b36-7f05-4499-98b6-e82e3b590cbb" />
<img width="1366" height="768" alt="Screenshot (114)" src="https://github.com/user-attachments/assets/c8ccad76-e63d-4657-a4dc-eb2497167850" />

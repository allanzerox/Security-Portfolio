Vulnerability: Business Logic Flaw  
Severity: High  
Platform: PortSwigger Web Security Academy  
Lab: [Low-level logic flaw](https://portswigger.net/web-security/logic-flaws/examples/lab-logic-flaws-low-level)

---

## Summary
An e-commerce application fails to validate cumulative item quantities 
in the cart. By sending a high volume of requests, an attacker can 
overflow the price calculation causing it to wrap into a large negative 
number. Additional items can then be added to bring the total to a 
minimal positive value, allowing expensive products to be purchased 
for as little as $0.03.

---

## How I exploited it

1. Added an item to the cart and intercepted the POST request in 
   Burp Suite
2. Sent the request to Burp Intruder and configured it to repeat 
   200 times with quantity set to maximum (99) per request
3. The cumulative quantity caused an integer overflow, flipping the 
   cart total to **-$21,323,869.53**
4. Added a second low-cost item repeatedly until the negative total 
   cancelled out to **$32.15**
5. Successfully checked out a $1337.00 jacket for $32.15

---

## Real World Impact
An attacker could purchase high-value products for near zero cost, 
causing direct financial loss to the business. This vulnerability 
requires no authentication bypass or special privileges — any 
customer can exploit it.

---

## How to fix it
- Validate that cart totals cannot go below zero server-side
- Implement maximum quantity limits enforced server-side, not 
  just client-side
- Monitor for unusual cart manipulation patterns

screenshoots

<img width="1366" height="768" alt="Screenshot (106)" src="https://github.com/user-attachments/assets/4d5348e1-69f7-444c-b02e-de6d2d799fbb" />

<img width="1366" height="768" alt="Screenshot (108)" src="https://github.com/user-attachments/assets/7ca5775e-7df2-4b4b-8f65-3ab0e148864c" />

<img width="1366" height="768" alt="Screenshot (109)" src="https://github.com/user-attachments/assets/90737f95-b00c-4c80-b97a-2bfe1ca6b970" />

<img width="1366" height="768" alt="Screenshot (110)" src="https://github.com/user-attachments/assets/81d44725-4d9a-4def-bb7e-48f82443e80e" />



## Screenshots
[Add screenshots here]

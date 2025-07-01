# Lab: CSRF Vulnerability with No Defenses

This lab demonstrates a basic Cross-Site Request Forgery (CSRF) attack where there are no defenses implemented by the application. The goal is to change the victim's email address by tricking them into visiting a malicious page.

**🧪 Lab Credentials:**  
Username: `wiener`  
Password: `peter`

---

## ✅ Goal  
Exploit the CSRF vulnerability to change the victim's email address without their consent.

---

## ⚙️ Steps to Exploit

1. Log in using `wiener:peter`.
2. Navigate to the email change functionality (`/my-account`) and change your own email. Capture the request using Burp Suite.
3. In Burp, locate the captured POST request to `/my-account/change-email` and note the structure.
4. If using Burp Suite Professional, right-click → Engagement Tools → Generate CSRF PoC.  
   Or if using the Community Edition, manually craft a CSRF HTML form as below:
   
```html
<form method="POST" action="https://YOUR-LAB-ID.web-security-academy.net/my-account/change-email">
  <input type="hidden" name="email" value="attacker@evil.com">
</form>
<script>
  document.forms[0].submit();
</script>
```
5. go to the exploit server in the lab.
6. paste the csrf code into the body section and click store
7. now update the email to a different one that doesn't match your current one.
8. Now click deliver exploit to victim.
 if successful the victim's email will be changed and lab will he solved.

## 💥 Impact

- Allows attackers to perform unwanted actions on behalf of authenticated users.
- In this lab, an attacker can change the victim's email address without their knowledge.
- Could lead to account takeover if combined with a password reset to the new email.
- No user interaction beyond visiting the malicious page is needed.

## 🛡️ Mitigation

- Implement Anti-CSRF Tokens:
  - Generate a unique token per user/session and validate it on sensitive requests.
  - Example: `<input type="hidden" name="csrf" value="unique-token-here">`

- Use SameSite Cookie Attribute:
  - `Set-Cookie: session=abc123; SameSite=Strict;`
  - Prevents cookies from being sent with cross-site requests.

- Check `Origin` or `Referer` headers:
  - Reject requests if the header doesn't match your application domain.

- Avoid GET requests for state-changing operations:
  - Always use POST/PUT/DELETE with proper CSRF protection.

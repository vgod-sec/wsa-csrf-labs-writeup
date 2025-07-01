# Lab: CSRF where token validation depends on request method

This lab demonstrates a misconfigured CSRF defense where token validation is only enforced on certain HTTP methods (e.g., POST), allowing attackers to bypass the protection by switching to a method like GET.

**🧪 Lab Credentials:**  
Username: `wiener`  
Password: `peter`

---

## ✅ Goal  
Exploit the CSRF vulnerability by crafting a GET request that changes the victim’s email address without triggering token validation.

---

## ⚙️ Exploitation Steps

1. Log in with `wiener:peter` and go to the "My Account" page.
2. Change your own email and intercept the request in Burp Suite.
3. Send the captured POST request to **Burp Repeater**.
4. Modify the request method from POST to **GET** using the “Change request method” option.
5. Remove the CSRF token from the request and test if it still works — you’ll notice the CSRF token is no longer required for GET requests.
6. Copy the request URL with the updated parameters (email) and create the following HTML exploit:

```html
<img src="https://YOUR-LAB-ID.web-security-academy.net/my-account/change-email?email=attacker@evil.com">
```
7. Go to the **Exploit Server** provided in the lab interface.
8. In the "Body" section, paste the following HTML exploit payload:

```html
<img src="https://YOUR-LAB-ID.web-security-academy.net/my-account/change-email?email=csrfvictim@evil.com">
```
9. Click **View exploit** to test the payload on yourself.  
   - If your email changes to `csrfvictim@evil.com`, the exploit works successfully.

10. Change the email in the payload to ensure it does **not match your own account email**.

11. Click **Deliver to victim** to send the exploit and complete the lab.
12. ## 💥 Impact
- Allows attackers to bypass CSRF protection by switching request method to GET.
- Enables unauthorized actions like changing user email without a valid token.

## 🛡️ Mitigation
- Enforce CSRF token validation on **all** HTTP methods.
- Block sensitive actions via **GET**; use POST with token checks.
- Use `SameSite` cookies and validate `Origin`/`Referer` headers.

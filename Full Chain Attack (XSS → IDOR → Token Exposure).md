# 💀 Full Chain Attack: DOM XSS → IDOR → Sensitive Data Exposure

## 🧠 Summary

A multi-step attack chain was identified by combining multiple vulnerabilities: a DOM-based XSS, an IDOR in API endpoints, and insecure handling of client-side tokens. When chained together, these issues could lead to unauthorized access to sensitive user data.

---

## 🎯 Target

Web application with client-side rendering and API endpoints

---

## 🔍 Step 1: DOM-Based XSS

### Finding

The application uses unsafe JavaScript:

document.getElementById("output").innerHTML = location.hash;

User-controlled input from `location.hash` is injected directly into the DOM.

---

### PoC

https://target.com/#<img src=x onerror=alert(1)>

JavaScript execution confirmed.

---

## 🔓 Step 2: IDOR in API

### Finding

API endpoint:

GET /api/user/123

Changing the ID:

GET /api/user/124

Returns data of another user ❌

---

## 🔗 Step 3: Chaining the Attack (Conceptual)

With XSS execution, an attacker can interact with the application in the context of the victim.

This allows:

* Sending authenticated requests to vulnerable endpoints
* Accessing data exposed via IDOR

Example (conceptual):

JavaScript can trigger requests within the application context and read accessible responses.

---

## 💥 Impact

When chained together, these vulnerabilities may lead to:

* Unauthorized access to other users’ data
* Exposure of sensitive information
* Actions performed on behalf of the victim
* Potential account compromise depending on application design

---

## 🛡️ Recommendation

* Fix DOM XSS by avoiding unsafe sinks like `innerHTML`
* Implement proper authorization checks on all API endpoints
* Avoid exposing sensitive data in client-side context
* Apply defense-in-depth security strategy

---

## 🧠 Methodology Insight

* Identified DOM XSS via client-side code analysis
* Discovered IDOR through API testing
* Evaluated how vulnerabilities interact together
* Assessed combined impact rather than individual issues

---

## ✅ Conclusion

While each vulnerability is impactful on its own, chaining them significantly increases the severity. This demonstrates the importance of holistic security practices across both client-side and server-side components.

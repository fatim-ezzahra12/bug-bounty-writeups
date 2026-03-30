# ⚡ Reflected XSS with Filter Bypass via Encoded Payload

## 🧠 Summary

A reflected Cross-Site Scripting (XSS) vulnerability was identified in a search parameter. Although basic filtering was implemented, it was possible to bypass the filter using encoding techniques, allowing execution of arbitrary JavaScript.

---

## 🎯 Target

Web Application Search Functionality

---

## 🔍 Reconnaissance

While testing input fields, I discovered a search parameter reflecting user input in the response:

GET /search?q=test

The value of the parameter `q` is reflected in the HTML response without proper sanitization.

---

## ⚠️ Initial Testing

Basic payload:

<script>alert(1)</script>

Result:

* Payload was blocked or filtered ❌

---

## 🧪 Bypass Technique

I attempted to bypass the filter using HTML encoding:

Payload:
<script>alert(1)</script>

In some cases, the application decoded the input before rendering, leading to execution.

Another successful payload:

<svg/onload=alert(1)>

This payload bypassed the filter and executed JavaScript successfully.

---

## 🧪 Proof of Concept (PoC)

1. Navigate to:
   /search?q=test
2. Replace parameter with payload:
   /search?q=<svg/onload=alert(1)>
3. Observe JavaScript execution in the browser

---

## 💥 Impact

* Execution of arbitrary JavaScript
* Session hijacking via cookies theft
* Phishing attacks through injected scripts
* Defacement of web pages

---

## 🛡️ Recommendation

* Properly sanitize and encode user input
* Use context-aware output encoding
* Implement Content Security Policy (CSP)
* Avoid rendering raw user input in HTML

---

## 🧠 Methodology Insight

This vulnerability was identified by:

* Testing input reflection points
* Trying common XSS payloads
* Applying encoding techniques to bypass filters
* Observing browser behavior and execution

---

## ✅ Conclusion

Despite basic filtering, improper handling of encoded input allowed XSS execution. This demonstrates that filtering alone is insufficient without proper output encoding and security controls.

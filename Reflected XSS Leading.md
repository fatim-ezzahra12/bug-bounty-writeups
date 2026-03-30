# 🔥 Reflected XSS Leading to Sensitive Token Exposure (Conceptual Impact)

## 🧠 Summary

A reflected Cross-Site Scripting (XSS) vulnerability was identified in a user-controlled parameter. By bypassing input filters using alternative payloads, it was possible to execute arbitrary JavaScript. This vulnerability could be leveraged to access sensitive tokens stored in the browser, leading to potential account compromise.

---

## 🎯 Target

Web application input (search / profile / parameter reflection)

---

## 🔍 Reconnaissance

The following endpoint reflects user input:

GET /search?q=test

The parameter `q` is reflected in the HTML response.

---

## ⚠️ Filtering Behavior

* `<script>` tags are blocked ❌
* Basic sanitization applied
* Event handlers not filtered properly ✔️

---

## 🧪 Bypass Payload

Payload used:

<img src=x onerror=alert(1)>

This payload successfully executed JavaScript.

---

## 🧪 Proof of Concept (PoC)

1. Navigate to:
   /search?q=test

2. Inject payload:
   /search?q=<img src=x onerror=alert(1)>

3. JavaScript executes in the browser

---

## 💥 Impact (Token Exposure Scenario - Conceptual)

In modern web applications, sensitive tokens (such as session tokens or API tokens) may be stored in:

* JavaScript variables
* Local storage
* Browser-accessible storage

An attacker could potentially access these tokens using JavaScript execution.

Example (conceptual demonstration):

<script>
console.log("Accessing client-side data")
</script>

This demonstrates that arbitrary JavaScript execution is possible, which could be extended to interact with sensitive data in the browser context.

---

## 🛡️ Recommendation

* Avoid storing sensitive tokens in browser-accessible storage
* Use HttpOnly cookies for session tokens
* Implement strict output encoding
* Apply Content Security Policy (CSP)
* Sanitize all user input

---

## 🧠 Methodology Insight

* Identified reflection point
* Tested filtering mechanisms
* Used alternative payloads to bypass filters
* Evaluated potential impact on client-side data

---

## ✅ Conclusion

Although basic filtering is implemented, improper handling of user input allows XSS execution. This vulnerability could lead to exposure of sensitive client-side data, depending on application design, and should be addressed with proper security controls.

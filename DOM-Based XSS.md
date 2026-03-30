# ⚡ DOM-Based XSS via Unsafe JavaScript Handling of URL Fragment

## 🧠 Summary

A DOM-based Cross-Site Scripting (XSS) vulnerability was discovered due to unsafe handling of user-controlled input from the URL fragment. The application directly injects this data into the DOM without proper sanitization, leading to arbitrary JavaScript execution.

---

## 🎯 Target

Client-side JavaScript functionality using URL fragment (`location.hash`)

---

## 🔍 Reconnaissance

While analyzing the application’s JavaScript, I identified the following code pattern:

document.getElementById("output").innerHTML = location.hash;

The application reads data from the URL fragment and injects it directly into the DOM.

---

## ⚠️ Vulnerability Details

The `location.hash` value is fully controlled by the user and is not sanitized before being inserted into the DOM using `innerHTML`.

This allows injection of malicious HTML/JavaScript.

---

## 🧪 Proof of Concept (PoC)

1. Navigate to:
   https://target.com/#test

2. Modify URL fragment:
   https://target.com/#<img src=x onerror=alert(1)>

3. Observe that JavaScript executes in the browser

---

## 💥 Impact

* Execution of arbitrary JavaScript in user browser
* Possible session manipulation
* Phishing attacks within trusted domain
* Interaction with client-side data

---

## 🛡️ Recommendation

* Avoid using `innerHTML` with untrusted input
* Use `textContent` instead of `innerHTML`
* Sanitize user input before DOM insertion
* Implement CSP (Content Security Policy)

---

## 🧠 Methodology Insight

* Reviewed client-side JavaScript
* Identified dangerous DOM sinks (`innerHTML`)
* Traced user input sources (`location.hash`)
* Crafted payload to exploit injection point

---

## ✅ Conclusion

This DOM-based XSS vulnerability arises from unsafe handling of user-controlled data in client-side JavaScript. Proper DOM manipulation techniques and input validation are required to mitigate this issue.

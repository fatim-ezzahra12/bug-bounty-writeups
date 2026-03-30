# 🔓 IDOR Vulnerability in API Endpoint Leading to Unauthorized Data Access

## 🧠 Summary

A critical Insecure Direct Object Reference (IDOR) vulnerability was discovered in an API endpoint that allows unauthorized access to other users' sensitive data by manipulating object identifiers.

---

## 🎯 Target

Web Application API (e-commerce / user dashboard)

---

## 🔍 Reconnaissance

During testing, I used Burp Suite to intercept HTTP requests while browsing user-related functionalities (orders, profile, etc).

I identified the following API endpoint:

GET /api/orders/123

The request returns order details associated with the authenticated user.

---

## ⚠️ Vulnerability Details

The application does not properly validate whether the requested resource belongs to the authenticated user.

By modifying the order ID:

GET /api/orders/124

I was able to access another user's order information without authorization.

---

## 🧪 Proof of Concept (PoC)

1. Login as a normal user
2. Intercept request:
   GET /api/orders/123
3. Change ID:
   GET /api/orders/124
4. Observe that the response contains data of another user

---

## 💥 Impact

* Unauthorized access to sensitive user data
* Potential exposure of personal information
* Possible escalation to account takeover in certain scenarios

---

## 🛡️ Recommendation

* Implement proper access control checks on the server side
* Ensure that users can only access their own resources
* Use indirect references (UUIDs) instead of incremental IDs

---

## 🧠 Methodology Insight

This vulnerability was identified by:

* Intercepting requests using Burp Suite
* Testing for ID manipulation across API endpoints
* Observing inconsistent authorization controls

---

## ✅ Conclusion

This IDOR vulnerability demonstrates a lack of proper authorization checks in the API, allowing attackers to access sensitive data of other users. Fixing this issue is critical to ensure user data privacy and application security.

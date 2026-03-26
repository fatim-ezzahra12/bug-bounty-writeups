# Reconnaissance Methodology for Bug Bounty

## 🎯 Objective

The goal of reconnaissance is to discover as many assets, endpoints, and entry points as possible in order to identify potential vulnerabilities.

---

## 🌐 1. Asset Discovery

* Identify main domain (e.g., example.com)
* Enumerate subdomains using tools like:

  * subfinder
  * assetfinder

Example:
subfinder -d example.com

---

## 🔍 2. Subdomain Enumeration

* Collect as many subdomains as possible
* Look for:

  * dev.example.com
  * api.example.com
  * staging.example.com

These are often vulnerable.

---

## 📂 3. Endpoint Discovery

* Use tools like:

  * ffuf
  * dirsearch

Example:
ffuf -u https://example.com/FUZZ -w wordlist.txt

Look for:

* /api/
* /admin/
* /uploads/
* /debug/

---

## 🔑 4. Parameter Discovery

* Find hidden parameters using:

  * Burp Suite
  * Param Miner

Look for:

* id=
* user_id=
* redirect=
* file=

---

## 🧪 5. Testing Phase

Test each endpoint for:

* XSS
* IDOR
* SSRF
* SQL Injection

---

## 📦 6. Analyze JavaScript Files

* Look for:

  * API endpoints
  * Secrets / tokens
  * Hidden routes

---

## 📊 7. Prioritization

Focus on:

* Authenticated features
* APIs
* File uploads
* Admin panels

---

## 🧠 Notes

A good recon process increases the chances of finding critical vulnerabilities like RCE, IDOR, and SSRF.

Consistency and automation are key to success in bug bounty hunting.

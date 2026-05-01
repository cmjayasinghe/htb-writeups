# Task 2: Hypertext Transfer Protocol Secure (HTTPS)

| Field | Details |
|-------|---------|
| **Module** | Web Requests — HTTPS Fundamentals |
| **Task Number** | 2 of 4 |
| **Platform** | Hack The Box Academy |
| **Difficulty** | Beginner |
| **Date Completed** | 2026-05-01 |

---

## What is HTTPS and Why Does it Exist?

HTTP sends all data in **cleartext**, meaning anyone on the same network can intercept and read it — including passwords and sensitive data. This is called a **Man-in-the-Middle (MitM) attack**.

**HTTPS** fixes this by wrapping HTTP inside **TLS encryption**, making intercepted traffic unreadable. It uses port **443** instead of HTTP's port **80**, and is identified by the `https://` prefix and padlock icon in the browser.

---

## How the TLS Handshake Works

When you visit an HTTPS site, before any real data is sent:

```
1. Client connects on port 80 → Server replies: 301 redirect to port 443
2. Client sends "Hello" → Server replies with its SSL Certificate
3. Client verifies the certificate is valid and trusted
4. Both sides exchange keys and establish an encrypted session
5. Normal HTTP communication continues — now fully encrypted
```

---

## cURL and HTTPS

cURL handles HTTPS automatically. If the target has an **invalid or self-signed SSL certificate** (common in HTB labs), cURL will block the request:

```bash
curl https://target.com
# curl: (60) SSL certificate problem: Invalid certificate chain
```

Use the `-k` flag to bypass certificate checking in lab environments:

```bash
curl -k https://target.com
```

> ⚠️ Only use `-k` in controlled lab/testing environments — never against real production sites.

---

## Key Takeaways

| Concept | Summary |
|---------|---------|
| HTTP flaw | Sends data in cleartext — vulnerable to interception |
| HTTPS | Encrypts all traffic using TLS — unreadable if intercepted |
| Default ports | HTTP → 80 / HTTPS → 443 |
| 301 redirect | Servers automatically redirect HTTP → HTTPS |
| `curl -k` | Skips SSL verification — for HTB labs only |
| DNS leak | HTTPS encrypts content but DNS queries can still reveal visited domains |

---

## References
- [HTB Academy — Web Requests](https://academy.hackthebox.com/module/35)
- [curl SSL Documentation](https://curl.se/docs/sslcerts.html)

---
*Module: Web Requests | Task 2/4 — HTTPs Fundamentals*  
*Author: [Chamikara Mihiranga Jayasinghe] | [https://github.com/cmjayasinghe]*


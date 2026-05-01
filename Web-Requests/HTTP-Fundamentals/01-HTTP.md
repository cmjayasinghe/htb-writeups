# Task 1: HyperText Transfer Protocol (HTTP)

| Field | Details |
|-------|---------|
| **Module** | Web Requests |
| **Section** | HTTP Fundamentals |
| **Task Number** | 1 of 4 |
| **Type** | Interactive |
| **Platform** | Hack The Box Academy |
| **Difficulty** | Beginner |
| **Date Completed** | 2026-05-01 |

---

## Objective

> Use cURL to download the file returned by `/download.php` on the target server and retrieve the flag.

---

## Concepts Covered

- What HTTP is and how client-server communication works
- URL structure and its components (scheme, host, port, path, query string, fragment)
- How DNS resolution works before an HTTP request is made
- Using `curl` from the command line to send HTTP GET requests
- Key curl flags: `-O`, `-o`, `-s`, `-v`, `-i`, `-h`

---

## Theory Summary

### What is HTTP?

HTTP (HyperText Transfer Protocol) is an **application-level protocol** used to access resources on the World Wide Web. It works on a **client-server model**:

- The **client** (e.g. your browser or curl) sends a request
- The **server** processes it and returns a response
- Default port: **80** for HTTP, **443** for HTTPS

### URL Structure

Every HTTP resource is accessed via a URL. Here is what each part means:

```
http://admin:password@inlanefreight.com:80/dashboard.php?login=true#status
 ^         ^               ^            ^        ^             ^         ^
Scheme  User Info         Host         Port     Path        Query     Fragment
```

| Component | Example | Required? | Description |
|-----------|---------|-----------|-------------|
| Scheme | `http://` | ✅ Yes | Identifies the protocol |
| User Info | `admin:password@` | ❌ No | Optional credentials |
| Host | `inlanefreight.com` | ✅ Yes | Server hostname or IP address |
| Port | `:80` | ❌ No | Defaults to 80 (HTTP) or 443 (HTTPS) |
| Path | `/dashboard.php` | ❌ No | File or folder on the server |
| Query String | `?login=true` | ❌ No | Parameters passed to the server |
| Fragment | `#status` | ❌ No | Client-side anchor, not sent to server |

### HTTP Flow

When you visit a URL, the following happens:

```
1. You type inlanefreight.com in your browser
        |
        v
2. Browser checks /etc/hosts for a local record
        |
        v
3. If not found → DNS query sent to resolve domain → IP returned
        |
        v
4. Browser sends GET / HTTP/1.1 to port 80 of that IP
        |
        v
5. Web server processes request → returns HTTP response (e.g. 200 OK)
        |
        v
6. Browser renders the HTML
```

> 💡 **Note:** You can manually add DNS records to `/etc/hosts` to map an IP to a domain name without going through a DNS server. This is commonly used in HTB machines.

### What is cURL?

`curl` (Client URL) is a **command-line tool** for making HTTP requests. Unlike a browser, it prints the raw HTML response — which is exactly what penetration testers need.

Key flags used in this task:

| Flag | Long Form | Description |
|------|-----------|-------------|
| `-O` | `--remote-name` | Save output to a file using the remote filename |
| `-o` | `--output <file>` | Save output to a specific filename you choose |
| `-s` | `--silent` | Suppress progress bar and status output |
| `-v` | `--verbose` | Show full request and response headers |
| `-i` | `--include` | Include response headers in the output |
| `-h` | `--help` | Show help menu |

---

## Solution

### Step 1: Test Basic Connectivity

Before downloading anything, confirm the server is reachable with a basic GET request:

```bash
curl http://<TARGET_IP>:<PORT>/
```

Web-Requests/Screenshots/1/1.png

This confirms the server is up and responding. The `-v` flag can be added to see the full HTTP exchange if needed.

Web-Requests/Screenshots/1/2.png

---

### Step 2: Download the File from /download.php

The task asks us to download the file returned by `/download.php`. We use the `-O` flag so curl saves it automatically using the server's filename:

```bash
curl -O http://<TARGET_IP>:<PORT>/download.php
```

Web-Requests/Screenshots/1/3.png

The file is now saved in your current directory.

---

### Step 3: Confirm the File Was Downloaded

```bash
ls -la
```

Web-Requests/Screenshots/1/4.png

---

### Step 4: Read the File to Find the Flag

```bash
cat download.php
```

Web-Requests/Screenshots/1/5.png

---

### Alternative: Silent Download (No Progress Bar)

If you want to suppress the progress bar output (useful in scripts):

```bash
curl -s -O http://<TARGET_IP>:<PORT>/download.php
cat download.php
```

Web-Requests/Screenshots/1/6.png

---

## Commands Used — Quick Reference

```bash
# Connect to VPN
sudo openvpn ~/Downloads/htb-academy.ovpn

# Test server connectivity
curl http://<TARGET_IP>:<PORT>/

# Download file using remote filename
curl -O http://<TARGET_IP>:<PORT>/download.php

# Download file silently
curl -s -O http://<TARGET_IP>:<PORT>/download.php

# Read the downloaded file
cat download.php

# Verbose output to inspect HTTP headers
curl -v http://<TARGET_IP>:<PORT>/download.php
```

---

## Key Takeaways

| Concept | What I Learned |
|---------|----------------|
| HTTP default port | Port **80** for HTTP; **443** for HTTPS |
| DNS resolution | Happens before any HTTP connection — domain → IP |
| `curl` vs browser | curl prints raw response; browsers render HTML |
| `-O` flag | Downloads and saves with the server's filename |
| `-s` flag | Removes the progress bar — useful for clean scripting |
| `-v` flag | Shows full request/response — essential for debugging |
| `/etc/hosts` | Can be used to manually map IPs to hostnames |
| HTTP flow | DNS → TCP connection → GET request → Server response |

---

## Security Insight

HTTP transmits **all data in plaintext**, including login credentials, session tokens, and sensitive content. Any attacker on the same network can intercept this traffic using a tool like Wireshark — this is called a **Man-in-the-Middle (MitM) attack**. This is why **HTTPS** (HTTP over TLS encryption) is critical for any website handling sensitive data.

---

## References

- [HTB Academy — Web Requests Module](https://academy.hackthebox.com/module/35)
- [curl Official Documentation](https://curl.se/docs/manpage.html)
- [MDN — HTTP Overview](https://developer.mozilla.org/en-US/docs/Web/HTTP/Overview)
- [RFC 7230 — HTTP/1.1 Message Syntax](https://tools.ietf.org/html/rfc7230)

---

*Module: Web Requests | Task 1/4 — HTTP Fundamentals*  
*Author: [Chamikara Mihiranga Jayasinghe] | [[Your GitHub Profile URL](https://github.com/cmjayasinghe)]*

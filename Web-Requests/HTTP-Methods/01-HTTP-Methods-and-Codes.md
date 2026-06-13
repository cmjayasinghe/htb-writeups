# HTTP Methods and Status Codes

| Field              | Details                |
| ------------------ | ---------------------- |
| **Module**         | Web Requests           |
| **Section**        | HTTP Methods and Codes |
| **Platform**       | Hack The Box Academy   |
| **Difficulty**     | Beginner               |
| **Date Completed** | 2026-06-13             |

---

## Objective

Understand the purpose of different HTTP request methods and HTTP response status codes, and learn how they are used in web applications and APIs.

---

## Concepts Covered

* HTTP Request Methods
* GET and POST Requests
* PUT and DELETE Methods
* HEAD, OPTIONS, and PATCH Methods
* HTTP Response Status Codes
* Success, Redirection, Client Errors, and Server Errors
* Using cURL to test HTTP methods
* Interpreting HTTP responses

---

# Theory Summary

## HTTP Request Methods

HTTP methods define the action that a client wants the server to perform on a specific resource.

Different methods are used depending on whether the client wants to retrieve, submit, update, or delete data.

### GET

The `GET` method requests data from a server.

Characteristics:

* Most common HTTP method
* Parameters are sent in the URL
* Used to retrieve resources
* Should not modify data on the server

Example:

```http
GET /index.php HTTP/1.1
Host: example.com
```

cURL Example:

```bash
curl http://example.com
```

---

### POST

The `POST` method sends data to the server.

Characteristics:

* Data is placed in the request body
* Commonly used for login forms
* Used when submitting information
* Can upload files and binary data

Example:

```http
POST /login.php HTTP/1.1
Host: example.com

username=admin&password=password
```

cURL Example:

```bash
curl -X POST -d "username=admin&password=password" http://example.com/login.php
```

---

### HEAD

The `HEAD` method retrieves only response headers.

Characteristics:

* No response body returned
* Useful for checking resource availability
* Can be used to determine file size before downloading

cURL Example:

```bash
curl -I http://example.com
```

---

### PUT

The `PUT` method uploads or replaces resources on the server.

Characteristics:

* Commonly used in REST APIs
* Can create or update resources
* Dangerous if improperly configured

Example:

```bash
curl -X PUT -d "data=test" http://example.com/api/user
```

Security Risk:

* Unrestricted file uploads
* Web shell uploads
* Unauthorized modifications

---

### DELETE

The `DELETE` method removes resources from the server.

Characteristics:

* Commonly used in APIs
* Deletes existing data

Example:

```bash
curl -X DELETE http://example.com/api/user/1
```

Security Risk:

* Data loss
* Denial of Service (DoS)

---

### OPTIONS

The `OPTIONS` method displays supported HTTP methods.

Example:

```bash
curl -X OPTIONS http://example.com -i
```

Useful for:

* Reconnaissance
* API testing
* Identifying allowed methods

---

### PATCH

The `PATCH` method modifies part of an existing resource.

Characteristics:

* Partial updates
* Common in REST APIs

Example:

```bash
curl -X PATCH -d '{"email":"new@example.com"}' http://example.com/api/user/1
```

---

# HTTP Status Codes

HTTP status codes inform the client about the result of a request.

---

## 1xx Informational

The request has been received and processing continues.

Examples:

* 100 Continue
* 101 Switching Protocols

---

## 2xx Success

The request completed successfully.

### 200 OK

Most common success response.

Example:

```http
HTTP/1.1 200 OK
```

Meaning:

* Request succeeded
* Resource returned successfully

---

## 3xx Redirection

The client must perform additional actions to access the resource.

### 302 Found

Temporary redirect to another URL.

Example:

```http
HTTP/1.1 302 Found
Location: /dashboard
```

Common Use:

* Redirecting users after login

---

## 4xx Client Errors

Indicates a problem with the client's request.

### 400 Bad Request

Malformed or invalid request.

Example:

```http
HTTP/1.1 400 Bad Request
```

---

### 403 Forbidden

Access denied.

Example:

```http
HTTP/1.1 403 Forbidden
```

Possible Causes:

* Missing permissions
* WAF blocking requests
* Restricted content

---

### 404 Not Found

Requested resource does not exist.

Example:

```http
HTTP/1.1 404 Not Found
```

Common During:

* Directory enumeration
* Broken links
* Resource discovery

---

## 5xx Server Errors

The server encountered an error while processing the request.

### 500 Internal Server Error

General server-side failure.

Example:

```http
HTTP/1.1 500 Internal Server Error
```

Possible Causes:

* Application crashes
* Database failures
* Coding errors

---

# Practical Examples Using cURL

### View Response Headers

```bash
curl -I http://example.com
```

---

### Verbose Request

```bash
curl -v http://example.com
```

This displays:

* HTTP Method
* Request Headers
* Response Headers
* Status Code

---

### Send POST Data

```bash
curl -X POST \
-d "username=admin&password=admin" \
http://example.com/login
```

---

### Check Supported Methods

```bash
curl -X OPTIONS http://example.com -i
```

---

# Security Insights

Understanding HTTP methods is essential during web application penetration testing.

Common findings include:

* PUT method allowing file uploads
* DELETE method exposed without authentication
* Misconfigured APIs accepting unauthorized requests
* Sensitive information leaked through HTTP responses
* Verbose error messages returned with 500 responses

HTTP status codes also provide valuable information during reconnaissance and vulnerability assessment.

Examples:

| Status Code | Information Revealed                           |
| ----------- | ---------------------------------------------- |
| 200         | Resource exists                                |
| 301/302     | Redirect detected                              |
| 403         | Resource exists but access denied              |
| 404         | Resource likely does not exist                 |
| 500         | Application error may indicate vulnerabilities |

---

# Key Takeaways

| Concept | What I Learned                                    |
| ------- | ------------------------------------------------- |
| GET     | Retrieves data from a server                      |
| POST    | Sends data to a server                            |
| HEAD    | Returns headers only                              |
| PUT     | Creates or updates resources                      |
| DELETE  | Removes resources                                 |
| OPTIONS | Lists supported methods                           |
| PATCH   | Modifies part of a resource                       |
| 2xx     | Successful requests                               |
| 3xx     | Redirections                                      |
| 4xx     | Client-side errors                                |
| 5xx     | Server-side errors                                |
| cURL    | Useful for testing and manipulating HTTP requests |

---

## References

* HTB Academy – Web Requests
* RFC 7231: Hypertext Transfer Protocol
* MDN Web Docs – HTTP Methods
* MDN Web Docs – HTTP Status Codes

---

*Module: Web Requests | Task 5/8 — HTTP Methods and Codes*  
*Author: [Chamikara Mihiranga Jayasinghe] | [https://github.com/cmjayasinghe]*


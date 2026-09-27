# Day 11 — DNS, DHCP, HTTP & HTTPS

## Objective

Understand:

- DNS resolution
- DNS record types
- DHCP lease process
- HTTP requests and responses
- HTTP headers
- HTTP status codes
- HTTPS and TLS
- `nslookup`
- `dig`
- `curl -v`

---

# 1. DNS

DNS stands for **Domain Name System**.

DNS translates domain names into IP addresses.

For example:

```text
google.com
    ↓
DNS resolution
    ↓
IP address
```

Instead of remembering an IP address, users can access a service using a domain name.

---

# 2. DNS Resolution Process

A simplified DNS resolution process:

```text
Browser / Application
        |
        | "What is the IP of google.com?"
        ↓
DNS Resolver
        |
        ↓
Root DNS
        |
        ↓
TLD DNS (.com)
        |
        ↓
Authoritative DNS
        |
        ↓
IP address
        |
        ↓
Client
```

In practice, caching can make the process shorter because a resolver may already have the answer.

---

# 3. Important DNS Records

| Record | Purpose |
|---|---|
| A | IPv4 address |
| AAAA | IPv6 address |
| CNAME | Alias for another domain name |
| MX | Mail server |
| NS | Authoritative name server |
| TXT | Text/verification information |

Examples:

```bash
dig google.com A
dig google.com AAAA
dig google.com MX
dig google.com NS
dig google.com TXT
```

---

# 4. DNS Practical

Run:

```bash
nslookup google.com
```

Then:

```bash
dig google.com
```

Useful short output:

```bash
dig google.com +short
```

Compare the outputs.

Questions to answer:

- Which DNS server answered?
- What IP address was returned?
- Is the answer IPv4 or IPv6?
- What is the TTL?
- What record type was returned?

---

# 5. DHCP

DHCP stands for **Dynamic Host Configuration Protocol**.

DHCP automatically provides network configuration to a client.

It can provide:

- IP address
- Subnet mask
- Default gateway
- DNS server
- Lease duration

---

# 6. DHCP DORA Process

The basic DHCP process is called DORA:

```text
Discover
    ↓
Offer
    ↓
Request
    ↓
Acknowledge
```

## 1. DHCP Discover

The client broadcasts a request asking for network configuration.

```text
Client → DHCP Server
        DISCOVER
```

## 2. DHCP Offer

A DHCP server offers an IP configuration.

```text
Client ← DHCP Server
        OFFER
```

## 3. DHCP Request

The client requests the offered configuration.

```text
Client → DHCP Server
        REQUEST
```

## 4. DHCP Acknowledge

The server confirms the lease.

```text
Client ← DHCP Server
        ACK
```

---

# 7. DHCP Lease

A DHCP address is normally leased for a period of time.

The client can renew the lease before it expires.

Mental model:

```text
DHCP
 ↓
Give me network configuration
 ↓
IP + subnet + gateway + DNS
 ↓
Lease for a period
 ↓
Renew when required
```

---

# 8. HTTP

HTTP stands for **Hypertext Transfer Protocol**.

HTTP is an application-layer protocol used for communication between clients and web servers.

Basic model:

```text
Client
  |
  | HTTP Request
  ↓
Web Server
  |
  | HTTP Response
  ↓
Client
```

---

# 9. HTTP Request

A basic HTTP request looks like:

```text
GET / HTTP/1.1
Host: example.com
User-Agent: curl/8.x
Accept: */*
```

The first line is the **request line**:

```text
GET / HTTP/1.1
```

It contains:

```text
Method
Path
HTTP Version
```

---

# 10. Common HTTP Methods

| Method | Purpose |
|---|---|
| GET | Retrieve a resource |
| POST | Submit data |
| PUT | Replace/update a resource |
| PATCH | Partially update a resource |
| DELETE | Delete a resource |

Example:

```http
GET /login HTTP/1.1
Host: example.com
```

---

# 11. HTTP Request Headers

Common request headers include:

```text
Host:
User-Agent:
Accept:
Content-Type:
Cookie:
Authorization:
```

Example:

```text
Host: example.com
User-Agent: curl/8.13.0
Accept: */*
```

Headers provide additional information about the request.

---

# 12. HTTP Response

A server responds with an HTTP response.

Example:

```text
HTTP/1.1 200 OK
Content-Type: text/html
Content-Length: 1250

<html>
...
</html>
```

The first line is the **status line**:

```text
HTTP/1.1 200 OK
```

It contains:

```text
HTTP Version
Status Code
Reason Phrase
```

---

# 13. Common HTTP Status Codes

| Code | Meaning |
|---:|---|
| 200 | OK |
| 201 | Created |
| 301 | Moved Permanently |
| 302 | Found / Redirect |
| 400 | Bad Request |
| 401 | Unauthorized |
| 403 | Forbidden |
| 404 | Not Found |
| 500 | Internal Server Error |

Status code categories:

```text
1xx → Informational
2xx → Success
3xx → Redirection
4xx → Client errors
5xx → Server errors
```

---

# 14. HTTP Response Headers

Common response headers include:

```text
Content-Type:
Content-Length:
Location:
Server:
Set-Cookie:
Cache-Control:
```

Example:

```text
HTTP/1.1 301 Moved Permanently
Location: https://www.example.com/
Content-Type: text/html
```

---

# 15. HTTP Response Body

The response body contains the actual content returned by the server.

For a web page, it may contain HTML:

```html
<html>
    <body>
        <h1>Hello</h1>
    </body>
</html>
```

So the simplified structure is:

```text
HTTP Response
     |
     +-- Status Line
     |
     +-- Headers
     |
     +-- Blank Line
     |
     +-- Response Body
```

---

# 16. Curl Practical

Run:

```bash
curl -v http://example.com
```

The `-v` option enables verbose output.

It helps show:

- DNS resolution
- TCP connection
- HTTP request
- HTTP response
- Headers

---

# 17. How to Read curl -v

A simplified example:

```text
* Connected to example.com
> GET / HTTP/1.1
> Host: example.com
> User-Agent: curl/8.x
> Accept: */*
>
< HTTP/1.1 200 OK
< Content-Type: text/html
< Content-Length: 1256
<
<html>
...
</html>
```

Remember:

```text
> = data sent by curl/client
< = data received from server
* = curl informational output
```

---

# 18. Label the curl Output

## Request line

```text
> GET / HTTP/1.1
```

## Request headers

```text
> Host: example.com
> User-Agent: curl/8.x
> Accept: */*
```

## Status line

```text
< HTTP/1.1 200 OK
```

## Response headers

```text
< Content-Type: text/html
< Content-Length: 1256
```

## Response body

```text
<html>
...
</html>
```

---

# 19. HTTP vs HTTPS

HTTP sends application data without TLS encryption.

HTTPS is:

```text
HTTP + TLS
```

HTTPS normally uses port:

```text
443
```

HTTP commonly uses:

```text
80
```

Simplified HTTPS flow:

```text
DNS
 ↓
TCP connection to port 443
 ↓
TLS handshake
 ↓
HTTP request
 ↓
HTTP response
```

---

# 20. HTTPS Security Properties

TLS provides important security properties such as:

- Encryption
- Integrity protection
- Server authentication through certificates

A certificate helps the client verify that it is communicating with the intended hostname, subject to certificate validation.

---

# 21. Full Web Request Mental Model

When visiting:

```text
https://google.com
```

Think:

```text
Domain
  ↓
DNS
  ↓
IP address
  ↓
Routing
  ↓
TCP connection
  ↓
Port 443
  ↓
TLS handshake
  ↓
HTTP request
  ↓
HTTP response
```

---

# 22. Security Perspective

Understanding HTTP is important for web security.

Security testing often involves inspecting:

- Request methods
- URL paths
- Query parameters
- Headers
- Cookies
- Authorization headers
- Request bodies
- Response headers
- Status codes
- Response bodies

Tools such as Burp Suite allow authorized testers to inspect and modify HTTP requests and responses.

---

# 23. Practical Checklist

Run:

```bash
nslookup google.com
```

Then:

```bash
dig google.com
```

Then:

```bash
curl -v http://example.com
```

Record:

### DNS

- DNS server:
- Returned IP:
- Record type:
- TTL:

### HTTP Request

- Request method:
- Request path:
- HTTP version:
- Host:
- User-Agent:

### HTTP Response

- Status code:
- Status message:
- Content-Type:
- Content-Length:
- Location:
- Response body:

---

# 24. Commands Reference

```bash
# DNS
nslookup google.com
dig google.com
dig google.com +short
dig google.com A
dig google.com AAAA
dig google.com MX
dig google.com NS

# HTTP
curl -v http://example.com

# HTTPS
curl -v https://example.com
```

---

# 25. Key Takeaways

```text
DNS
 ↓
Domain → IP address

DHCP
 ↓
Automatically provides network configuration

HTTP
 ↓
Request → Response

HTTPS
 ↓
HTTP + TLS
```

HTTP request:

```text
Request Line
    ↓
Headers
    ↓
Blank Line
    ↓
Optional Body
```

HTTP response:

```text
Status Line
    ↓
Headers
    ↓
Blank Line
    ↓
Response Body
```

---

# 26. Progress

- [x] Learned DNS resolution
- [x] Learned DHCP DORA
- [x] Learned DHCP leases
- [x] Learned HTTP requests
- [x] Learned HTTP responses
- [x] Learned HTTP headers
- [x] Learned HTTP status codes
- [x] Practiced nslookup
- [x] Practiced dig
- [x] Practiced curl -v
- [ ] Inspect HTTP traffic in Wireshark
- [ ] Inspect HTTP requests in Burp Suite
- [ ] Complete TryHackMe Tasks 10–12

---

# Day 11 Summary

The main mental model:

```text
DHCP
  ↓
Get network configuration

DNS
  ↓
Domain → IP

HTTP
  ↓
Client → Request → Server
Client ← Response ← Server

HTTPS
  ↓
HTTP + TLS
```

Full networking-to-web flow:

```text
Domain
  ↓
DNS
  ↓
IP
  ↓
Route
  ↓
TCP
  ↓
Port 80/443
  ↓
HTTP / HTTPS
  ↓
Request
  ↓
Response

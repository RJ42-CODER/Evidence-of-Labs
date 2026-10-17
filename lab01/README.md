# Web Application Security — HTTP Fundamentals & API Analysis

> Understanding how web applications communicate, how APIs process requests, and how HTTP security controls help protect sensitive data.

## Overview

Web applications rely on HTTP to exchange information between clients and servers. Understanding this communication is essential for identifying insecure configurations, analysing application behaviour, and conducting effective web application security assessments.

This write-up documents the core concepts, protocol behaviour, security mechanisms, and practical HTTP request operations explored during hands-on learning.

---

## Table of Contents

1. [Web Application Architecture](#1-web-application-architecture)
2. [URL Structure and Security](#2-url-structure-and-security)
3. [HTTP Message Structure](#3-http-message-structure)
4. [HTTP Methods and API Operations](#4-http-methods-and-api-operations)
5. [Request Headers and Body Encoding](#5-request-headers-and-body-encoding)
6. [HTTP Responses and Status Codes](#6-http-responses-and-status-codes)
7. [Cookie Security](#7-cookie-security)
8. [HTTP Security Headers](#8-http-security-headers)
9. [Practical API Analysis](#9-practical-api-analysis)
10. [Security Takeaways](#10-security-takeaways)

---

## 1. Web Application Architecture

A web application typically consists of a client, a server-side application, and a data store.

| Component                      | Responsibility                               | Security relevance                                    |
| ------------------------------ | -------------------------------------------- | ----------------------------------------------------- |
| Web browser                    | Sends requests and renders responses         | Browser-side security and session handling            |
| Web server                     | Receives HTTP requests and serves content    | Access control, configuration and request handling    |
| Application backend            | Implements business logic and API operations | Input validation, authorization and business logic    |
| Database                       | Stores application data                      | Data confidentiality, integrity and access control    |
| Web Application Firewall (WAF) | Filters potentially malicious HTTP traffic   | Adds a layer of protection against common web attacks |

### Request lifecycle

A typical request follows this sequence:

1. A user interacts with a web application.
2. The browser constructs an HTTP request.
3. The request travels to the server, potentially through a proxy, load balancer or WAF.
4. The application processes the request and applies relevant validation and authorization checks.
5. The server returns an HTTP response.
6. The browser interprets the response and updates the interface.

**Security principle:** A WAF can block some malicious requests, but it does not replace secure application code, authentication, authorization, or server-side input validation.

---

## 2. URL Structure and Security

A Uniform Resource Locator (URL) identifies a resource and describes how to access it.

Example:

`https://example.com/api/users?id=42`

| Component    | Value         | Purpose                           |
| ------------ | ------------- | --------------------------------- |
| Scheme       | `https`       | Specifies the protocol            |
| Host         | `example.com` | Identifies the destination host   |
| Path         | `/api/users`  | Identifies the requested resource |
| Query string | `?id=42`      | Supplies additional parameters    |

### HTTPS

HTTPS uses HTTP over TLS to protect communication in transit.

It provides:

- **Confidentiality:** Helps prevent unauthorized parties from reading traffic.
- **Integrity:** Helps detect modification of transmitted data.
- **Server authentication:** Allows the client to verify the server's certificate and identity, subject to correct validation.

HTTPS does not automatically prevent application vulnerabilities, phishing, or unauthorized access.

### Typosquatting

Typosquatting involves registering deceptive domain names that resemble legitimate domains, often exploiting typing mistakes or visual similarity.

**Security practice:** Verify domains carefully before entering credentials or sensitive information.

### Query strings

Query strings commonly contain search terms, filters, identifiers, or other parameters.

For example:

`/products?category=books&page=2`

Sensitive values should not be placed in URLs unnecessarily because URLs may appear in browser history, server logs, analytics, and referrer information.

---

## 3. HTTP Message Structure

HTTP uses a request-response model. The client sends a request, and the server returns a response.

### HTTP request

```http
GET /api/users HTTP/1.1
Host: example.com
Accept: application/json

```

A request consists of:

1. **Request line:** Method, target path and HTTP version.
2. **Headers:** Metadata about the request.
3. **Blank line:** Separates headers from the message body.
4. **Optional body:** Contains submitted data when required.

### HTTP response

```http
HTTP/1.1 200 OK
Content-Type: application/json

{"message":"Request successful"}
```

A response consists of:

1. **Status line:** HTTP version, status code and reason phrase.
2. **Headers:** Metadata about the response.
3. **Blank line:** Separates headers from the response body.
4. **Optional body:** Contains returned data or content.

### Why this matters in security testing

Inspecting raw HTTP messages helps identify:

- Unexpected parameters and exposed information.
- Missing or inconsistent security headers.
- Session cookies and their attributes.
- Differences between successful and failed requests.
- Whether server responses disclose implementation details.

---

## 4. HTTP Methods and API Operations

HTTP methods describe the intended operation on a resource. The actual behaviour depends on the server implementation and authorization rules.

| Method    | Typical purpose                             | Security consideration                         |
| --------- | ------------------------------------------- | ---------------------------------------------- |
| `GET`     | Retrieve a resource                         | Sensitive data must still be access-controlled |
| `POST`    | Submit data or trigger an operation         | Validate input and enforce authorization       |
| `PUT`     | Create or replace a resource representation | Prevent unauthorized modification              |
| `PATCH`   | Partially modify a resource                 | Restrict which fields a user may change        |
| `DELETE`  | Request resource deletion                   | Verify permissions and protect critical data   |
| `OPTIONS` | Discover communication options              | Avoid exposing unnecessary information         |

### Important distinction

- **Safe methods:** Intended not to change application state; `GET` is the common example.
- **Idempotent methods:** Repeating the same request has the same intended effect as making it once. `PUT` and `DELETE` are defined as idempotent, although responses may differ.
- **POST:** Is not generally defined as idempotent.

These properties describe HTTP semantics, not a guarantee that every application implements them correctly.

### API endpoints

An API endpoint is a resource or operation exposed through an application interface.

For example:

```text
GET    /api/users
POST   /api/user/2
DELETE /api/user/1
```

The endpoint and HTTP method together help determine the intended operation.

**Security takeaway:** Changing an identifier in a URL must not grant access to another user's data. Applications need server-side object-level authorization checks.

---

## 5. Request Headers and Body Encoding

HTTP headers carry metadata that helps clients and servers interpret messages.

| Header           | Purpose                                                      |
| ---------------- | ------------------------------------------------------------ |
| `Host`           | Identifies the target host in HTTP/1.1                       |
| `User-Agent`     | Describes the client software                                |
| `Accept`         | Indicates response media types the client can process        |
| `Content-Type`   | Describes the media type of the message body                 |
| `Content-Length` | Specifies the body length in bytes when used                 |
| `Authorization`  | Carries authentication credentials or tokens when applicable |
| `Cookie`         | Sends applicable cookies to the server                       |

### URL-encoded form data

A common form submission format is:

`application/x-www-form-urlencoded`

Example:

```http
POST /api/user/2 HTTP/1.1
Host: example.com
Content-Type: application/x-www-form-urlencoded

country=US
```

Here, `country=US` is the request body. It is not a URL query parameter because it appears after the blank line separating the headers from the body.

### JSON request bodies

APIs may instead accept JSON:

```http
POST /api/user/2 HTTP/1.1
Host: example.com
Content-Type: application/json

{"country":"US"}
```

The correct format depends on the API contract. A server may reject a request if its body format or `Content-Type` does not match what the endpoint expects.

**Security takeaway:** Never assume that changing a request field is authorized simply because the server accepts the request format. Validate input and enforce permissions on the server.

---

## 6. HTTP Responses and Status Codes

HTTP status codes communicate the outcome of a request.

| Status                      | Meaning                                    | Typical interpretation                          |
| --------------------------- | ------------------------------------------ | ----------------------------------------------- |
| `200 OK`                    | Request succeeded                          | A successful operation or retrieval             |
| `201 Created`               | Resource created                           | A new resource was created                      |
| `204 No Content`            | Request succeeded without response content | Common for successful deletion or updates       |
| `301` / `302`               | Redirection                                | Client may be directed elsewhere                |
| `400 Bad Request`           | Invalid request                            | Malformed syntax or invalid input               |
| `401 Unauthorized`          | Authentication required or failed          | Valid authentication is missing or unsuccessful |
| `403 Forbidden`             | Request refused                            | The server declines to authorize the operation  |
| `404 Not Found`             | Resource not found                         | The resource is unavailable at that target      |
| `405 Method Not Allowed`    | Method unsupported for that resource       | The endpoint does not allow that method         |
| `429 Too Many Requests`     | Rate limit exceeded                        | Requests are arriving too frequently            |
| `500 Internal Server Error` | Server encountered an unexpected condition | Application or server-side failure              |

### What to inspect during testing

- Does a successful request return the expected content?
- Does invalid input produce a controlled error?
- Are stack traces or internal implementation details exposed?
- Are authentication and authorization failures handled appropriately?
- Does the response disclose sensitive data unnecessarily?

A status code alone does not prove that an operation succeeded securely. Inspect the response body and verify the resulting application state where appropriate.

---

## 7. Cookie Security

Cookies are frequently used to maintain sessions and store application-related state.

The `Set-Cookie` response header instructs the browser to store a cookie.

### Important cookie attributes

| Attribute             | Purpose                                                    |
| --------------------- | ---------------------------------------------------------- |
| `Secure`              | Restricts cookie transmission to secure HTTPS connections  |
| `HttpOnly`            | Prevents access through JavaScript's `document.cookie` API |
| `SameSite`            | Controls when cookies are included in cross-site requests  |
| `Path`                | Limits the URL paths for which a cookie is sent            |
| `Domain`              | Controls the hosts to which a cookie applies               |
| `Expires` / `Max-Age` | Controls cookie lifetime                                   |

Example:

```http
Set-Cookie: session=REDACTED; Secure; HttpOnly; SameSite=Lax
```

The example uses a placeholder, not a real session credential.

### Why these attributes matter

- `Secure` reduces the risk of session cookies being transmitted over unencrypted HTTP.
- `HttpOnly` makes cookie theft through direct JavaScript cookie access more difficult, but does not prevent all consequences of cross-site scripting (XSS).
- `SameSite` can reduce some cross-site request forgery (CSRF) risks, depending on the application's design and cookie settings.

**Security takeaway:** Cookie attributes are complementary controls. They should be combined with appropriate session management, CSRF defences, HTTPS, and XSS prevention.

---

## 8. HTTP Security Headers

Security headers communicate policies that browsers use when handling application content.

### Content-Security-Policy (CSP)

CSP restricts the sources from which certain types of content may be loaded or executed.

Example:

```http
Content-Security-Policy: default-src 'self'; script-src 'self'
```

- `default-src` defines a fallback policy for resource types.
- `script-src` controls permitted script sources.

A restrictive, correctly designed policy can reduce the impact of some XSS vulnerabilities. The example is illustrative; a real policy must account for the application's legitimate resources.

### Strict-Transport-Security (HSTS)

HSTS instructs browsers to use HTTPS for a domain for a specified period.

Example:

```http
Strict-Transport-Security: max-age=31536000; includeSubDomains
```

- `max-age` specifies how long the policy remains active.
- `includeSubDomains` extends the policy to subdomains.

Only deploy this policy after confirming HTTPS works correctly across the intended domain scope.

### X-Content-Type-Options

Example:

```http
X-Content-Type-Options: nosniff
```

The `nosniff` directive prevents supported browsers from interpreting a response as a different MIME type in relevant contexts.

This helps reduce certain content-type confusion risks, particularly when combined with correct `Content-Type` headers.

### Server disclosure

A response may contain a header such as:

```http
Server: example-server
```

Detailed server version information can help an attacker identify potentially vulnerable software. Minimizing unnecessary version disclosure is useful, but hiding a banner does not fix an underlying vulnerability.

---

## 9. Practical API Analysis

The following examples illustrate the three request operations practised during the exercise. They use placeholder hostnames and contain no live credentials.

### A. Retrieve a user list

```http
GET /api/users HTTP/1.1
Host: example.com
Accept: application/json
```

**Purpose:** Request the user-list resource.

**What to examine:**

- Response status and content type.
- Structure of the returned data.
- Whether sensitive fields are exposed.
- Whether access controls are applied to the endpoint.

### B. Modify a user's country

```http
POST /api/user/2 HTTP/1.1
Host: example.com
Content-Type: application/x-www-form-urlencoded

country=US
```

**Purpose:** Submit a country value for the selected user in an API that supports this operation.

**What to examine:**

- Whether the endpoint accepts the expected body format.
- Whether the response indicates success or failure.
- Whether the authenticated user is authorized to modify that specific record.
- Whether the server validates permitted field values.

Changing a user identifier or submitted field must not bypass object-level authorization or field-level access controls.

### C. Request deletion of a user

```http
DELETE /api/user/1 HTTP/1.1
Host: example.com
```

**Purpose:** Request deletion of the specified user resource.

**What to examine:**

- Whether the endpoint supports the method.
- Whether the caller has sufficient permissions.
- Whether the response accurately reflects the outcome.
- Whether destructive operations have suitable safeguards and audit logging.

**Practical lesson:** HTTP methods, endpoint paths, headers, body encoding, and server-side access controls must work together for an API to behave securely.

---

## 10. Practical Evidence

The screenshots below document the hands-on request analysis performed during this exercise.

### Evidence 1

![HTTP request analysis — screenshot 1](<Screenshot 2026-10-17 151313.png>)

### Evidence 2

![HTTP request analysis — screenshot 2](<Screenshot 2026-10-17 151418.png>)

### Evidence 3

![HTTP request analysis — screenshot 3](<Screenshot 2026-10-17 151500.png>)

---

## Key Security Takeaways

1. **Understand the complete HTTP message:** The method, target, headers, body and response all provide useful diagnostic information.
2. **Treat API authorization as a server-side responsibility:** Knowing an endpoint or changing an object identifier must not grant additional permissions.
3. **Validate input and restrict writable fields:** Applications should accept only expected values and authorized changes.
4. **Protect session cookies:** Use appropriate attributes and robust session management.
5. **Configure security headers deliberately:** CSP, HSTS and `nosniff` address different browser-side risks.
6. **Do not rely on status codes alone:** Validate the response content and actual application behaviour.
7. **Minimize information disclosure:** Avoid exposing sensitive data, credentials, unnecessary software details or internal error messages.

## Further Reading

- [MDN — HTTP Overview](https://developer.mozilla.org/en-US/docs/Web/HTTP)
- [MDN — HTTP Request Methods](https://developer.mozilla.org/en-US/docs/Web/HTTP/Reference/Methods)
- [MDN — HTTP Status Codes](https://developer.mozilla.org/en-US/docs/Web/HTTP/Reference/Status)
- [MDN — Set-Cookie](https://developer.mozilla.org/en-US/docs/Web/HTTP/Reference/Headers/Set-Cookie)
- [OWASP — Web Security Testing Guide](https://owasp.org/www-project-web-security-testing-guide/)

---

_Personal technical notes documenting concepts studied and practical HTTP request analysis._

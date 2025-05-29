
# ❗ Important Application Error Codes

This document provides a consolidated and prioritized list of critical application error codes used in web applications, system-level operations, and databases.

---

## ✅ HTTP Status Codes

| Code | Description |
|------|-------------|
| **200** | OK – Successful response |
| **201** | Created – Resource created successfully |
| **204** | No Content – Success with no response body |
| **400** | Bad Request – Client sent invalid request |
| **401** | Unauthorized – Missing or invalid credentials |
| **403** | Forbidden – Authenticated but not authorized |
| **404** | Not Found – Resource not found |
| **405** | Method Not Allowed – Invalid HTTP method used |
| **408** | Request Timeout – Client took too long |
| **409** | Conflict – State conflict, like duplicate record |
| **429** | Too Many Requests – Rate limit exceeded |
| **500** | Internal Server Error – Unspecified server issue |
| **502** | Bad Gateway – Invalid response from upstream |
| **503** | Service Unavailable – Server overloaded or down |
| **504** | Gateway Timeout – Upstream server unresponsive |

---

## 🖥️ Linux System Error Codes

| Code | Description |
|------|-------------|
| **EPERM (1)** | Operation not permitted |
| **ENOENT (2)** | No such file or directory |
| **EACCES (13)** | Permission denied |
| **ENOSPC (28)** | No space left on device |
| **ECONNREFUSED** | Connection refused |
| **EHOSTUNREACH** | No route to host |

---

## 🛢️ Database Error Codes (MySQL/PostgreSQL)

| Code | Description |
|------|-------------|
| **ER_DUP_ENTRY** | Duplicate key/record |
| **ER_NO_SUCH_TABLE** | Referenced table does not exist |
| **ER_PARSE_ERROR** | Invalid SQL syntax |
| **ER_LOCK_WAIT_TIMEOUT** | Lock wait timeout exceeded |

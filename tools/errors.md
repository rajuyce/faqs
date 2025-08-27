
# 📋 Common Application Error Codes

This document summarizes key HTTP, database, system, and application-level error codes used in software systems.

---

## 🌐 HTTP Error Codes (Client & Server)

### 🔸 1xx – Informational
| Code | Description           | Explanation |
|------|-----------------------|-------------|
| 100  | Continue              | Initial part of request received, continue with rest. |
| 101  | Switching Protocols   | Server agrees to switch protocols (e.g., HTTP to WebSocket). |

### 🔹 2xx – Success
| Code | Description   | Explanation |
|------|---------------|-------------|
| 200  | OK            | Request successful. |
| 201  | Created       | Resource successfully created. |
| 204  | No Content    | Success, but no response body. |

### ⚠️ 3xx – Redirection
| Code | Description             | Explanation |
|------|-------------------------|-------------|
| 301  | Moved Permanently       | Resource has been permanently moved. |
| 302  | Found                   | Temporary redirect to another URI. |
| 304  | Not Modified            | Cached content is still valid. |

### ❗ 4xx – Client Errors
| Code | Description        | Explanation |
|------|--------------------|-------------|
| 400  | Bad Request        | Malformed or invalid request. |
| 401  | Unauthorized       | Requires authentication. |
| 403  | Forbidden          | Authenticated but not authorized. |
| 404  | Not Found          | Resource doesn’t exist. |
| 405  | Method Not Allowed | Unsupported HTTP method. |
| 408  | Request Timeout    | Client took too long. |
| 409  | Conflict           | State conflict (e.g., duplicate resource). |
| 429  | Too Many Requests  | Rate limiting hit. |

### 🔥 5xx – Server Errors
| Code | Description            | Explanation |
|------|------------------------|-------------|
| 500  | Internal Server Error  | Server-side error. |
| 501  | Not Implemented        | Feature not supported. |
| 502  | Bad Gateway            | Invalid upstream response. |
| 503  | Service Unavailable    | Server overloaded or under maintenance. |
| 504  | Gateway Timeout        | Upstream didn’t respond in time. |

---

## 🗄️ Database Error Codes

| Code               | Description                            |
|--------------------|----------------------------------------|
| ER_DUP_ENTRY        | Unique constraint violation.           |
| ER_NO_SUCH_TABLE    | Table doesn’t exist.                   |
| ER_PARSE_ERROR      | SQL syntax error.                      |
| ER_LOCK_WAIT_TIMEOUT| Transaction timed out waiting for lock.|
| ER_OUT_OF_MEMORY    | DB ran out of memory.                  |

---

## 💻 System-Level Error Codes (Linux/POSIX)

| Code | Symbol         | Description                |
|------|----------------|----------------------------|
| 1    | EPERM          | Operation not permitted    |
| 2    | ENOENT         | No such file or directory  |
| 13   | EACCES         | Permission denied          |
| 17   | EEXIST         | File already exists        |
| 28   | ENOSPC         | No space left on device    |
| 111  | ECONNREFUSED   | Connection refused         |
| 113  | EHOSTUNREACH   | No route to host           |

---

## 🔐 Authentication & Authorization Errors

| Code         | Type              | Description                           |
|--------------|-------------------|---------------------------------------|
| 401          | Unauthorized      | Missing or invalid credentials        |
| 403          | Forbidden         | Insufficient permissions              |
| JWT Expired  | InvalidTokenError | Token expired                         |

---

## 📦 Application-Specific Errors

| Code | Meaning                 | Notes                          |
|------|-------------------------|--------------------------------|
| 1001 | InvalidInputError       | Input validation failed        |
| 1002 | ResourceAlreadyExists   | Duplicate resource             |
| 1003 | PaymentFailed           | Transaction declined           |
| 1004 | QuotaExceeded           | Usage limit crossed            |

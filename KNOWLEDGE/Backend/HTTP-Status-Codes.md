# HTTP Status Codes

## 200 OK

The request was successful.

Example:

* Home page loads.
* ASCII art generated successfully.

---

## 400 Bad Request

The client sent invalid input.

Examples:

* Empty input.
* Invalid banner.
* Non-ASCII characters.

---

## 404 Not Found

The requested resource does not exist.

Example:

* `/wrong-page`

---

## 405 Method Not Allowed

The requested URL exists, but the HTTP method is incorrect.

Example:

* Sending a POST request to `/`.

---

## 500 Internal Server Error

The server failed while processing a valid request.

Examples:

* Missing banner file.
* Template execution failure.
* File system errors.

---

# Engineering Rule

Ask one question whenever an error occurs:

"Did the client cause this, or did the server?"

If the client caused it, return a 4xx status.

If the server caused it, return a 5xx status.
